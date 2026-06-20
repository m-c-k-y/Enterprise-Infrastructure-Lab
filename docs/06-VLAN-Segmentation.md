# VLAN Segmentation

### Objective:
   - Segment servers and clients into separate VLANs

---

<br>

**VLAN assignment:**

 - **VLAN10 (Clients) = *192.168.110.0/24***
 - **VLAN20 (Servers) = *192.168.120.0/24***

<br>

**VLANs will have a DHCP Address pool range of .100 - .199:**

> <img src="../images/vlan-segmentation/dhcp-range.png" width="500">


<br>

### 📝 Origianl plan:

Migrate the ***client machine to VLAN10*** followed by the ***Domain Controller to VLAN20***.

#### 🟥 PROBLEM:

Migrating the client machine to ***VLAN10*** resulted in loss of access to the ***pfSense browser UI*** from the client's browser:

> <img src="../images/vlan-segmentation/pfsense-lost-access.png" width="500">
>
#### 🟨 WHY:
> 
The client was now in a different subnet to pfSense but no firewall rule was created to allow traffic from that subnet.
> 
#### 🟩 SOLUTION:

Added ***temporary firewall rule*** to allow all traffic from the ***VLAN10 subnet***:

> <img src="../images/vlan-segmentation/allow-all-vlan10.png" width="500">

### 💡 Updated plan:

Migrate the ***Domain Controller to VLAN20*** followed by the ***client machine to VLAN10***.

> **WHY:**
> - Migrating the ***Domain Controller*** first will result in easier management of potential ***DNS/NTP*** issues between the ***client*** and ***DC***.
---
<br>

### <mark>Step 1</mark>: Move the Domain Controller to VLAN20:

<br>

**Firewall rules for VLAN20 to allow for DNS and NTP:**

> <img src="../images/vlan-segmentation/allow-dns-ntp-vlan20.png" width="500">

**Windows VLAN tag:**

> <img src="../images/vlan-segmentation/windows-vlan-tag.png" width="500">

**Request to obtain an IP address automatically from the DHCP server:**

> <img src="../images/vlan-segmentation/request-dhcp.png" width="500">
> 
> Why?:
> - *Introducing automation here will reduce the risk of user misinput that can happen when manually configuring IP addresses.*
> 
> **The Domain Controller obtained an IP address of 192.168.120.100:**
>> 
>> <img src="../images/vlan-segmentation/dc-obtained-dhcp.png" width="500">

**Set a static DHCP reservation for the Domain Controller using its MAC address:**

> <img src="../images/vlan-segmentation/static-dhcp-dc.png" width="500">
> 
> - **IP reserved for the Domain Controller:** *192.168.120.10*
> 
> **Why?:**
> 
> - *DHCP reservation was used to ensure the Domain Controller always receives the same IP address while still benefiting from centralized IP management.*

**Domain Controller's ipconfig:**

> <img src="../images/vlan-segmentation/dc-ipconfig.png" width="500">
---
<br>

### <mark>Step 2</mark>: Move clients to VLAN10:

<br>

**Note:** *Most of the migration process for VLAN10 followed the same procedure as VLAN20. Rehashed steps will not be shown.

<br>

**Firewall rules for VLAN10:**

> <img src="../images/vlan-segmentation/firewall-rules-vlan10.png" width="500">
> - **Rule 1: *Allow required AD services from VLAN10 to the Domain Controller***
> - **Rule 2: *Allow pfSense browser UI access from VLAN10***
> - **Rule 3: *Block traffic to all other VLAN20 addresses***
> - **Rule 4: *Block all other traffic from VLAN10***

**The Domain Controller's address was specified as the DNS server in the DHCP config, in order to automate DNS server assignment for clients:**

> <img src="../images/vlan-segmentation/dhcp-dns.png" width="500">

**Client's ipconfig:**

> <img src="../images/vlan-segmentation/client-ipconfig.png" width="500">


