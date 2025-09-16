<p align="center">
<img width="624" height="426" alt="image" src="https://github.com/user-attachments/assets/a3047667-9805-4b14-9fd9-4d35355b7320" />



</p>

<h1> Configuring On-premise Active Directory within Azure VMs</h1>
This tutorial outlines the implementation of on-premise Active Directory within Azure Virtual Machines.<br />





<h2>Environments and Technologies Used</h2>

- Remote Desktop
- Microsoft Azure (VMs/Compute)
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows 10 (21H2)
- Window Server 2022

<h2>High-Level Deployment and Configuration Steps</h2>


<h2>Installation Walkthrough</h2>

<p>
<img width="900" height="796" alt="Screenshot 2025-09-01 140308" src="https://github.com/user-attachments/assets/9e6fc229-bddf-42f9-900e-e60ad0e7d271" />

</p>
<p>
The goal of this tutorial is show how the client VM in Azure can join an Active Directory domain by using the Domain Controller as its DNS server inside the same virtual network.
</p>
<br />

<p>
<img width="1138" height="1254" alt="Screenshot 2025-09-01 152120" src="https://github.com/user-attachments/assets/306e4754-c66a-482d-bf10-85454b43262f" />

</p>
<p>
First, create a resource group--> Make sure it is on the correct subscription--> create a name (for this tutorial, Active-Directory-Lab)--> region, East US 2)--> review and create.
</p>
<br />

<p>
<img width="1138" height="1254" alt="Screenshot 2025-09-01 152120" src="https://github.com/user-attachments/assets/d734b413-85fe-438b-814d-08d40273704e" />

</p>
<p>
Create a Virtual network--> Resource group, Active-Directory-Lab--> Virtual Network Name,   Active-Directory-VNet--> region, East US 2--> review and create.
</p>
<br />

<p>
<img width="1942" height="484" alt="Screenshot 2025-09-01 154834" src="https://github.com/user-attachments/assets/b58b132f-445e-4477-9e53-78a6ed7518e1" />

</p>
<p>
Next, we are going to create 2 virtual machines. First the domain controller VM. Click create--> resource group, Active-Directory-Lab--> VM name, DC-1--> region, East US 2--> Windows Server 2022 Datacenter: Azure Edition Hotpatch- x64 Gen2--> size, Standard_D2s_v3- 2vcpus, 8GiB memory--> Administrator account--> create username and password--> Check the licensing box--> Next for disk--> make sure the virtual nextwork recently created is selected--> review and create then click create.
</p>
<br />

<p>
<img width="1952" height="636" alt="Screenshot 2025-09-01 160024" src="https://github.com/user-attachments/assets/71e7ac0b-b53c-4904-bba5-ce3ca26ab747" />

</p>
<p>
Next, we are going to create the client VM. Click create--> resource group, Active-Directory-Lab--> VM name, Client-1--> region, East US 2--> Windows 10 Pro, version 22H2- x64 Gen2--> size, Standard_D2s_v3- 2vcpus, 8GiB memory--> Administrator account--> create a username and password --> Check the licensing box--> Next for disk--> make sure the virtual nextwork recently created is selected--> review and create then click create.
</p>
<br />

<p>
<img width="1900" height="1227" alt="Screenshot 2025-09-01 160730" src="https://github.com/user-attachments/assets/028a3927-eec8-45e7-af54-980e48f3cad6" />
<img width="1906" height="1248" alt="Screenshot 2025-09-01 161104" src="https://github.com/user-attachments/assets/5c48615b-021e-40ff-a84f-6b306a8f6a49" />


</p>
<p>
Next, we are going to set DC-1 VM virtual NIC to static so the IP address does not change. We are going to tell Client-1 to use DC-1 as the DNS server. Also, we need to manually configure the DNS settings for Client-1 to use DC-1 IP address.

Click on DC-1 VM--> network settings--> Network Interface/IP Configuration-->IP Configurations--> ipconfig1--> Private IP address setting, static--> click save.
  
