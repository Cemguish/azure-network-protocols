# Deploy-Active Directory-PowerShell & Group Policy

<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Deploying and Managing Active Directory in Azure with PowerShell & Group Policy</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Create one virtual machine running windows server this is going to be our domain controller.
- Create another virtual machine running windows 10 this will be the client that will join the domain.
- Set up dns IP address in client NIC to same address as the Domain controller.
- 

<h2>Deployment and Configuration Steps</h2>

<p>
  Open Azure and create a resource group.
<img width="1254" height="918" alt="AD1" src="https://github.com/user-attachments/assets/dec71678-1149-45d2-864e-1ceb39bd57ae" />
</p>

<p>
  Name your resource group and select the appropriate region.
  Preview and create.
<img width="1346" height="797" alt="AD2" src="https://github.com/user-attachments/assets/a7a4fa7f-e1c2-43b4-b85d-6c3967635c5f" />

</p>
<br />

<p>
Next I will create a virtual network and create virtual network name. 
Review and Create
<img width="1490" height="889" alt="AD3" src="https://github.com/user-attachments/assets/7f3b0e98-5db9-4435-a4be-ce723b991dad" />



</p>
<p>
  Now I will create the Domain controller.
  Go to virtual machines 
  Create new virtual machine
<img width="1694" height="893" alt="AD4" src="https://github.com/user-attachments/assets/eea77dbb-eef4-4988-bdf4-b1cd51bc4e04" />
</p>
<br />

<p>
  Fill in the necessary information like Resource group, Virtual machine name, Region, Availability zone, Image, Size, Username, Password.
  Check the licensing box then 
<img width="1226" height="929" alt="AD5" src="https://github.com/user-attachments/assets/598af876-e6c9-4b60-a624-103d1d2c5520" />
<img width="1224" height="925" alt="AD6" src="https://github.com/user-attachments/assets/7c06a9dd-5a74-4da1-a465-5e54dda73a7c" />
</p>

<p>
Next go to Next: Disk > Next: Networking > 
Make sure the Virtual network is set to your virtual network name you created previously.
Then Review and create. 
<img width="1455" height="921" alt="AD7" src="https://github.com/user-attachments/assets/1b1cb2b0-7dcd-44ef-964e-98d9a8a02ca0" />
</p><br />

<p>
  Now go back and create another Virtual machine. 
   Fill in the necessary information like Resource group, Virtual machine name Client-1, Region, Availability zone, Image Windows 10 pro, Size, Username, Password.
  Check the licensing box then 
<img width="799" height="927" alt="AD8" src="https://github.com/user-attachments/assets/74145e2e-8664-4c7c-8f49-309e97bbf3dd" />
</p><br />

<p>
Next go to Next: Disk > Next: Networking > 
Make sure the Virtual network is set to your virtual network name you created previously.
Then Review and create. When this is successfully deployed go back to virtual machine.
  <img width="1185" height="923" alt="AD9" src="https://github.com/user-attachments/assets/0a596991-7ddd-4ccd-a7f2-779500b15b4d" />
</p><br />

<p>
  Now in virtual machine I will set the domain controller NIC IP address to static.
<img width="1888" height="864" alt="AD10" src="https://github.com/user-attachments/assets/295f7a0d-55ac-4ec4-b1ec-096e3305869c" />

</p>

<p>
   Click on dc-1 > Networking > Network settings > Locate and copy the dc-1 private IP address. 
  Now click on the Network interface/IP configuration box.
  
  <img width="1885" height="911" alt="AD12" src="https://github.com/user-attachments/assets/5e8b1386-048f-47a8-847a-1edbee62db3a" />
</p><br />

<p>
  Navigate to and click on ipconfig1.
  Go to Private IP address settings > Allocation.
  Select Static and save. Now the address should be static
<img width="1479" height="941" alt="AD13" src="https://github.com/user-attachments/assets/d145da61-1fad-4c82-9099-19d76864c4c4" />
</p>


<p>
Now I will log into the domain controller.
Go back to azure virtual machine then copy dc-1 public IP address
<img width="1909" height="779" alt="AD14" src="https://github.com/user-attachments/assets/8f8e2b35-c557-4e35-a7db-0d8eb68649e2" />
</p><br />



<p>
  Open remote desktop paste dc-1 public IP address and the password you created and login.
  <img width="667" height="826" alt="AD15" src="https://github.com/user-attachments/assets/69eafc09-b823-477d-8c17-5a652e63a884" />
