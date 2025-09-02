<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Deploying and Managing Active Directory in Azure with PowerShell & Group Policy</h1>
Demonstration on how to implement Active Directory within Azure Virtual Machines.<br />

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
  

<h2>Deployment and Configuration Steps</h2>
<p>
  <ul>
  <li>Step 1: Go to the Azure Website https://azure.microsoft.com</li>
  <li>Step 2: Click Start free</li>
  <li> Step 3: Sign in / Create a Microsoft Account</li>
  <li>Step 4: Verify Your Identity</li>
  <li>Step 5: Accept Terms & Conditions</li>
  <li>Step 6: Start Using Azure</li>
    </ul>
<img width="1254" height="918" alt="AD1" src="https://github.com/user-attachments/assets/dec71678-1149-45d2-864e-1ceb39bd57ae" />
</p>

<p>     
  Open Azure and create a resource group.
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
 Now I will login to client-1 
  Go back to azure get client-1 public IP address
  Go to remote desktop.
  Once logged in Right click on start menu > System > Rename this PC Advanced.
  Under the computer tab > change > in this box I will join this to the domain. Type in mydomain.com > click ok 
  At the Computer Name > Domain Changes pop up in put User name and password click ok. 
  The pop up will give a prop to restart.
  When it comes back up it will be a member of the domain
  <img width="1327" height="1020" alt="Add14" src="https://github.com/user-attachments/assets/7621f5e1-e32a-4c39-86fc-3b16ac06d466" />
  <img width="1411" height="1016" alt="Add15" src="https://github.com/user-attachments/assets/03177c91-fb0c-4def-88ec-405d2468e8f2" />
</p><br />


<p>
  Now go back to dc-1 
  Right click on start menu
  Type in active directory > Active Directory User and Computers > mydomain > client-1 > double click to verify
  Login to the Domain Controller and verify Client-1 shows up in ADUC

<img width="1003" height="943" alt="Add16" src="https://github.com/user-attachments/assets/27d41b0f-81f5-42b1-86b3-685fff7b69de" />
</p>




<p>
Create a new Organizatiional Unit  named “_CLIENTS” and drag Client-1 into there
</p>
<br />

<p>
  Setup Remote Desktop for non-administrative users on Client-1
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

Next To Do on Monday


<p>
Open remote desktop log into Client-1 as mydomain.com\jane_admin.
Right click on start menu > Go to system > properties > Click Remote Desktop
Scroll down to user accounts and click on select users that can remotely access this PC.
Then click add > go to Enter the object names to select box
Type in domain users then check names > click ok and click ok again
Now all domain admins by default should be able to log into the computer.
<img width="1220" height="1039" alt="AD28" src="https://github.com/user-attachments/assets/609acdab-a246-4494-a95b-368297c8b5b8" />
<img width="1236" height="983" alt="AD29" src="https://github.com/user-attachments/assets/4046163c-908f-4405-9fd4-61f974d18a2b" />

</p><br />
<p>
Next, I will create a bunch of additional users and attempt to log into client-1 with one of the users
Go to back azure get the public IP address go to remote desktop then login to DC-1 as jane_admin
Go to the start menu > type in PowerShell_ise > Right click > Run as an administrator.
Next, Scroll to the script downmenu.
Click the arrow this should split the screen horizontally. ( top half should be white) This is where you will paste the contents of the script.
I am using a pre-generated script from Github
  Go to Github Navigate to the page the script is on. Then copy Raw.
<img width="1098" height="966" alt="AD30" src="https://github.com/user-attachments/assets/1503987f-ae39-4e72-a8f8-773c686a4285" />

Now Create a new file on the desktop> Control =S save to desktop.
Next copy and paste script in Powershell.
Go to start menu open Active Directory. Right click > Run > Type in dsa.msc.
This opens Active Directory users and Computers.
Go back to Active Directory and run script.
<img width="1919" height="1038" alt="AD31" src="https://github.com/user-attachments/assets/70759c6a-469a-4a61-afc5-ebd471a29c2a" />
</p>



<p>
   This will create multiple users.
  When finished, open Active Directory and observe the accounts in the appropriate Organizational Unit　(_EMPLOYEES)
<img width="1904" height="997" alt="AD32" src="https://github.com/user-attachments/assets/225cf5f9-9380-4dfa-8df5-ea7dad3a0af1" /> 
</p><br />


<p>
  Now go to Active Directory Users and computers.
  Click on mydomain.com 
  Scroll down to _EMPLOYEES > right click > then refresh
There should be multiple users available.
  <img width="1383" height="990" alt="AD33" src="https://github.com/user-attachments/assets/7b7fcba2-d183-402a-965a-b380c64b792f" />
</p><br />

<p>
  I will select a random user and attempt to log into Client-1 with one of the accounts.
  Go to client-1 logout then, log back in using one of the credentials selected from the _EMPLOYEES.
