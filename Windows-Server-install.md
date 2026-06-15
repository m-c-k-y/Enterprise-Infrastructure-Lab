# Windows Server Install + Set Up


### Goal:
   - Create and configure a ***Windows Server*** virtual machine that will take the role of the ***Domain Controller*** in the ***Active Directory*** environment.

---

<br>

### <mark>Step 1</mark>: Create virtual machine and attach Windows Server ISO:

<br>

**VM Name:** DC01
> ![](images/windows-server-setup/DC-VM-name.png)
>
> - **DC01 was chosen because this machine would become the *Domain Controller* for the *Active Directory* environment.**

**OS:**
> ![](images/windows-server-setup/ISOattach.png)
>
> - **ISO image:** Windows Server 2022

**Disk:** 
> ![](images/windows-server-setup/VMdisk.png)
> 
> - **Disk size:** 40GB

 **CPU:** 
> ![](images/windows-server-setup/CPU-cores.png)
> 
> - **Cores:** 2
> - **Type:** host

**Memory:**
> ![](images/windows-server-setup/serverVM-memory.png)
> 
> - **MiB:** 4GB

**Network:**
> ![](images/windows-server-setup/VMnetwork.png)
> 
> - **Bridge:** vmbr0
> - **Virtual network card:** VirtIO

<br>

### <mark>Step 2</mark>: Attach VirtIO ISO and load storage driver:

<br>

#### VirtIO:
   - **VirtIO** is a set of drivers designed for virtual machines. It allows the VM to communicate more efficiently with Proxmox than traditional emulated hardware, resulting in better performance. The downside is that **Windows Server does not recognize it out of the box**, so the **VirtIO driver ISO** will have to be attached during installation.

<br>

**Attach VirtIO ISO:**
> ![](images/windows-server-setup/virtio-attach.png)
	   
**Load VirtIO storage driver:**
> ![](images/windows-server-setup/loaddriver.png)
> 
**Windows Server could not detect a virtual disk because the VirtIO storage drivers are not included by default.**

> ![](images/windows-server-setup/virtiodrive.png)
>
> ![](images/windows-server-setup/vioscsi1.png)![](images/windows-server-setup/2k22.png)![](images/windows-server-setup/amd64.png)
> 
> ![](images/windows-server-setup/unallocated-space.png)
> 
> - **After loading the VirtIO storage driver from the attached VirtIO ISO, Windows Server was able to detect the virtual disk and installation could proceed as normal.**

<br>

### <mark>Step 3</mark>: Initial Windows Server configuration:

<br>

**The Server was renamed to DC01 to identify it as the first Domain Controller in the environment:**
> ![](images/windows-server-setup/dcnamechange.png)

**Install VirtIO network driver:**
> ![](images/windows-server-setup/updatenetworkdriver.png)
> 
> ![](images/windows-server-setup/networkdriver1.png)
> 
> ![](images/windows-server-setup/networkdriver2.png)
> 
> ![](images/windows-server-setup/networkdriver3.png)
	   
**VirtIO network driver successfully installed:**
> ![](images/windows-server-setup/networkdriversuccess.png)

<br>

### <mark>Step 4</mark>: Configure Static IP:

<br>

**Domain Controller static IP: *192.168.50.10***

**Subnet: *192.168.50.0/24***
> ![](images/windows-server-setup/staticIP1.png)

**DNS server: *192.168.50.10 (Domain Controller itself)***
> ![](images/windows-server-setup/dnsserverDC.png)
> 
> - **The Domain Controller was configured to use itself as its DNS server, as Active Directory relies on DNS for service discovery and authentication. External DNS requests would later be forwarded upstream as required.**

**Verification:**
> ![](images/windows-server-setup/DC-Verification.png)
> 
> - **The network configuration was verified using *ipconfig /all.***
	   
