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
<img width="2322" height="1076" alt="Screenshot 2025-09-03 003345" src="https://github.com/user-attachments/assets/a6a5cb5f-8077-4693-b20d-7fc461bb5c0d" />
<img width="1288" height="714" alt="Screenshot 2025-09-03 003522" src="https://github.com/user-attachments/assets/5225ec63-40c8-4010-89d1-a874ecbe92e8" />


</p>
<p>
Now we have to join Client-1 to your domain (mydomain.com). Obtain IP address of Client-1 and login as the original local admin (username) and join it to the domain (computer will restart)--> right click start and go to system--> rename this PC (advanced)--> under the computer name tab, click change--> click on domain and type in box, mydomain.com--> click ok--> a windows security pop up should appear and enter the mydomain.com\jane_admin and password credentials and should grant permission to join the domain. A pop up should appear after welcoming you to the domain.
</p>
<br />

<p>
<img width="1500" height="1042" alt="Screenshot 2025-09-03 004054" src="https://github.com/user-attachments/assets/0fd96f98-ea75-417b-848a-7c21c9efa61c" />
<img width="1494" height="1046" alt="Screenshot 2025-09-03 004208" src="https://github.com/user-attachments/assets/4828252a-a5a1-4ccd-91db-21c9d3211a41" />


</p>
<p>
Log back on to DC-1--> go to Active Directory Users and Computers to verify that Client-1 appears there. Next, create a new organization unit and call it _CLIENTS and drag Client-1 it into the _CLIENTS organization unit.

</p>
<br />

<p>
<img width="892" height="908" alt="Screenshot 2025-09-03 005220" src="https://github.com/user-attachments/assets/10a25391-05c8-4d0f-b4e4-a73e9a2deced" />

</p>
<p>
We are going to setup Remote Desktop for non-administrative users on Client-1. Locate the IP address of Client-1 and log into Client-1 as mydomain.com\jane_admin. Once logged on--> open system--> select users that can remotely access this PC--> click add--> type domain users--> click ok. You can now log into Client-1 as a normal, non-administrative user now.


</p>
<br />

<p>
<img width="2566" height="1328" alt="Screenshot 2025-09-03 005912" src="https://github.com/user-attachments/assets/0db671c5-bb4b-4799-b99a-23b3fdb0e93b" />

</p>
<p>
Next, We are going to create a bunch of additional users and attempt to log into client-1 with one of the users--> Login to DC-1 as jane_admin--> Open PowerShell_ise as an administrator--> you are going to copy and paste the following script https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1 and run the script and observe the accounts being created.

</p>
<br />

<p>
<img width="876" height="606" alt="Screenshot 2025-09-03 010917" src="https://github.com/user-attachments/assets/c8e9197d-55a6-452d-9a5c-641d8433ba04" />


</p>
<p>
In Active Directory Users and Computers, pick one of the users (remember which one you used) and attempt to log into Client-1 with one of the accounts. (take note of the password in the script) You should be able to login.

</p>
<br />

<p>
<img width="1586" height="1298" alt="Screenshot 2025-09-03 233053" src="https://github.com/user-attachments/assets/42c8c240-91b9-4203-90af-a1af6e1dddee" />
<img width="1712" height="1116" alt="Screenshot 2025-09-03 233611" src="https://github.com/user-attachments/assets/d7211a44-a091-4564-9114-bfa9bde6fe6c" />
<img width="532" height="220" alt="image" src="https://github.com/user-attachments/assets/08d28186-5c74-41b3-b900-1bd2889241d6" />

</p>
<p>
Next we are going to be dealing with Account Lockouts but we are going to configure account lockout threshold in group policy--> Get logged into dc-1--> go to run and type gpmc.msc and click enter--> right click default domain policy and click edit--> In the Group Policy Management Editor, expand the following: Computer Configuration > Policies > Windows Settings > Security Settings > Account Policies > Account Lockout Policy.

You will see three primary settings that you need to configure: Account Lockout Duration which is the time in minutes that an account remains locked before it is automatically unlocked--> Account Lockout Threshold which is the number of failed logon attempts that will trigger an account lockout--> Reset Account Lockout Counter after which is the time in minutes after which the failed logon attempts counter is reset to 0, assuming there are no additional failed logon attempts.


</p>
<br />

<p>
<img width="994" height="574" alt="Screenshot 2025-09-03 234553" src="https://github.com/user-attachments/assets/6ab925d7-2b22-4a05-b1bc-66ba69aeb329" />
<img width="1098" height="296" alt="Screenshot 2025-09-03 234821" src="https://github.com/user-attachments/assets/ed55d349-fa07-42ef-b1b7-bafde6d973b8" />

</p>
<p>
Next, we have to update Group Policy. You can wait for the Group Policy to propagate automatically, or you can force an update immediately. On Client-1, open Command Prompt and type gpupdate /force, then press Enter. The Group Policy should now be applied. After, select one of the users and try to fail login in 5 times to see if it gets locked out.


</p>
<br />

<p>
<img width="1908" height="1056" alt="Screenshot 2025-09-03 235002" src="https://github.com/user-attachments/assets/69fc787c-a4de-460e-95f4-2e5ad93c2ad8" />
<img width="1836" height="748" alt="Screenshot 2025-09-03 011120" src="https://github.com/user-attachments/assets/1004c388-eab9-4c48-b4f2-867fd9398980" />

</p>
<p>
In Active Directory Users and Computers in DC-1--> go to _EMPLOYEES and the user you chose--> go to the account tab and you will see that the box is checked to show that the account is locked out. Uncheck the box to unlock the account. Try to login with the user after and you should be able to login now.
</p>
<br />

<p>
<img width="1092" height="528" alt="Screenshot 2025-09-03 235650" src="https://github.com/user-attachments/assets/94633bc8-3300-44f6-b6ae-6d4474955306" />
<img width="1030" height="1044" alt="Screenshot 2025-09-03 235930" src="https://github.com/user-attachments/assets/bf8fe190-70b2-443b-a088-2b64965d1c77" />

</p>
<p>
To reset password--> In DC-1--> go to Active Directory users and computers and search the user and click on it--> right click the name and click reset password and/or disable accounts as well.
</p>
<br />

<p>
<img width="2100" height="1416" alt="Screenshot 2025-09-04 000744" src="https://github.com/user-attachments/assets/0207476b-dfd0-4175-8348-a38923cd0311" />


</p>
<p>
To search up the logs to see past login attempts--> search eventvwr.msc and observe to login attempts/failures.
</p>
<br />
<img width="1560" height="468" alt="Screenshot 2025-09-04 001019" src="https://github.com/user-attachments/assets/f15a6436-734e-40e8-b0d0-f4664aca8e10" />
<img width="908" height="946" alt="Screenshot 2025-09-04 001206" src="https://github.com/user-attachments/assets/9585019c-3950-40d1-b99f-6809c4064c37" />

<p>


</p>
<p>
If the login attempts/failures are not present--> go back to Client-1--> search eventvwr.msc and run as administrator--> enter credentials for jane_admin--> and you should be able to see the login attempts/failures of the users.
</p>
<br />

<p>
<img width="2300" height="1396" alt="Screenshot 2025-09-04 001417" src="https://github.com/user-attachments/assets/94b2fb9c-4f31-49b8-89a5-e3cd46139300" />

</p>
<p>
This concludes this tutorial of Active Directory and I hope you find this helpful. Do not forget to delete the VMs so no additional charges apply to your account.