<img width="1229" height="1039" alt="AD34" src="https://github.com/user-attachments/assets/fa745688-2d8f-435f-9816-faf5ffe7216a" />
</p><br />



<p>
   Go to remote desktop 
  Change the user name after forward slash to your select user from the _EMPLOYEES list and paste then log in.
  Password be (Password1)
<img width="1590" height="1065" alt="image" src="https://github.com/user-attachments/assets/c531c30a-7bf1-4758-9f6d-356cd898e290" />
</p><br />



<p>
  Go to the Start menu > bring up CMD line then look at the user.
  Then go to this PC > C.drive > Users you should see the users profiles
  It should be the user you selected from the _EMPLOYEES list in Active Directory users and Computers.
<img width="1677" height="990" alt="AD38" src="https://github.com/user-attachments/assets/8ec7540f-f39c-424e-95ec-9328e314fd66" />

</p> 


<p>
  Now we are going to dealing with Account Lockouts.
  Set up lock out policy in Active Directory. I will use group policy to configure it.
  In dc-1 go to the start menu > Right click > Run > Type gpmc.msc enter
  This will bring up Group Policy Management Console.
  Right click on Default Domain > Edit > computer configuration > window settings > Security settings > Account Policies > Account Lockout policy
  To Set lock out duration to 30 minutes. Click ok
  Next, Set Account threshold to 5 log in attempt fails.
  After these settings I will test it out.
<img width="1545" height="1039" alt="AD41" src="https://github.com/user-attachments/assets/aa276d85-c372-450c-82c5-05805769a2ba" />
<img width="1698" height="1039" alt="AD42" src="https://github.com/user-attachments/assets/8d4add5c-be22-4457-8205-3da407222d3b" />
</p>


<p>
  Go to Default Domain Policy > Setting
  You should your the changes you made in your settings.
<img width="1804" height="1042" alt="AD43" src="https://github.com/user-attachments/assets/8c3ae581-6986-49d7-91fe-345cced524e1" />
<img width="1666" height="1020" alt="image" src="https://github.com/user-attachments/assets/1399add8-70fe-40df-a509-d324dfd7da34" />
</p>

<p>
Login to dc-1 > Open Active Directory users and Computer.
Go to mydomain > _EMPLOYEES > Select employee from the list. I Will select User bar.rir and Password is (Password1).
Go back Azure get client-1 public IP address > Open remote desktop and login using a bad password (5 times) until user is locked out. 
  <img width="1294" height="932" alt="AD40" src="https://github.com/user-attachments/assets/567e1089-e039-4f5c-9820-908efd6cd80c" />
</p>
<br />

<p>
Now lets login to client-1 with wrong password 5 times.
Once locked go back to the domain controller > Right click > on mydomain > In the name field Type in the name of locked account and find.
When the account pops up > double click on the name > Account > To unlock account check the box > Apply > ok
<img width="1152" height="1038" alt="AD45" src="https://github.com/user-attachments/assets/be0c21f4-dc32-4f27-aef7-4b5e9755a592" />
<p/>
  
<p>
Go back to client-1 and Attempt to log in.
  You should be able to log in 
  Double check user name in powershell
<img width="1579" height="1026" alt="AD48" src="https://github.com/user-attachments/assets/1ef64418-d90b-41c5-b568-903bc0469a3b" />
  <img width="1686" height="999" alt="AD49" src="https://github.com/user-attachments/assets/00c78f38-e55d-4401-8375-0c266997efbc" />

</p>

<p>
Now I will demonstrate how to reset password.
Go back to dc-1
Once locked go back to the domain controller > Right click > on mydomain > In the name field type in the name of the user account you need to reset.
When the account pops up > right click > scroll down to reset and select it > You will be prompted to reset password >  In put new password then ok > This should reset the account.
<img width="1720" height="890" alt="AD50" src="https://github.com/user-attachments/assets/ec8509dd-73db-4053-82eb-db5a65c3af68" />
</p>



<p>
Now I will demonstrate how to disable an account.
  Go back to mydomain.com > Click Right > Find > Type in the the user account you what to disable > Find now
  Scroll down to the user > Right Click > Disable the account.
  Now account should be disabled.
  To re-enable follow the same steps and click enable.
  <img width="1730" height="982" alt="AD51" src="https://github.com/user-attachments/assets/a4e21426-b792-4a0d-9882-39e3c0c6cf3e" />
</p>
<br />




<p>
  Lastly, I will view event logs 
  Go to windows menu type event viewer
  Click on window log dropdown menu> Find > Type in user account you wish to observe.
  This will show you logins, logouts, failure etc.
You can do the same process by login in client-1 > go to the start menu > event viewer > right click and run as administrator.
  This concludes the demonstration on group policies in Active directory 
<img width="1921" height="1044" alt="image" src="https://github.com/user-attachments/assets/38e317e6-1f50-4dfe-bb7d-824b43eba704" />
</p>