</p>
<br />

<p>
<img width="1898" height="1106" alt="Screenshot 2025-09-01 161621" src="https://github.com/user-attachments/assets/7b54f25d-756b-493a-b835-8ab04efc9dd6" />

</p>
<p>
Next, we are going to log into DC-1 via remote desktop conneciton. We are going to disable the firewall to test connectivity--> Locate the IP address on the VM page and scrolling right on the VM or clicking the VM and locating it there in the overview tab--> Open remote desktop connection and copy and past the IP address and click connect--> enter the username and password we created when creating the VM and click ok. You should be able to log into the VM.
</p>
<br />

<p>
<img width="1446" height="1080" alt="image" src="https://github.com/user-attachments/assets/a010feda-38ac-41f1-a36e-ae059ff995df" />

</p>
<p>
To disable the firewall, go to the start menu and click run--> type wf.msc--> click Windows Defender Firewall--> under firewall state, select off and do the same thing for public and private profile--> click apply after.
</p>
<br />

<p>
<img width="1824" height="1144" alt="Screenshot 2025-09-01 165143" src="https://github.com/user-attachments/assets/2be43084-4e8e-49a3-8432-03b2967c3407" />
<img width="1864" height="1234" alt="Screenshot 2025-09-01 165329" src="https://github.com/user-attachments/assets/c628c938-8b64-4d42-8989-dd5cf32acc28" />

</p>
<p>
After the VM is created, set Client-1's DNS settings to DC-1's Private IP address. Click on DC-1 VM and located the IP address--> Go to Client-1 VM--> click networking--> network settings--> click on the Network Interface--> DNS servers, Custom and under DNS server, type in DC-1's private IP address--> click save.
</p>
<br />

<p>
<img width="1870" height="732" alt="Screenshot 2025-09-01 170316" src="https://github.com/user-attachments/assets/1a791989-03c4-47f0-89d8-a55feb28181a" />

</p>
<p>
Next, we have to restart Client-1--> go to the VM page and check the box of Client-1 and click restart--> After it restarts, Login to Client-1 and attempt to ping DC-1’s private IP address to ensure the ping succeeded.


</p>
<br />

<p>
<img width="1892" height="880" alt="Screenshot 2025-09-01 170653" src="https://github.com/user-attachments/assets/31227131-3671-4e1c-915b-59b0b9e7ba52" />
<img width="1350" height="454" alt="Screenshot 2025-09-01 172502" src="https://github.com/user-attachments/assets/c11e1053-527d-41db-9975-4b19bb48ba64" />
</p>
<p>
Once logged on into Client-1, obtain the private address of DC-1--> go back to Client-1 and click start and search Powershell and open it. type ping 10.0.0.4 and it should ping. If it says "timeout" it means something went wrong. 
</p>
<br />

<p>
<img width="1204" height="1154" alt="Screenshot 2025-09-01 172640" src="https://github.com/user-attachments/assets/5a737058-77b2-4387-9942-e872cee50180" />


</p>
<p>
When you type in ipconfig /all, you will be able to see information pertaining to DC-1 including the private IP address and DNS servers.
</p>
<br />

<p>
<img width="2566" height="1318" alt="Screenshot 2025-09-02 234707" src="https://github.com/user-attachments/assets/0c087a92-dab4-4aaa-8d53-1395e9554582" />
  <img width="1552" height="1034" alt="Screenshot 2025-09-02 234856" src="https://github.com/user-attachments/assets/782b6684-9ace-4977-82d6-df1ce724d3c6" />

</p>
<p>
For this next section, we are going to install Active Directory--> Log into DC-1 and install Active Directory Domain Services--> click start--> server manager--> add roles and features--> click next until server roles--> check Active Directory domain services, add features--> click next until confirmation, check box for restarting computer and click install.
</p>
<br />

