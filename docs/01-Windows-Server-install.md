# Windows Server Install + Set Up


### Goal:
   - **Create and configure a Windows Server** virtual machine that will take the role of the **Domain Controller** in the **Active Directory environment**.

---

<br>

### <mark>Step 1</mark>: Create virtual machine and attach Windows Server ISO:

<br>

**VM Name:**
> <img src="../images/windows-server-setup/DC-VM-name.png" width="600">
>
> - **DC01 was chosen as the name because this machine would become the *Domain Controller* for the *Active Directory* environment.**

**OS:**
> <img src="../images/windows-server-setup/ISOattach.png" width="600">
>
> - **ISO image:** Windows Server 2022

**Disk:** 
> <img src="../images/windows-server-setup/VMdisk.png" width="600">
> 
> - **Disk size:** 40GB

 **CPU:** 
> <img src="../images/windows-server-setup/CPU-cores.png" width="600">
> 
> - **Cores:** 2
> - **Type:** host

**Memory:**
> <img src="../images/windows-server-setup/serverVM-memory.png" width="600">
> 
> - **MiB:** 4GB

**Network:**
> <img src="../images/windows-server-setup/VMnetwork.png" width="600">
> 
> - **Bridge:** vmbr0
> - **Virtual network card:** VirtIO
---

<br>

### <mark>Step 2</mark>: Attach VirtIO ISO and load storage driver:

<br>

#### VirtIO:
   - **VirtIO** is a set of drivers designed for virtual machines. It allows the VM to communicate more efficiently with Proxmox than traditional emulated hardware, resulting in better performance. The downside is that Windows Server does not recognize it out of the box, so the **VirtIO driver ISO** will have to be attached during installation.

<br>

**Attach VirtIO ISO:**
> <img src="../images/windows-server-setup/virtio-attach.png" width="600">
	   
**Load VirtIO storage driver:**
> <img src="../images/windows-server-setup/loaddriver.png" width="600">
> 
**Windows Server could not detect a virtual disk because the VirtIO storage drivers are not included by default.**

> <img src="../images/windows-server-setup/virtiodrive.png" width="600">
>
> <img src="../images/windows-server-setup/vioscsi1.png" width="200"><img src="images/windows-server-setup/2k22.png" width="200"><img src="images/windows-server-setup/amd64.png" width="200">
> 
> <img src="../images/windows-server-setup/unallocated-space.png" width="600">
> 
> - **After loading the VirtIO storage driver from the attached VirtIO ISO, Windows Server was able to detect the virtual disk and installation could proceed as normal.**

<br>

### <mark>Step 3</mark>: Initial Windows Server configuration:

<br>

**The Server was renamed to DC01 to identify it as the first Domain Controller in the environment:**
> <img src="../images/windows-server-setup/dcnamechange.png" width="600">

**Install VirtIO network driver:**
> <img src="../images/windows-server-setup/updatenetworkdriver.png" width="600">
> 
> <img src="../images/windows-server-setup/networkdriver1.png" width="200"><img src="images/windows-server-setup/networkdriver2.png" width="200"><img src="images/windows-server-setup/networkdriver3.png" width="200">
> 

	   
**VirtIO network driver successfully installed:**
> <img src="../images/windows-server-setup/networkdriversuccess.png" width="600">

<br>

### <mark>Step 4</mark>: Configure Static IP:

<br>

**Domain Controller static IP: *192.168.50.10***

**Subnet: *192.168.50.0/24***
> <img src="../images/windows-server-setup/staticIP1.png" width="600">

**DNS server: *192.168.50.10 (Domain Controller itself)***
> <img src="../images/windows-server-setup/dnsserverDC.png" width="600">
> 
> - **The Domain Controller was configured to use itself as its DNS server, as Active Directory relies on DNS for service discovery and authentication. External DNS requests would later be forwarded upstream as required.**

**Verification:**
> <img src="../images/windows-server-setup/DC-Verification.png" width="600">
> 
> - **The network configuration was verified using *ipconfig /all.***
	   
