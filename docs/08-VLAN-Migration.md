 # VLAN Migration


### 🎯 Objective:
   - Successfully move the Domain Controller over to VLAN10 (192.168.110.0/24)

---

<br>

### <mark>Step 1</mark>: Prepare Proxmox and pfSense for VLAN migration:

<br>

**Made the Proxmox bridge VLAN-aware:**
>
> <img src="../images/vlan-migration/vlan-aware.png" width="600">

**Set the VLAN tag directly to the VM instead of the VirtIO adapter inside Windows:**
>
> <img src="../images/vlan-migration/vlan-tag.png" width="600">
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

> <img src="../images/vlan-migration/manual-dc-ip-vlan10.png" width="600">


#### 🟥 Problem #1:
Upon moving the **Domain Controller to VLAN10**, all reachibility to addresses outside of **VLAN10** was lost.

**Ping test from Domain Controller:**
>  
> <img src="../images/vlan-migration/failed-ping-from-vlan10.png" width="600">

#### 🟨 Theory:
Incoming traffic back to VLAN10 was getting dropped by the **ISP router** because it didn't know where **192.168.110.10 (Domain Controller's address)** was.

#### 🟩 Solution:
**Added static routes to the ISP router config:** 
>
> <img src="../images/vlan-migration/ISP-router-static-IP.png" width="600">
>
> - **192.168.20.100 =** VLAN99 SVI

<br>

#### 🟥 Problem #2:
**Using tracert, we see that traffic has no trouble reaching the ISP router, however after that, the traffic is dropped:**
>
> <img src="../images/vlan-migration/tracert-fail.png" width="600">

#### 🟨 Updated theory:
Since setting static routes for **VLAN10 and VLAN20** did NOT resolve the problem, the issue is likely occurring beyond the router. This may suggest that the ISP router is unable or unwilling to NAT traffic from VLAN10, preventing traffic from finding its way back to the Domain Controller.


### 🟩 Solution/Change of plan:
Unfortunately, this ISP router does **NOT** provide NAT setting that can be changed in order to resolve this issue. Because of this, **pfSense** will be reintroduced as an edge router while still having the switch handle all inter-VLAN routing.

---
<br>

### <mark>Step 3</mark>: Create and configure a transit VLAN:

<br>

#### Why a transit VLAN?:
> The switch and pfSense will now both be routing to each other, but both live in different subnets. **VLAN50 (Transit VLAN)** was created as a direct link for both routing devices to communicate.

<br>

### VLAN50 = 192.168.150.0/30
**Why /30?:**
> Only the **pfSense gateway (192.168.150.1)** and the **switch's VLAN50 SVI (192.168.150.2)** will be on this VLAN, meaning only 2 host addresses are needed.

<br>

**VLAN50 created:**

> <img src="../images/vlan-migration/VLAN50-created.png" width="600">

**Switch's VLAN50 SVI:**

> <img src="../images/vlan-migration/VLAN50-SVI.png" width="600">
	
**Switch's new default gateway (pfSense):**

> <img src="../images/vlan-migration/switch-default-gateway.png" width="600">


**Two NICs were added to the pfSense VM:**

> <img src="../images/vlan-migration/2nics-pfsenseVM.png" width="600">
> 
> - **net0** = WAN
> - **net1** = LAN (VLAN traffic)
> 
> **net0** was left without a VLAN tag since all untagged WAN traffic will be going through the **native VLAN (VLAN99)**.
> 
**pfSense WAN and LAN assignment:**

> <img src="../images/vlan-migration/pfsense-wan-lan.png" width="600">

---
<br>

### <mark>Step 4</mark>: Set up pfSense static routes and NAT rules:

<br>

**Temporary "Allow Any" rule added to the LAN firewall in pfSense:**

> <img src="../images/vlan-migration/allow-any-rule-vlan50.png" width="600">
>
> - This would ensure all traffic could cross over the **transit VLAN (VLAN50)**.

**Successful ping test from the switch to pfSense:**

> <img src="../images/vlan-migration/pingtest-switch-pfsense.png" width="600">


**Static routes added for VLAN10 and VLAN20:**

> <img src="../images/vlan-migration/static-routes-vlan10-vlan20.png" width="600">
>
> - Without these, pfSense wouldn't know where to send traffic to for either subnet.
	
**Outbound NAT rules created for both VLANs:**

> <img src="../images/vlan-migration/nat-rules-vlans.png" width="600">
>
> - This resolves the original issue by allowing pfSense to perform outbound NAT for the VLAN networks before forwarding traffic to the ISP router.

<br>

### 🟩 Result:

The **Domain Controller had been successfully migrated to VLAN10**, and traffic was now able to reach beyond the ISP router without being dropped.

**Successful DC ping test:**

> <img src="../images/vlan-migration/dc-ping-test-vlan10.png" width="600">

**Successful DC DNS test:**

> <img src="../images/vlan-migration/dc-dns-test-vlan10.png" width="600">

--- 
<br>

### <mark>Updated traffic flow</mark>:
<br>

> <img src="../images/vlan-migration/vlan50-traffic-flow.png" width="250">
