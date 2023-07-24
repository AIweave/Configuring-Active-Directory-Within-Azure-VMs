<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Deploying Active Directory Within the cloud (Azure)</h1>

This tutorial outlines the implementation of Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- [Microsoft Azure (Virtual Machines)](https://azure.microsoft.com/en-us/free/search/?ef_id=_k_Cj0KCQjwn_OlBhDhARIsAG2y6zP4dj0GTUbQZfgBzQwT0oEX3HE2sFzljRNaK8gSsTL7Rqxnb98bYOoaAp-hEALw_wcB_k_&OCID=AIDcmm5edswduu_SEM__k_Cj0KCQjwn_OlBhDhARIsAG2y6zP4dj0GTUbQZfgBzQwT0oEX3HE2sFzljRNaK8gSsTL7Rqxnb98bYOoaAp-hEALw_wcB_k_&gad=1&gclid=Cj0KCQjwn_OlBhDhARIsAG2y6zP4dj0GTUbQZfgBzQwT0oEX3HE2sFzljRNaK8gSsTL7Rqxnb98bYOoaAp-hEALw_wcB)
- Microsoft Remote Desktop
- Active Directory Domain Services

<h2>Operating Systems Used </h2>

- Windows Server 2022 (Domain Contoller or DC)
- Windows 10 (Client)

<h2>Instructions</h2>

- **Part 1: Setup Resources in Azure**
- Create the Domain Controller VM (Windows Server 2022) named “DC”.
- Set Domain Controller’s NIC Private IP address to be "static" by clicking on the VM's name created in Azure. For an example, "DC" as shown below. 

![Screen Shot 2023-07-24 at 7 40 43 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/0bc5e565-81c2-4ee6-8ece-7d327f6ed075)

- Choose "Networking" in the left column of page. Then, "Network Interface" in the middle of screen.
  
![Screen Shot 2023-07-24 at 7 37 38 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/7bc760e0-0813-4bc4-9d8a-2b4fe11791d5)

- Select "IP Configurations" in the left column.

![Screen Shot 2023-07-24 at 7 38 09 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/353a3300-b534-49ea-9f53-6953d8aea16d)

- Select "Static" under "Assignment", then "Save".
  
![Screen Shot 2023-07-24 at 7 38 24 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/a2feb3a7-dffa-46d6-a85a-4ee85ba36e86)

- Create a Client VM (Windows 10) named “Client”. Use the same Resource Group and Vnet that was created with the DC.
- Ensure that both VMs are in the same Vnet.
- Ensure Connectivity between the client and Domain Controller by logging into Client with Remote Desktop and ping DC’s private IP address with ping -t <ip address> (perpetual ping).

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

This window should appear next.
    
![Screen Shot 2023-07-24 at 6 45 10 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/e44ed623-c8bf-4258-b2ec-8ce0280d3662)

- Add new administrator to the “Domain Admins” Security Group by right clicking their name.  Select "Properties". 

![Screen Shot 2023-07-24 at 6 48 02 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/26cc425f-b100-4432-81e7-c4ca8984da89)

- Click "Add". Type "Domain Admins" in the entry box. Apply by pressing "OK".  

![Screen Shot 2023-07-24 at 6 51 09 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/b1e10fa0-6cd2-48cf-925a-4ac3d8cddb42)

- Log out the Remote Desktop connection to DC and log back in as “mydomain.com\(admin username)”.

- **Part 2: Join Client to your domain (mydomain.com).**  
- From the Azure Portal, set Client's DNS settings to the DC’s Private IP address by copying and pasting DC's Private IP address to the Client's "DNS servers".

![Screen Shot 2023-07-24 at 7 00 02 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/bb0d8819-89df-437d-b144-f80443439eb2)

- From the Azure Portal, restart Client
- Login to Client (Remote Desktop) as the original local admin (username created in Azure) and join it to the domain (DC) by going to "Systems" in Windows.  Click "Rename this PC (advanced)".

![Screen Shot 2023-07-24 at 7 02 33 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/66f3f43b-c553-470e-be03-5a37efc8ab08)

- Choose "Change".
- On the next window, select " Domain" under "Member of" in order to type DC's domain name (ex.mydomain.com).
   
![Screen Shot 2023-07-24 at 7 02 50 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/06840733-9e66-47bd-aab2-7f72a46715da)

- If a windows appear asking for the creditials of the DC, enter it.

![Screen Shot 2023-07-24 at 7 03 18 PM](https://github.com/AIweave/Configuring-Active-Directory-Within-Azure-VMs/assets/121763338/6c46ff6a-6dd9-4b25-92bd-80281b798cd5)

Setup Complete.


