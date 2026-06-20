# Active Directory Domain Service (AD DS) installation

### 🎯 Objective: 
   - Install **Active Directory Domain Services (AD DS)** and promote DC01 to a Domain Controller.

---

<br>

### <mark>Step 1</mark>: Install Active Directory Domain Services (AD DS):

<br>

> <img src="../images/adds-install/add-roles-and-feature.png" width="600">
>
> <img src="../images/adds-install/role-based-install.png" width="600">

**Select *"Active Directory Domain Services"* from the *Server Roles* list:**
>
> <img src="../images/adds-install/serverrole.png" width="600">
   
   
**Install selected Tools and Services:** 
>
> <img src="../images/adds-install/addsinstall.png" width="600">
---

<br>

### <mark>Step 2</mark>: Promote the server to a Domain Controller:

<br>

**After the installation process is complete, we are able to promote the server to a *Domain Controller*:**
>
> <img src="../images/adds-install/promotetoDC.png" width="600">

**A new forest was created because this was the first *Domain Controller* within a new *Active Directory* environment:**
>
> <img src="../images/adds-install/addnewforest.png" width="600">

**Select the functional level and capabilities of the *Domain Controller*:**
>
> <img src="../images/adds-install/domainoptions.png" width="600">
>
> - ***Functional level*** refers to the available features throughout the Active Directory environment. Since the environment uses ***Windows Server 2022***, I selected the highest available level at the time (***Windows Server 2016***).
   
**Since a new forest was created within an isolated homelab environment rather than an existing DNS infrastructure, Microsoft wasn't able to create a *DNS delegation*, as there was no DNS zone above it:** 
>
> <img src="../images/adds-install/delegationcannotbefound2.png" width="600">
>
> - ***DNS delegation*** was skipped by leaving it unchecked.

**Automatically assigned *NetBIOS* name:**
>
> <img src="../images/adds-install/netbiosname.png" width="600">
>
> - The name ***"LAB"*** was automatically generated from the domain name (***lab.local***) and was left unchanged.
   
**After successfully promoting to a *Domain Controller*, the server initiated an automatic restart:**  
>
> <img src="../images/adds-install/addsrestart.png" width="600">

**Post-reboot verification of *AD DS + DNS* install and *Domain Controller* promotion:**
>
> <img src="../images/adds-install/domainconfirmation.png" width="600">
   