</p><br />

<p>
You should now be logged in to the Domain controller.
  <img width="1907" height="959" alt="AD16" src="https://github.com/user-attachments/assets/174aca99-a275-4b89-b87c-a80edb39e940" />
</p>
<br />

<p>
Side note to check to see if you are in the domain controller.
  Right click on start menu > select system.
  The about screen should popup.
  Check windows specifications - Edition it should say "Windows Server 2022 Datacenter Azure Edition"
  If you see this your are on the right path.
  If you don't see it logout then login again.
  Close window 
</p>

<p>
Once you confirm you are in the domain controller.
  Right click the start menu > Run this should take you to the Firewall Defender menu.
  <img width="1808" height="1010" alt="AD18" src="https://github.com/user-attachments/assets/502ac808-c2ba-4c1b-bbb9-c726dfecd351" />
</p><br />

<p>
  Once you are at the Firewall Defender menu. 
  I will be turning off the firewall for the purpose of this project.
<img width="1051" height="787" alt="image" src="https://github.com/user-attachments/assets/7b791be8-9f90-4b26-9858-e927aa684106" />
</p><br />

<p>
Click on Windows Firewall Defender Properties.
  Next go to firewall state turn it to off. 
  Do this for the following tabs domain and the private profiles tab.
  Apply then ok
  <img width="1051" height="787" alt="AD19" src="https://github.com/user-attachments/assets/68e382b2-e48b-40e4-9c3b-eded067b71b4" />
</p><br />


<p>
  Now I will set client-1 DNS settings to dc-1 private IP address.
  Go to azure and get dc-1 private IP address 
  Copy it.
  Go to click Networking > Networking settings > IP configuration Client-1 Network interface 

<img width="1877" height="921" alt="AD20" src="https://github.com/user-attachments/assets/d2cf0cd7-48f7-45be-b571-a63e349a5354" />
</p>



<p>
  Next go to DNS server > DNS server switch to custom then paste the IP address. Paste dc-1 private IP address and save.
  <img width="1492" height="894" alt="AD21" src="https://github.com/user-attachments/assets/1d579c77-5e8b-4787-a5f3-347a222d3f94" />
<img width="1493" height="867" alt="AD22" src="https://github.com/user-attachments/assets/0886ac62-ebf8-4c3b-97cf-a94b49df5e33" />
</p><br />

<p>
  Now from the azure portal I will restart Client-1.
  Go back to virtual machine.
  Check the Client-1 box and restart.
<img width="1890" height="886" alt="AD23" src="https://github.com/user-attachments/assets/fd3665cf-4a11-4178-b1f4-5d9d3dce22c0" />
</p><br />


<p>
  Now log into client-1 in azure
  Copy the public IP address
  Go to remote desktop
  Paste client-1 public IP address 
  User name: labuser and Cyberlab123!
  <img width="988" height="870" alt="AD24" src="https://github.com/user-attachments/assets/a365d62c-a319-49f5-b887-1e0880a87b89" />
</p><br />

<p>
  Now I will attempt to ping dc-1’s private IP address.
  Go back to azure > virtual machine > click on dc-1 
  Navigate to private IP address
  Copy it 
  Go back to Client-1 session and open powershell.
<img width="1119" height="914" alt="AD25" src="https://github.com/user-attachments/assets/6691bd7e-23fe-4c30-940b-a991f9457481" />
</p><br />



<p>
Open powershell Type ping and dc-1’s private IP address enter
  It should ping.
  <img width="913" height="779" alt="AD26" src="https://github.com/user-attachments/assets/577d3baa-a344-419e-a68e-73717427b41b" /> 
</p><br />


<p>
  Now run ipconfig /all.
  This completes setting up the infrastructure.
<img width="982" height="862" alt="AD27" src="https://github.com/user-attachments/assets/4365bb36-399d-4793-95f9-57106ce0a498" />
</p>

<p>
Next , I will install Active Directory.
Go to azure copy the public IP address for dc-1.
<img width="1869" height="625" alt="ADD1" src="https://github.com/user-attachments/assets/7ee77e09-e035-4507-b18f-1b525a349964" />
</p><br />

<p>
  Open Remote desktop paste the dc-1 public IP address.
<img width="802" height="935" alt="Add2" src="https://github.com/user-attachments/assets/c10e0d33-e3a6-4138-b477-34c00e045e50" />
</p><br />


