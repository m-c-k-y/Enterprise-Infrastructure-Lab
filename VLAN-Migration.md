 # VLAN Migration


### Goal:
   - Successfully move the Domain Controller over to VLAN10 (192.168.110.0/24)

---

<br>

### <mark>Step 1</mark>: Prepare Proxmox and pfSense for VLAN migration:

<br>

**Made the Proxmox bridge VLAN-aware:**
>
>![](images/vlan-aware.png)

**Set the VLAN tag directly to the VM instead of the VirtIO adapter inside Windows:**
>
>![](images/vlan-tag.png)
>	
>**Why?:**
> - This provides centralized management making it easier to troubleshoot potential issues in the future.
>
> - Users inside the VM cannot accidentally break VLAN configuration.
---
<br>

### <mark>Step 2</mark>: Start the migration process:

<br>

**Manually assigned the Domain Controller an address of 192.168.110.10:**

>![](images/manual-dc-ip-vlan10.png)


#### 🟥 Problem #1:
Upon moving **the Domain Controller** to **VLAN10**, all reachibility to addresses outside of VLAN10 was lost.

**Ping test from Domain Controller:**
>  
>![](images/failed-ping-from-vlan10.png)

#### 🟨 Theory:
Incoming traffic back to **VLAN10** was getting dropped by the **ISP router** because it didn't know where **192.168.110.10 (Domain Controller's address)** was.

#### 🟩 Solution:
**Added static routes to the ISP router config:** 
>
>![](images/ISP-router-static-IP.png)
> - 192.168.20.100 = VLAN99 SVI

<br>

#### 🟥 Problem #2:
**Using tracert, we see that traffic has no trouble reaching the ISP router, however after that, the traffic is dropped:**
>
>![](images/tracert-fail.png)

#### 🟨 Updated theory:
Since setting static routes for **VLAN10 and VLAN20** did NOT resolve the problem, the issue is likely occurring **beyond the router**. This may suggest that the **ISP router is unable or unwilling to NAT traffic** from VLAN10, preventing traffic from finding its way back to **the Domain Controller**.


### 🟩 Solution/Change of plan:
Unfortunately, this **ISP router does NOT provide NAT setting** that can be changed in order to resolve this issue. Because of this, **pfSense will be reintroduced as an edge router** while still having the switch handle all inter-VLAN routing.

---
<br>

### <mark>Step 3</mark>: Create and configure a transit VLAN:

<br>

#### Why a transit VLAN?:
> The **switch and pfSense will now both be routing to eachother**, but both live in different subnets. **VLAN50 (Transit VLAN)** was created as a direct link for both routing devices to communicate.

<br>

### VLAN50 = 192.168.150.0/30
**Why /30?:**
> Only the **pfSense gateway (192.168.150.1)** and the **switch's VLAN50 SVI (192.168.150.2)** will be on this VLAN, meaning **only 2 host addresses are needed**.

<br>

**VLAN50 created:**

> ![](images/VLAN50-created.png)

**Switch's VLAN50 SVI:**

> ![](images/VLAN50-SVI.png)
	
**Switch's new default gateway (pfSense):**

> ![](images/switch-default-gateway.png)


**Two NICs were added to the pfSense VM:**

> ![](images/2nics-pfsenseVM.png)
> 
> - **net0** = WAN
> - **net1** = LAN (VLAN traffic)
> 
> **net0 was left without a VLAN tag** since all **untagged WAN traffic** will be going through the **native VLAN (VLAN99)**.
> 
**pfSense WAN and LAN assignment:**

> ![](images/pfsense-wan-lan.png)

---
<br>

### <mark>Step 4</mark>: Set up pfSense static routes and NAT rules:

<br>

**Temporary "Allow Any" rule added to the LAN firewall in pfSense:**

> ![](images/allow-any-rule-vlan50.png)
> - This would ensure all traffic could cross over the **transit VLAN (VLAN50)**.

**Successful ping test from the switch to pfSense:**

> ![](images/pingtest-switch-pfsense.png)


**Static routes added for VLAN10 and VLAN20:**

> ![](images/static-routes-vlan10-vlan20.png)
> - Without these, **pfSense** wouldn't know where to send traffic to for either subnet.
	
**Outbound NAT rules created for both VLANs:**

> ![](images/nat-rules-vlans.png)
> - This resolves the original issue by allowing **pfSense to perform outbound NAT** for the **VLAN networks** before forwarding traffic to the **ISP router**.

<br>

### 🟩 Result:

The **Domain Controller had been successfully migrated to VLAN10**, and traffic was now able to reach beyond the ISP router without being dropped.

**Successful DC ping test:**

> ![](images/dc-ping-test-vlan10.png)

**Successful DC DNS test:**

> ![](images/dc-dns-test-vlan10.png)

--- 
<br>

### <mark>Updated traffic flow</mark>:

> ![](images/vlan50-traffic-flow.png)