<p>          
<img width="1848" height="818" alt="Screenshot 2025-09-02 235255" src="https://github.com/user-attachments/assets/0ed1efd8-96c6-47df-a87d-da2eccf27301" />
<img width="1528" height="1118" alt="Screenshot 2025-09-02 235431" src="https://github.com/user-attachments/assets/98744b4c-8aae-4b0b-9be9-232f78daa1d9" />


</p>
<p>
Back to Server Manager in DC-1, we are going to promote as a domain controller--> click "promote this server to a domain controller--> add a new forest--> under root domain name--> put mydomain.com (can be any name, just remember it)--> click next until the installtion process and once finished, the computer will restart.
  After, we have to log back in to DC-1 as a user: mydomain.com\(username) make sure you use the correct backslash. Since now DC-1 is a domain controller now, the login has to be more specific.
</p>
<br />

<p>
<img width="1994" height="1052" alt="Screenshot 2025-09-03 001210" src="https://github.com/user-attachments/assets/e4e960b9-d050-485a-abdf-1cdfd8017c8a" />
<img width="1990" height="1052" alt="Screenshot 2025-09-03 001346" src="https://github.com/user-attachments/assets/e9ee0fdb-3a43-4bde-a24e-f10b780d05c2" />
<img width="862" height="742" alt="Screenshot 2025-09-03 001450" src="https://github.com/user-attachments/assets/4222bb0e-01ea-4ca6-a766-e675ef240598" />
<img width="392" height="1034" alt="Screenshot 2025-09-03 001628" src="https://github.com/user-attachments/assets/0499bd93-f5a7-4722-a124-08c3ebd6bf64" />


</p>
<p>
After logging in DC-1 as a domain controller, we are going to create a Domain Admin user within the domain. In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU)--> click start--> windows admistrative tools--> active directory users and computers, on here, right click mydomain.com--> new--> organizational unit, name it (_EMPLOYEES), repeat and create (_ADMINS) make sure not to mess this part up
</p>
<br />

<p>
<img width="1998" height="1044" alt="Screenshot 2025-09-03 001816" src="https://github.com/user-attachments/assets/46db816c-8907-4213-9277-7a610d7a7a83" />
<img width="862" height="750" alt="Screenshot 2025-09-03 001925" src="https://github.com/user-attachments/assets/d0c02eba-72ab-4e7c-8eca-a8f1490bbe03" />
<img width="862" height="736" alt="Screenshot 2025-09-03 002120" src="https://github.com/user-attachments/assets/40aa4657-94a9-4f5b-b65e-ad657b58f365" />



</p>
<p>
Create a new employee named “Jane Doe” (same password) with the username of “jane_admin”/ (password). In Active Directory Users and Computers--> right click _ADMINS--> new--> user--> name the user Jane Doe, user logon name is jane_admin--> create a password (remember it)

</p>
<br />

<p>
<img width="1998" height="1110" alt="Screenshot 2025-09-03 002352" src="https://github.com/user-attachments/assets/d66d15de-938e-4f30-9c4d-611ab44bb18b" />

</p>
<p>
Add jane_admin to the “Domain Admins” Security Group. In Active Directory Users and Computers--> right click Jane Doe--> properties--> member of--> click add--> tyoe in the box Domain Admins and click check names--> click ok--> apply--> ok again. By doing this now the jane_admin is now a  domain admin
</p>
<br />

<p>
<img width="1278" height="504" alt="Screenshot 2025-09-03 002806" src="https://github.com/user-attachments/assets/2933543b-b4bc-425a-a8ad-f899f53b683c" />

</p>
<p>
We are going to log out of DC-1 and log back in under Jane Doe but as a Domain Admin. Obtain    DC-1 IP address and logh back in and enter the credentials: mydomain.com\jane_admin and the password you created. You should be able to log in.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img width="2322" height="1076" alt="Screenshot 2025-09-03 003345" src="https://github.com/user-attachments/assets/a6a5cb5f-8077-4693-b20d-7fc461bb5c0d" />

</p>
<p>
Join Client-1 to your domain (mydomain.com). Login to Client-1 as the original local admin (username) and join it to the domain 

</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