<p>
When you're in dc-1 go to the start menu and sroll up to server manager.
  <img width="1853" height="1041" alt="Add3" src="https://github.com/user-attachments/assets/a8c5481d-7cfd-4d49-b444-71f482899493" />
</p><br />

<p>
  Now I will set up active directory.
  Go to add roles and features.
  Click next to all.
<img width="1911" height="1030" alt="Add4" src="https://github.com/user-attachments/assets/300c4071-cc9e-435d-99d6-17751d255917" />
</p>

<p>
  When you get to Select server role check the Active directory services box.
  Click Add features button.
  Then next check the Restart if destination server automatically if required box and install.
  After the install close
<img width="1916" height="1038" alt="add6" src="https://github.com/user-attachments/assets/ed92c5c1-4ab3-488e-971d-5d571c719719" />
</p><br />


<p>
  Now I will promote dc-1 as an active domain controller: Also known as a forest.
  Go back to the domain controller dc-1.
  Navigate to the right locate the flag 
  Click on the flag 
  Click on Promote this server to a domain controller.
<img width="1911" height="1030" alt="Add4" src="https://github.com/user-attachments/assets/300c4071-cc9e-435d-99d6-17751d255917" />
</p>

<p>
  <ul>
  <h4>This will bring up the deployment configuration menu.</h4>
  <li>Select Add a new forest.</li>
  <li>For the root domain name type mydomain.com </li>
  <li>Click next and Enter you password and confirm.</li>
  <li>Click next.</li>
  <li>Then install at prerequisites check screen.</li>
  <li>After installation the session window automatically restart.</li>
  <li>Now The server was successfully configured as a domain controller.</li>
</p><br />
<img width="1935" height="1043" alt="Add7" src="https://github.com/user-attachments/assets/292e6d21-36ab-47d9-8fe7-4ed62ffdc395" />
</ul><br />




<p>
  Next log into dc-1 as a domain user.
  User name should be (mydomain.com\labuser) for example or whatever you user name you used and your password.
  Now login
<img width="1274" height="1015" alt="Add8" src="https://github.com/user-attachments/assets/4a453382-5999-4f15-bdcd-8d21419e5d0b" />
</p>



<p>
Now I will Create a Domain Admin user within the domain.
Open Active Directory users and computers and create an organizational unit called _EMPLOYEES.
Go to start click Administrative Tools > Active Directory users and computers 
I will create organizational unit called _EMPLOYEES.
Click mydomain.com dropdown menu > users > Right click on mydomain.com > New organizational unit > Name _EMPLOYEES > Then click ok
Now I will create another one called _ADMIN 
Duplicate previous steps.
  When completed I will create a new  user/employee.
  <img width="1694" height="954" alt="Add9" src="https://github.com/user-attachments/assets/ae4c0156-8093-4ce7-8791-fdf33407de31" />
</p>
<br />


<p>
Now I will create a new  user/employee. 
Go to admins folder right click >  new > User. 
Fill in the necessary fields. 
Create a new employee for example named “Jane Doe” (same password) with the username of “jane_admin” / Cyberlab123!.
Optional, Uncheck user must change password at next logon.
For this project I wil check the password never expires.
Hit next and finish.
<img width="1671" height="954" alt="Add11" src="https://github.com/user-attachments/assets/7cb51008-8b2b-4e1d-9e49-c44f09b1b77e" />
</p>





<p>
Now I will add jane_admin to the “Domain Admins” Security Group
Right click jane doe > Properties > Member of Tab > Add 
Type in the object name to select box. 
Domain Admins > Click check names box > Click ok > Apply > Ok
Now account is an actual domain admin.
<img width="1653" height="944" alt="Add12" src="https://github.com/user-attachments/assets/b4c12836-1074-4c22-ad80-fbed6a7e6539" />
</p><br />





<p>
  Now logout an close the dc-1 connection.
  Log out / close the connection to DC-1 and log back in as “mydomain.com\jane_admin”
  To log out right click > Run > Type logoff > Enter
  Next, log back into dc-1 as jane admin.
  Go to azure get dc-1 public IP address and log in again.
  I will be using this account from this point on.
<img width="1281" height="1033" alt="Add13" src="https://github.com/user-attachments/assets/073da3d6-b9a9-4cd9-8f4c-245d90971071" />

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
