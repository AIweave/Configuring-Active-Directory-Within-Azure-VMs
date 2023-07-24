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

 ![Screen Shot 2023-07-24 at 6 00 27 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/19836aba-f6e5-4745-9d36-7814bf6c3ec4)

  - If this the command fails, login to the Domain Controller and enable ICMPv4 in on the local windows Firewall by following below:

    *Inbound Rules -> Enable ICMPv4(s) (Protocols) by right clicking mouse for pop-up*
 
![Screen Shot 2023-07-24 at 5 59 21 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/25a3eed3-4480-4bc5-9d47-e13acba24680)

![Screen Shot 2023-07-24 at 6 04 34 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/7fefa1f5-b8d3-404a-8f0c-028094e786b5)

  - Check back at Client-1 to see the ping succeed. "Reply means success".

![Screen Shot 2023-07-24 at 6 05 08 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/b8d22ce6-7cab-4c0a-b852-8df0cfe39507)


- Install Active Directory
  - Login to DC-1 and install Active Directory Domain Services by selecting "Add Roles and features" in Server Manager Dashboard.
 
![Screen Shot 2023-07-24 at 6 07 44 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/710d255c-3786-4747-997f-5dd99d508d71)

  - Select "Active Directory Domain Services" under "Roles" drop list.

![Screen Shot 2023-07-24 at 6 08 39 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/d65e0aae-7b8d-4e6b-b4fa-e4bd2421fa3b)

  - Promote server manager to a domain controller by clicking the yellow flag on the top right side of screen.  When the pop-up window appears, select "Promote this server to a domain controller".

  ![Screen Shot 2023-07-24 at 6 13 09 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/6ec0f580-eca3-42c5-9632-209ccd860593)

  - Setup a new forest. Create a "Root domain name" and "Password".

![Screen Shot 2023-07-24 at 6 14 13 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/4f51748d-5190-41fa-bdda-b892f070955d)

  - Restart and then log back into DC using Root domain name\user as User login and its password previously created for Password. For an example, username: mydomain.com\labuser.

![Screen Shot 2023-07-24 at 6 19 54 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/4be164f2-e50b-42f7-affe-74cbdebe6df5)


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


