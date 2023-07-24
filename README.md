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
  - Type Active Directory Users and Computers (ADUC) in Windows search box located at the bottom of the screen.
 
    ![Screen Shot 2023-07-24 at 6 35 34 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/3fc97147-547c-4452-9c24-e324ebe3bce4)

  - Create the Organizational Units (OU) called “_EMPLOYEES” and "_ADMINS” by clicking on the Root Domain Name created from the Server Manager.  Then right click to create the "Organizational Unit“.
 
![Screen Shot 2023-07-24 at 6 36 21 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/6fc2cf05-bc0e-4b45-91e4-b7dc193e61ed)

![Screen Shot 2023-07-24 at 6 40 41 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/411dc7ed-5f35-4b8f-aacb-08a1674bbced)

  - Create an administrator to be used when connecting the Client to the DC by right clicking "_ADMINS" in the left column. Selecting:

    *New -> User*
    
![Screen Shot 2023-07-24 at 6 45 10 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/e44ed623-c8bf-4258-b2ec-8ce0280d3662)

  - Add new administrator to the “Domain Admins” Security Group by right clicking their name.  Select "Properties". 

    ![Screen Shot 2023-07-24 at 6 48 02 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/26cc425f-b100-4432-81e7-c4ca8984da89)

    - Click "Add". Type "Domain Admins" in the entry box. Apply by pressing "OK".  

![Screen Shot 2023-07-24 at 6 51 09 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/b1e10fa0-6cd2-48cf-925a-4ac3d8cddb42)

  - Log out the Remote Desktop connection to DC and log back in as “mydomain.com\(admin username)”.

- Join Client-1 to your domain (mydomain.com)
  - From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
  - From the Azure Portal, restart Client-1
  - Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
  - Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain
  - Create a new OU named “_CLIENTS” and drag Client-1 into there (Step is not really necessary, just for organizational purposes. I guess I skipped this in the lab!)


