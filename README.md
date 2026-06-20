# Enterprise Networking & Active Directory Homelab

#### Project status:

	🟢 Active Development
---

<br>

### CURRENT LAB DIAGRAM:

<br>

![](images/current-lab-diagram.png)

<br>

#### Note:
- This diagram will be updated as the project evolves.

---

## PROJECT OVERVIEW

This project is designed to simulate a real enterprise network environment with a goal of teaching core networking, system administration and troubleshooting skills. Rather than focus on individual technologies in isolation, I wanted to build an environment where multiple systems depended on one another, creating opportunities to troubleshoot real-world issues.

<br>

### Key Objectives

- Set up and configure server virtualization
- Build an Active Directory environment
- Separate users into different groups/departments
- Add VLAN segmentation for servers and client machines
- Implement and manage firewall rules
- Add a managed a layer 3 switch to handle inter-VLAN routing

### Technologies Used

- Optiplex 7060
- Proxmox
- Windows Server / Active Directory
- pfSense
- Cisco Catalyst 3560-X
- VLANs / Inter-VLAN Routing
- DNS / DHCP / NTP

### Documentation

- [Windows Server Installation](docs/01-Windows-Server-install.md)
- [AD DS Installation](docs/02-AD-DS-Installation.md)
- [NTP](docs/03-DC-Client-NTP.md)
- [Users, Groups, and Permissions](docs/04-Users-Groups-Permissions.md)
- [pfSense](docs/05-pfSense.md)
- [VLAN Segmentation](docs/06-VLAN-Segmentation.md)
- [Cisco Switch](docs/07-Cisco-Switch.md)
- [VLAN Migration](docs/08-VLAN-Migration.md)

