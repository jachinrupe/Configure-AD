<p align="center">
<img src="https://i.imgur.com/7xLtdix.png" alt="osTicket logo"/>
</p>

<h1>Azure Installing an Active Directory.</h1>
This tutorial outlines how to install an active directory using azure.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- Windows Firewall

<h2>Operating Systems Used </h2>

- Windows 10</b> (21H2)
- Windows Server 2022</b> (21H2)

<h2>List of Prerequisites</h2>

- Microsoft Azure Free Account
- Understanding of Windows Ost
- Understanding how a domain controller interacts with a client.
- Understanding of the difference between dynamic and static DNS assignments.

  Setup Resources in Azure
Create the Domain Controller VM (Windows Server 2022) named “DC-1”
![Screenshot 2023-11-03 140600](https://github.com/jachinrupe/Configure-AD/assets/149485790/9fcd28c0-1db9-497f-929f-d5883c264f09)
Take note of the Resource Group and Virtual Network (Vnet) that get created at this time
![Screenshot 2023-11-03 140726](https://github.com/jachinrupe/Configure-AD/assets/149485790/fd56c528-8a91-4216-bae2-9765bf4a9926)
Set Domain Controller’s NIC Private IP address to be static
![Screenshot 2023-11-03 140840](https://github.com/jachinrupe/Configure-AD/assets/149485790/242212f3-03ef-435d-9896-18f68f2af8e0)
![Screenshot 2023-11-03 140853](https://github.com/jachinrupe/Configure-AD/assets/149485790/0fbfbcb2-4543-43a8-9b6f-e5db8f27d8dd)


Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.a
![Screenshot 2023-11-03 141204](https://github.com/jachinrupe/Configure-AD/assets/149485790/86984bcd-cdc1-4aed-ace9-7c7dfdcc3a26)
Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher
![Screenshot 2023-11-03 142744](https://github.com/jachinrupe/Configure-AD/assets/149485790/c13e7f8a-9ac3-46a5-8fbc-493d7caa2d35)

Ensure Connectivity between the client and Domain Controller
Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t <ip address> (perpetual ping)
![Screenshot 2023-11-03 142924](https://github.com/jachinrupe/Configure-AD/assets/149485790/fde962d1-589d-4aa5-8583-0ef109b6bfc3)
Notice the request timeout
![Screenshot 2023-11-03 143322](https://github.com/jachinrupe/Configure-AD/assets/149485790/e9083756-5d28-4573-80ad-ec1c6e7c0112)
Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall
![Screenshot 2023-11-03 143607](https://github.com/jachinrupe/Configure-AD/assets/149485790/36ef801d-ffeb-40c1-a8a2-eb66af9e9749)
![Screenshot 2023-11-03 143734](https://github.com/jachinrupe/Configure-AD/assets/149485790/db62fa41-d097-4d3a-8060-91a41240b8b2)
![Screenshot 2023-11-03 143754](https://github.com/jachinrupe/Configure-AD/assets/149485790/c01946cf-4223-4b1b-bc53-b487f7885279)

Check back at Client-1 to see the ping succeed
![Screenshot 2023-11-03 144629](https://github.com/jachinrupe/Configure-AD/assets/149485790/7278e9a3-e276-4353-affb-a0847f24beba)

Install Active Directory
Login to DC-1 and install Active Directory Domain Services
![Screenshot 2023-11-03 144804](https://github.com/jachinrupe/Configure-AD/assets/149485790/1ee390e2-2926-443e-84b6-077f9d2016c8)
![Screenshot 2023-11-03 144825](https://github.com/jachinrupe/Configure-AD/assets/149485790/56f4b8ac-51c8-4ddd-a569-d64fe5ed3fa8)

Promote as a DC: Setup a new forest as mydomain.com (can be anything, just remember what it is)
![Screenshot 2023-11-03 145306](https://github.com/jachinrupe/Configure-AD/assets/149485790/55d18a9a-56e4-45b7-8809-7d397db9b472)
![Screenshot 2023-11-03 145335](https://github.com/jachinrupe/Configure-AD/assets/149485790/b135aaf1-c68d-4e87-825d-72742ba1965f)
![Screenshot 2023-11-03 145438](https://github.com/jachinrupe/Configure-AD/assets/149485790/36db2efa-2642-41d6-bf24-3cedd194c014)

Restart and then log back into DC-1 as user: mydomain.com\labuser
![Screenshot 2023-11-03 151155](https://github.com/jachinrupe/Configure-AD/assets/149485790/c6c55ffb-ab8f-468b-ad66-b4d52da60ae9)

Create an Admin and Normal User Account in AD
![Screenshot 2023-11-03 151403](https://github.com/jachinrupe/Configure-AD/assets/149485790/5d6d1291-cca9-4b45-bf66-80c70c0cb061)
In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”
![Screenshot 2023-11-03 151508](https://github.com/jachinrupe/Configure-AD/assets/149485790/381435f3-840c-45df-9922-1f4d4ccff3ab)
Create a new OU named “_ADMINS”
![Screenshot 2023-11-03 151548](https://github.com/jachinrupe/Configure-AD/assets/149485790/a1e3a010-29f3-458f-a34b-c2f96994799b)
Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”

Add jane_admin to the “Domain Admins” Security Group
![Screenshot 2023-11-03 151634](https://github.com/jachinrupe/Configure-AD/assets/149485790/4572dd10-5429-4bd2-a89e-0bd90c181720)
![Screenshot 2023-11-03 151655](https://github.com/jachinrupe/Configure-AD/assets/149485790/8e7341c1-740e-4705-8fa4-d7e0f995bc6a)
![Screenshot 2023-11-03 151717](https://github.com/jachinrupe/Configure-AD/assets/149485790/62d6606b-9447-4437-b3ae-1521b5d56997)
Log out/close the Remote Desktop connection to DC-1 and log back in as “mydomain.com\jane_admin”
User jane_admin as your admin account from now on

Join Client-1 to your domain (mydomain.com)
From the Azure Portal, set Client-1’s DNS settings to the DC’s Private IP address
![Screenshot 2023-11-03 152504](https://github.com/jachinrupe/Configure-AD/assets/149485790/06795b84-d7b8-48c5-bfe7-d119f1740341)
![Screenshot 2023-11-03 152603](https://github.com/jachinrupe/Configure-AD/assets/149485790/10f65682-bbb6-4f5c-af75-74fd8cbbbbfe)

From the Azure Portal, restart Client-1
Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
![Screenshot 2023-11-05 124024](https://github.com/jachinrupe/Configure-AD/assets/149485790/a995a30f-6879-4476-b8b7-bee6df6c34c5)


Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain
![Screenshot 2023-11-05 124436](https://github.com/jachinrupe/Configure-AD/assets/149485790/139b9ced-7b2b-4dc8-aecd-b5d9d23b18da)

Create a new OU named “_CLIENTS” and drag Client-1 into there (Step is not really necessary, just for organizational purposes. I guess I skipped this in the lab!)
![Screenshot 2023-11-05 124539](https://github.com/jachinrupe/Configure-AD/assets/149485790/90628def-c216-4c44-af57-da2c7dc36a3b)

Setup Remote Desktop for non-administrative users on Client-1
Log into Client-1 as mydomain.com\jane_admin and open system properties
Click “Remote Desktop”
![Screenshot 2023-11-05 125007](https://github.com/jachinrupe/Configure-AD/assets/149485790/7cb3ada4-89f8-49da-9809-7b8cb82d03e8)
Allow “domain users” access to remote desktop
![Screenshot 2023-11-05 125057](https://github.com/jachinrupe/Configure-AD/assets/149485790/cbfe8943-e1d3-401a-adde-c9d33a6b3c64)
![Screenshot 2023-11-05 125106](https://github.com/jachinrupe/Configure-AD/assets/149485790/d441e78b-55f4-4f85-878a-1748ed521dae)

You can now log into Client-1 as a normal, non-administrative user now

Create a bunch of additional users and attempt to log into client-1 with one of the users
Login to DC-1 as jane_admin
Open PowerShell_ise as an administrator
![Screenshot 2023-11-05 125613](https://github.com/jachinrupe/Configure-AD/assets/149485790/42207551-e4fa-45d3-9a7a-092fcb8e6147)
Create a new File and paste the contents of the script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)
Run the script and observe the accounts being created
![Screenshot 2023-11-05 125817](https://github.com/jachinrupe/Configure-AD/assets/149485790/f36dd540-7681-4f04-a095-112531b55427)

When finished, open ADUC and observe the accounts in the appropriate OU
attempt to log into Client-1 with one of the accounts (take note of the password in the script)
![Screenshot 2023-11-05 131526](https://github.com/jachinrupe/Configure-AD/assets/149485790/950fa05b-ec02-4b24-9ab7-c0dbbf89396e)




