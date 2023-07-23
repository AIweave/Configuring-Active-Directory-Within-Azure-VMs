<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Deploying Active Directory Within the cloud (Azure)</h1>

This tutorial outlines the implementation of Active Directory within Azure Virtual Machines.<br />

**MUST KNOW** -  Due to the length of this process, it is recommended that the "Video Demonstration" is initially viewed as well as in combination with the instructions below. 

<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- [Microsoft Azure (Virtual Machines)](https://azure.microsoft.com/en-us/free/search/?ef_id=_k_Cj0KCQjwn_OlBhDhARIsAG2y6zP4dj0GTUbQZfgBzQwT0oEX3HE2sFzljRNaK8gSsTL7Rqxnb98bYOoaAp-hEALw_wcB_k_&OCID=AIDcmm5edswduu_SEM__k_Cj0KCQjwn_OlBhDhARIsAG2y6zP4dj0GTUbQZfgBzQwT0oEX3HE2sFzljRNaK8gSsTL7Rqxnb98bYOoaAp-hEALw_wcB_k_&gad=1&gclid=Cj0KCQjwn_OlBhDhARIsAG2y6zP4dj0GTUbQZfgBzQwT0oEX3HE2sFzljRNaK8gSsTL7Rqxnb98bYOoaAp-hEALw_wcB)
- Microsoft Remote Desktop
- Active Directory Domain Services

<h2>Operating Systems Used </h2>

- Windows Server 2022 (Domain Contoller or DC)
- Windows 10 (Client)

<h2>Instructions</h2>

- Setup Resources in Azure
  - Create the Domain Controller VM (Windows Server 2022) named “DC”
  - Set Domain Controller’s NIC Private IP address to be "static"
  - Create a Client VM (Windows 10) named “Client”. Use the same Resource Group and Vnet that was created with the DC
  - Ensure that both VMs are in the same Vnet
    
- Ensure Connectivity between the client and Domain Controller
  - Login to Client with Remote Desktop and ping DC’s private IP address with ping -t <ip address> (perpetual ping)
  - If this the command fails, login to the Domain Controller and enable ICMPv4 in on the local windows Firewall
  - Check back at Client-1 to see the ping succeed

- Install Active Directory
  - Login to DC-1 and install Active Directory Domain Services
  - Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
  - Restart and then log back into DC-1 as user: mydomain.com\labuser

- Create an Admin and Normal User Account in AD
  - In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
  - Create a new OU named “_ADMINS”
  - Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”
  - Add jane_admin to the “Domain Admins” Security Group
  - Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
  - User jane_admin as your admin account from now on


- Join Client-1 to your domain (mydomain.com)
  - From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
  - From the Azure Portal, restart Client-1
  - Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
  - Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain
  - Create a new OU named “_CLIENTS” and drag Client-1 into there (Step is not really necessary, just for organizational purposes. I guess I skipped this in the lab!)


- Setup Remote Desktop for non-administrative users on Client-1
  - Log into Client-1 as mydomain.com\jane_admin and open system properties
  - Click “Remote Desktop”
  - Allow “domain users” access to remote desktop
  - You can now log into Client-1 as a normal, non-administrative user now


