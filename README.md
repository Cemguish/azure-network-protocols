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
  

<h2>Steps to Download Azure</h2>
<p>
  <ul>
  <li>Step 1: Go to the Azure Website https://azure.microsoft.com</li>
  <li>Step 2: Click Start free</li>
  <li> Step 3: Sign in / Create a Microsoft Account</li>
  <li>Step 4: Verify Your Identity</li>
  <li>Step 5: Accept Terms & Conditions</li>
  <li>Step 6: Start Using Azure</li>
    </ul>
    <img width="1909" height="1001" alt="image" src="https://github.com/user-attachments/assets/cd545f5d-90e3-4ae4-88ec-01ce45b03b5b" />
</p><br>



<p>  
    <b>Open Azure and create a resource group.</b>
  <ul>
 <li>Log in to Azure Portal</li>
<li>In the left-hand menu, click Resource groups.</li>
<li>Click + Create.</li>
<li>Fill in the details:</li>
<li>Subscription: Choose your Azure subscription.</li>
<li>Resource group name: Enter a unique name (Active-Directory).</li>
<li>Region: Select the region closest to where your resources will run (like East US2).</li>
<li>Click Review + Create → then Create.</li>
<li>Done, You’ll see your resource group listed.</li><br>
  <img width="1254" height="918" alt="AD1" src="https://github.com/user-attachments/assets/dec71678-1149-45d2-864e-1ceb39bd57ae" />
    </ul>
</p>



<br>

<p>

 <b>Next, create a virtual network and assign it a name.</b> 
  <ul>
<li>In the left menu, search for Virtual networks and click it.</li>
<li>Click + Create.</li>
<li>Fill in the Basics tab:</li>
<li>Subscription: Select your subscription.</li>
<li>Resource group: Choose an existing one or create a new resource group.</li>
<li>Name: Enter a name (e.g., MyVNet01).</li>
<li>Region: Choose a region (should match where you’ll deploy resources).</li>
<li>Move to the IP Addresses tab:</li>
<li>Define the Address space (default is 10.0.0.0/16).</li>
<li>Add at least one subnet (default is 10.0.0.0/24).</li>
<li>(Optional) Configure Security (like DDoS protection, Firewall, etc.).</li>
<li>Click Review + Create → then Create.</li><br>
<img width="1346" height="797" alt="AD2" src="https://github.com/user-attachments/assets/a7a4fa7f-e1c2-43b4-b85d-6c3967635c5f" />
<img width="1490" height="889" alt="AD3" src="https://github.com/user-attachments/assets/7f3b0e98-5db9-4435-a4be-ce723b991dad" />
    
 </ul>

</p>
<br>

<p>
 <b>Now I will create the Domain controller. Go to virtual machine and create a virtual machine.</b><br>
 
  <ul>   
<li>Click + Create → Azure virtual machine.</li>
<li>Fill in the Basics tab:</li>
<li>Subscription: Select your subscription ( Azure subscription1.)</li>
<li>Resource group: Pick your existing one (e.g., MyResourceGroup) (Active-Directory).</li>
<li>VM name: Choose a name (e.g., MyVM01) (dc-1).</li>
<li>Region: Same region as your VNet.(East US 2) </li>
<li>Image: Choose the OS (Windows Server, Ubuntu, etc.) (Windows Server 2022 Datacenter: Azure Edition).</li>
<li>Size: Pick a VM size (e.g., B1s for free-tier)(Standard_D2s_v3 -2 vcpus,8 GiB memory).</li>
<li>Authentication type: Choose Password or SSH key.(Cyberlab123!)</li>
<li>Username/Password or SSH Key: Set login credentials.(labuser)</li> 
<li>Check Licensing box </li> 
<li>Inbound ports: Select what to allow (e.g., RDP for Windows, SSH for Linux).</li>
<li>Go to Networking:</li>
<li>Select your Virtual network (e.g., MyVNet01)(Active-Directory-VNet).</li>
<li>Choose an existing Subnet (e.g., MySubnet01).</li>
<li>Leave defaults for Disks and Management (or customize if needed).</li>
<li>Click Review + Create → Create.</li>
<li>✅ After deployment, you can connect via SSH (Linux) or RDP (Windows).</li><br>
     </ul>
<img width="1694" height="893" alt="AD4" src="https://github.com/user-attachments/assets/eea77dbb-eef4-4988-bdf4-b1cd51bc4e04" />
<img width="1226" height="929" alt="AD5" src="https://github.com/user-attachments/assets/598af876-e6c9-4b60-a624-103d1d2c5520" />
<img width="1224" height="925" alt="AD6" src="https://github.com/user-attachments/assets/7c06a9dd-5a74-4da1-a465-5e54dda73a7c" />
<img width="1455" height="921" alt="AD7" src="https://github.com/user-attachments/assets/1b1cb2b0-7dcd-44ef-964e-98d9a8a02ca0" />
</p> <br>

<p>
  <b>Now go back and create another Virtual machine.</b> 
    <ul>  
  <li>Click + Create → Azure virtual machine.</li>
<li>Fill in the Basics tab:</li>
<li>Subscription: Select your subscription ( Azure subscription1.)</li>
<li>Resource group: Pick your existing one (e.g., MyResourceGroup) (Active-Directory).</li>
<li>VM name: Choose a name (e.g., MyVM01) (client-1).</li>
<li>Region: Same region as your VNet.(East US 2) </li>
<li>Image: Choose the OS (Windows Server, Ubuntu, etc.)(Windows 10 Pro,Version 22H2 - x64 Gen2).</li>
<li>Size: Pick a VM size (e.g., B1s for free-tier)(Standard_D2s_v3 -2 vcpus,8 GiB memory).</li>
<li>Authentication type: Choose Password or SSH key.(Cyberlab123!)</li>
<li>Username/Password or SSH Key: Set login credentials.(labuser)</li> 
<li>Check Licensing box </li> 
<li>Inbound ports: Select what to allow (e.g., RDP for Windows, SSH for Linux).</li>
<li>Go to Networking:</li>
<li>Select your Virtual network (e.g., MyVNet01)(Active-Directory-VNet).</li>
<li>Choose an existing Subnet (e.g., MySubnet01).</li>
<li>Leave defaults for Disks and Management (or customize if needed).</li>
<li>Click Review + Create → Create.</li>
<li>After deployment, you can connect via SSH (Linux) or RDP (Windows).</li><br>
</ul>
<img width="799" height="927" alt="AD8" src="https://github.com/user-attachments/assets/74145e2e-8664-4c7c-8f49-309e97bbf3dd" />
<img width="1185" height="923" alt="AD9" src="https://github.com/user-attachments/assets/0a596991-7ddd-4ccd-a7f2-779500b15b4d" />
</p><br>



<p>
  <b>Now, in the virtual machine, set the Domain Controller NIC IP address to static.</b>
     <ul>  
  <li>Go to the VM in the Azure Portal.</li>
<li>In the left menu, under Settings, click Networking.</li>
<li>Select the Network interface (it will have a name like MyVM01-nic) (Locate and copy the dc-1 private IP address).</li>
<li>In the NIC blade, go to IP configurations.</li>
<li>Click the IP configuration (usually named ipconfig1).</li>
<li>Change Assignment from Dynamic → Static.</li>
<li>Choose an available IP address (or keep the current one).</li>
<li>Click Save.</li><br>
       </ul>
<img width="1888" height="864" alt="AD10" src="https://github.com/user-attachments/assets/295f7a0d-55ac-4ec4-b1ec-096e3305869c" />
<img width="1885" height="911" alt="AD12" src="https://github.com/user-attachments/assets/5e8b1386-048f-47a8-847a-1edbee62db3a" />
<img width="1479" height="941" alt="AD13" src="https://github.com/user-attachments/assets/d145da61-1fad-4c82-9099-19d76864c4c4" />
</p><br>


<p>
  <b>Now I will log into the domain controller.</b>
  <li>Go back to the Azure virtual machine, then copy the dc-1 public IP address.</li>
  <li>Open remote desktop, paste the dc-1 public IP address and the password (Cyberlab123!) you created, and log in (labuser).</li>
  <li>You should now be logged in to the Domain controller.</li><br />
<img width="1909" height="779" alt="AD14" src="https://github.com/user-attachments/assets/8f8e2b35-c557-4e35-a7db-0d8eb68649e2" />
<img width="667" height="826" alt="AD15" src="https://github.com/user-attachments/assets/69eafc09-b823-477d-8c17-5a652e63a884" />
<img width="1907" height="959" alt="AD16" src="https://github.com/user-attachments/assets/174aca99-a275-4b89-b87c-a80edb39e940" />
</p><br>



<p>
  <b>Side note: Check to see if you are in the domain controller.</b>
   <li>Right-click on start menu > select system.</li>
  <li>The about screen should pop up.</li>
   <li>Check windows specifications - Edition it should say "Windows Server 2022 Datacenter Azure Edition"</li>
   <li>If you see this, you're on the right path.</li>
   <li>If you don't see it, log out and then log in again.</li>
   <li>Close window</li> 
   <li>Once you confirm that you are in the domain controller.</li>
   <li>Right-click the Start menu > Run. This should take you to the Firewall Defender menu.</li><br>


  <b>Check if you are on the Domain Controller</b>
  <dl>
<dt>Right-click the Start menu → select System.</dt>
 <dd>The About screen will appear.</dd>
 <dd>Under Windows specifications – Edition, confirm it says: Windows Server 2022 Datacenter Azure Edition.</dd>


<dt>👉 Side Note:</dt>
<dd>If you see this, you’re on the right path.</dd>
<dd>If not, log out and log back in, then check again.</dd>
<dd>Close the About window once confirmed.</dd>
  <img width="1808" height="1010" alt="AD18" src="https://github.com/user-attachments/assets/502ac808-c2ba-4c1b-bbb9-c726dfecd351" />
</p><br>



<p>
<b>Once you are in the correct domain controller, you will be disabling the Windows firewall.</b>
<li>Go to the Windows Defender Firewall menu, turn off the firewall.</li>
<li>This is only for the purpose of this project.</li>
 <li>Once you are at the Firewall Defender menu.</li>
 <li>For the purpose of this project, I will turn off the firewall.</li>
  <li>Click on Windows Firewall Defender Properties.</li>
  <li>Next go to firewall state turn it to off.</li> 
  <li>Repeat this process for the Domain and Private Profiles tabs.</li>
  <li>Apply then ok</li><br>
    <i>For this project, we are temporarily disabling the firewall to prevent network restrictions from blocking communication between your server and the other devices or services you will set up.</i>
 <i>Normally, keeping the firewall on is a security best practice, but in this specific lab or project environment, turning it off will make configuration easier and prevent issues with connectivity.</i><br>
 <img width="1808" height="1010" alt="AD18" src="https://github.com/user-attachments/assets/c20566bf-71a4-49fa-a7a8-73932cc9ad36" />
  <img width="1051" height="787" alt="image" src="https://github.com/user-attachments/assets/7b791be8-9f90-4b26-9858-e927aa684106" />
  <img width="1051" height="787" alt="AD19" src="https://github.com/user-attachments/assets/68e382b2-e48b-40e4-9c3b-eded067b71b4" />
</p><br>


<p>
  <b>Now I will set client-1 DNS settings to the dc-1 private IP address.</b>
  <li>Go to Azure and get the dc-1 private IP address.</li> 
  <li>Copy it.</li>
  <li>Go to click Networking > Networking settings > IP configuration Client-1 Network interface</li> 
  <li>Next go to DNS server > DNS server switch to custom, then paste the IP address.</li>
  <li>Paste dc-1 private IP address and save.</li><br>
  <img width="1877" height="921" alt="AD20" src="https://github.com/user-attachments/assets/d2cf0cd7-48f7-45be-b571-a63e349a5354" />
  <img width="1492" height="894" alt="AD21" src="https://github.com/user-attachments/assets/1d579c77-5e8b-4787-a5f3-347a222d3f94" />
<img width="1493" height="867" alt="AD22" src="https://github.com/user-attachments/assets/0886ac62-ebf8-4c3b-97cf-a94b49df5e33" />
</p><br>



<p>
    <b>Now, from the Azure portal, I will restart client-1.</b>
    <li>Go back to the virtual machine.</li>
    <li>Check the client-1 box and restart.</li>
    <li>Now log in to client-1 in Azure.</li>
    <li>Copy the public IP address.</li>
    <li>Go to the remote desktop.</li>
    <li>Paste client-1 public IP address.</li> 
    <li>User name: labuser and Cyberlab123!</li><br>
  <img width="1890" height="886" alt="AD23" src="https://github.com/user-attachments/assets/fd3665cf-4a11-4178-b1f4-5d9d3dce22c0" />
  <img width="988" height="870" alt="AD24" src="https://github.com/user-attachments/assets/a365d62c-a319-49f5-b887-1e0880a87b89" />
</p><br>

<p>
      <b>Now I will attempt to ping dc-1’s private IP address.</b>
     <li>Go back to azure > virtual machine > click on dc-1.</li> 
     <li>Navigate to private IP address.</li>
     <li>Copy it.</li> 
     <li>Return to the Client-1 session and open PowerShell.</li>
     <li>Open PowerShell, type ping followed by DC-1’s private IP address, and press Enter.</li>
     <li>It should ping.</li>
   <li>Open PowerShell, Type ping and dc-1’s private IP address, and enter.</li>
     <li>It should ping.</li><br>
  <img width="1119" height="914" alt="AD25" src="https://github.com/user-attachments/assets/6691bd7e-23fe-4c30-940b-a991f9457481" />
  <img width="913" height="779" alt="AD26" src="https://github.com/user-attachments/assets/577d3baa-a344-419e-a68e-73717427b41b" /> 
</p><br>


<p>
  <b>Now run ipconfig /all.</b>
  <li>This completes setting up the infrastructure.</li>
  <li>Next, I will install Active Directory.</li>
  <li>Go to Azure, copy the public IP address for dc-1.</li>
  <li>Open Remote Desktop and paste DC-1’s public IP address.</li><br>
  <img width="982" height="862" alt="AD27" src="https://github.com/user-attachments/assets/4365bb36-399d-4793-95f9-57106ce0a498" />
<img width="1869" height="625" alt="ADD1" src="https://github.com/user-attachments/assets/7ee77e09-e035-4507-b18f-1b525a349964" />
<img width="802" height="935" alt="Add2" src="https://github.com/user-attachments/assets/c10e0d33-e3a6-4138-b477-34c00e045e50" />
</p><br>


<p>
  <b>Once logged in to DC-1, go to the Start menu and open Server Manager.</b>
    <li>Next, I will set up Active Directory.</li>
    <li>Go to add roles and features.</li>
    <li>Click Next on all prompts.</li>
    <li>When you reach the Select Server Roles screen, check the Active Directory Domain Services box.</li>
    <li>Click the Add Features button.</li>
    <li>Next, check the Restart the destination server automatically if required box, and then click Install.</li>
    <li>After the installation is complete, close the window.</li><br>
    <img width="1853" height="1041" alt="Add3" src="https://github.com/user-attachments/assets/a8c5481d-7cfd-4d49-b444-71f482899493" />
  <img width="1911" height="1030" alt="Add4" src="https://github.com/user-attachments/assets/300c4071-cc9e-435d-99d6-17751d255917" />
<img width="1916" height="1038" alt="add6" src="https://github.com/user-attachments/assets/ed92c5c1-4ab3-488e-971d-5d571c719719" />
</p><br>


<p>
   <b>Next, I will promote DC-1 to an Active Directory Domain Controller, also known as a forest.</b>
     <li>Return to the Domain Controller, dc-1.</li>
     <li>On the right, locate the flag icon.</li> 
     <li>Click the flag icon.</li>
     <li>Click Promote this server to a domain controller.</li><br>
<img width="1911" height="1030" alt="Add4" src="https://github.com/user-attachments/assets/300c4071-cc9e-435d-99d6-17751d255917" />
</p>

<p>
  <ul>
  <h4>This will open the Deployment Configuration menu.</h4>
  <li>Select Add a new forest.</li>
  <li>For the Root Domain Name, enter mydomain.com.</li>
  <li>Click Next, then enter your password and confirm it.</li>
  <li>Click next.</li>
  <li>Then, click Install at the Prerequisites Check screen.</li>
  <li>After the installation, the session window will automatically restart.</li>
  <li>The server has now been successfully configured as a Domain Controller.</li>
  <li>Next, log in to DC-1 using a domain user account.</li>
  <li>Use the username in the format mydomain.com\labuser (or the username you created) and enter your password.</li>
  <li>Now login</li><br>
<img width="1935" height="1043" alt="Add7" src="https://github.com/user-attachments/assets/292e6d21-36ab-47d9-8fe7-4ed62ffdc395" />
<img width="1274" height="1015" alt="Add8" src="https://github.com/user-attachments/assets/4a453382-5999-4f15-bdcd-8d21419e5d0b" />
    </p>
</ul><br>



<p>
<b>Next, I will create a Domain Admin user within the domain.</b>
<li>Open Active Directory Users and Computers and create an Organizational Unit named _EMPLOYEES./li>
<li>Go to Start, then click Administrative Tools > Active Directory Users and Computers.</li> 
<li>I will create an Organizational Unit named _EMPLOYEES.</li>
<li>Click the mydomain.com dropdown menu.</li> 
  <li>Users</li> 
  <li>Right-click on mydomain.com.</li> 
  <li>Select New > Organizational Unit.</li> 
  <li>Name _EMPLOYEES.</li> 
  <li>Then, click OK.</li>
<li>Next, I will create another Organizational Unit named _ADMIN.</li>
<li>Repeat the previous steps to create the _ADMIN Organizational Unit.</li>
  <li>Once complete, I will create a new user/employee.</li><br>
  <img width="1694" height="954" alt="Add9" src="https://github.com/user-attachments/assets/ae4c0156-8093-4ce7-8791-fdf33407de31" />
</p>
<br>


<p>
<b>Next, I will create a new user/employee.</b> 
<li>Go to the _ADMIN folder and right-click it.</li> 
  <li>New</li> 
  <li>User</li> 
<li>Fill in all the required fields.</li> 
<li>Create a new employee, for example, named Jane Doe, with the username jane_admin and the password Cyberlab123!.</li>
<li>Optionally, uncheck User must change password at next logon.</li>
<li>For this project, I will check Password never expires.</li>
<li>Click Next, then click Finish.</li><br>
<img width="1671" height="954" alt="Add11" src="https://github.com/user-attachments/assets/7cb51008-8b2b-4e1d-9e49-c44f09b1b77e" />
</p>
<br>




<p>
<b>Next, I will add jane_admin to the Domain Admins security group.</b> 
<li>Right-click jane doe.</li>  
  <li>Properties.</li>   
  <li>Member of Tab.</li>  
  <li>Add.</li>  
<li>Type the object name into the Select box.</li>  
<li>Domain Admins.</li>  
  <li>Click the Check Names button.</li>  
  <li>Then, click OK.</li>  
  <li>Apply.</li>  
  <li>OK.</li> 
<li>The account is now a Domain Admin.</li><br>
<img width="1653" height="944" alt="Add12" src="https://github.com/user-attachments/assets/b4c12836-1074-4c22-ad80-fbed6a7e6539" />
</p><br>





<p>
  <b>Now, log out and close the DC-1 connection.</b>
  <li>Log out and close the connection to DC-1, then log back in as mydomain.com\jane_admin.</li>  
  <li>To log out, right-click the Start menu.</li>  
    <li>Run.</li>    
    <li>Type logoff and press Enter.</li>   
    <li>Enter.</li>  
  <li>Next, log back into DC-1 as jane_admin.</li>  
  <li>In Azure, obtain DC-1’s public IP address and log in again.</li>  
  <li>I will use this account from this point onward.</li><br>
<img width="1281" height="1033" alt="Add13" src="https://github.com/user-attachments/assets/073da3d6-b9a9-4cd9-8f4c-245d90971071" />

</p><br>


<p>
  <b>Next, I will log in to Client-1.</b> 
    <li>In Azure, obtain Client-1’s public IP address.</li> 
    <li>Open Remote Desktop.</li> 
    <li>Once logged in, right-click the Start menu, select System, then click Rename this PC (Advanced).</li> 
    <li>Under the Computer tab, click Change. In the box, type mydomain.com to join the domain, then click OK.</li>  
    <li>In the Computer Name/Domain Changes pop-up, enter your username and password, then click OK.</li>  
    <li>The pop-up will prompt you to restart the computer.</li> 
    <li>After restarting, the computer will be a member of the domain.</li><br> 
  <img width="1327" height="1020" alt="Add14" src="https://github.com/user-attachments/assets/7621f5e1-e32a-4c39-86fc-3b16ac06d466" />
  <img width="1411" height="1016" alt="Add15" src="https://github.com/user-attachments/assets/03177c91-fb0c-4def-88ec-405d2468e8f2" />
</p><br>


<p>
    <b>Return to dc-1.</b>  
   <li>Right-click the Start menu.</li> 
   <li>Type Active Directory in the search, open Active Directory Users and Computers, navigate to mydomain > Client-1, and double-click to verify.</li> 
   <li>Log in to the Domain Controller and verify that client-1 appears in Active Directory Users and Computers (ADUC).</li> 
   <li>Create a new Organizatiional Unit  named “_CLIENTS” and drag client-1 into that folder.</li> 
    <li>Set up Remote Desktop on client-1 for non-administrative users.</li><br>
<img width="1003" height="943" alt="Add16" src="https://github.com/user-attachments/assets/27d41b0f-81f5-42b1-86b3-685fff7b69de" />
</p><br>

<p>
  <b>Open Remote Desktop and log in to client-1 as mydomain.com\jane_admin.</b> 
   <li>Right-click the Start menu, go to System > Properties, and then click Remote Desktop.</li> 
   <li>Scroll down to User Accounts and click Select users that can remotely access this PC.</li> 
   <li>Then click Add, and go to the Enter the object names to select box.</li> 
   <li>Type Domain Users, click Check Names, then click OK, and click OK again.</li> 
   <li>Now, all Domain Admins should be able to log in to the computer by default.</li><br>
<img width="1220" height="1039" alt="AD28" src="https://github.com/user-attachments/assets/609acdab-a246-4494-a95b-368297c8b5b8" />
<img width="1236" height="983" alt="AD29" src="https://github.com/user-attachments/assets/4046163c-908f-4405-9fd4-61f974d18a2b" />
</p><br>
    
<p>
 <b>I will create several additional users and then attempt to log in to client-1 with one of them.</b> 
 <li>In Azure, obtain the public IP address, open Remote Desktop, and log in to DC-1 as jane_admin.</li> 
 <li>Go to the Start menu, type PowerShell_ISE, right-click it, and select Run as Administrator.</li> 
 <li>Next, Scroll to the script downmenu.</li> 
 <li>Click the arrow to split the screen horizontally (the top half should be white). This is where you will paste the contents of the script.</li> 
 <li>I am using a pre-generated script from GitHub.</li> 
   <li>Go to GitHub, navigate to the page with the script, and then click Raw to copy it.</li><br>
<img width="1098" height="966" alt="AD30" src="https://github.com/user-attachments/assets/1503987f-ae39-4e72-a8f8-773c686a4285" />
</p><br>


 <b>Create a new file on the desktop, press Ctrl + S, and save it to the desktop.</b> 
 <li>Next, copy and paste the script into PowerShell.</li> 
 <li>Go to the Start menu, open Active Directory, right-click, select Run, and type dsa.msc.</li> 
 <li>This will open Active Directory Users and Computers.</li> 
 <li>Return to Active Directory and run the script.</li><br> 
<img width="1919" height="1038" alt="AD31" src="https://github.com/user-attachments/assets/70759c6a-469a-4a61-afc5-ebd471a29c2a" />
</p><br>



<p>
    <b>This will create multiple users.</b> 
   <li>When finished, open Active Directory and verify the accounts in the appropriate Organizational Unit (_EMPLOYEES).</li><br>  
<img width="1904" height="997" alt="AD32" src="https://github.com/user-attachments/assets/225cf5f9-9380-4dfa-8df5-ea7dad3a0af1" /> 
</p><br>


<p>
   <b>Next, go to Active Directory Users and Computers.</b> 
   <li>Click on mydomain.com.</li>  
   <li>Scroll down to _EMPLOYEES > right click > then refresh.</li> 
 <li>There should be multiple users available.</li><br>  
  <img width="1383" height="990" alt="AD33" src="https://github.com/user-attachments/assets/7b7fcba2-d183-402a-965a-b380c64b792f" />
</p><br>

<p>
   <b>I will select a random user and attempt to log into Client-1 with one of the accounts.</b>
    <li>Go to client-1 logout then, log back in using one of the credentials selected from the _EMPLOYEES.</li><br> 
<img width="1229" height="1039" alt="AD34" src="https://github.com/user-attachments/assets/fa745688-2d8f-435f-9816-faf5ffe7216a" />
</p><br>



<p>
     <b>Go to remote desktop</b>  
      <li>Change the user name after forward slash to your select user from the _EMPLOYEES list and paste then log in.</li>
      <li>Password be (Password1).</li><br> 
<img width="1590" height="1065" alt="image" src="https://github.com/user-attachments/assets/c531c30a-7bf1-4758-9f6d-356cd898e290" />
</p><br> 



<p>
  <b>Go to the Start menu > bring up CMD line then look at the user.</b> 
  <li>Then go to this PC > C.drive > Users you should see the users profiles.</li>
  <li>It should be the user you selected from the _EMPLOYEES list in Active Directory users and Computers.</li><br> 
<img width="1677" height="990" alt="AD38" src="https://github.com/user-attachments/assets/8ec7540f-f39c-424e-95ec-9328e314fd66" />
</p><br>  


<p>
   <b>Now we are going to dealing with Account Lockouts.</b> 
  <li>Set up lock out policy in Active Directory. I will use group policy to configure it.</li>
  <li>In dc-1 go to the start menu > Right click > Run > Type gpmc.msc enter.</li>
  <li>This will bring up Group Policy Management Console.</li>
  <li>Right click on Default Domain > Edit > computer configuration > window settings > Security settings > Account Policies > Account Lockout policy.</li>
  <li>To Set lock out duration to 30 minutes. Click ok.</li>
  <li>Next, Set Account threshold to 5 log in attempt fails.</li>
  <li>After these settings I will test it out.</li><br> 
<img width="1545" height="1039" alt="AD41" src="https://github.com/user-attachments/assets/aa276d85-c372-450c-82c5-05805769a2ba" />
<img width="1698" height="1039" alt="AD42" src="https://github.com/user-attachments/assets/8d4add5c-be22-4457-8205-3da407222d3b" />
</p><br> 


<p>
     <b>Go to Default Domain Policy > Setting</b> 
    <li>You should see the changes you made in your settings.</li><br>
<img width="1804" height="1042" alt="AD43" src="https://github.com/user-attachments/assets/8c3ae581-6986-49d7-91fe-345cced524e1" />
<img width="1666" height="1020" alt="image" src="https://github.com/user-attachments/assets/1399add8-70fe-40df-a509-d324dfd7da34" />
</p>

<p>
 <b>Login to dc-1 > Open Active Directory users and Computer.</b> 
 <li>Go to mydomain > _EMPLOYEES > Select employee from the list. I Will select User bar.rir and Password is (Password1).</li>
 <li>Go back Azure get client-1 public IP address > Open remote desktop and login using a bad password (5 times) until user is locked out.</li><br>
  <img width="1294" height="932" alt="AD40" src="https://github.com/user-attachments/assets/567e1089-e039-4f5c-9820-908efd6cd80c" />
</p>
<br>

<p>
 <b>Now lets login to client-1 with wrong password 5 times.</b> 
  <li>Once locked go back to the domain controller > Right click > on mydomain > In the name field Type in the name of locked account and find.</li>
  <li>When the account pops up > double click on the name > Account > To unlock account check the box > Apply > OK.</li><br> 
<img width="1152" height="1038" alt="AD45" src="https://github.com/user-attachments/assets/be0c21f4-dc32-4f27-aef7-4b5e9755a592" />
<p/><br> 
  
<p>
 <b>Go back to client-1 and Attempt to log in.</b> 
    <li>You should be able to log in.</li> 
    <li>Double check user name in powershell.</li><br> 
<img width="1579" height="1026" alt="AD48" src="https://github.com/user-attachments/assets/1ef64418-d90b-41c5-b568-903bc0469a3b" />
  <img width="1686" height="999" alt="AD49" src="https://github.com/user-attachments/assets/00c78f38-e55d-4401-8375-0c266997efbc" />

</p><br> 

<p>
 <b>Now I will demonstrate how to reset password.</b> 
  <li>Go back to dc-1.</li>
  <li>Once locked go back to the domain controller > Right click > on mydomain > In the name field type in the name of the user account you need to reset.</li>
  <li>When the account pops up > right click > scroll down to reset and select it > You will be prompted to reset password >  In put new password then ok > This should reset the account.</li><br> 
<img width="1720" height="890" alt="AD50" src="https://github.com/user-attachments/assets/ec8509dd-73db-4053-82eb-db5a65c3af68" />
</p><br> 



<p>
 <b>Now I will demonstrate how to disable an account.</b> 
    <li>Go back to mydomain.com > Click Right > Find > Type in the the user account you what to disable > Find now.</li>
    <li>Scroll down to the user > Right Click > Disable the account.</li>
    <li>Now account should be disabled.</li>
    <li>To re-enable follow the same steps and click enable.</li><br> 
  <img width="1730" height="982" alt="AD51" src="https://github.com/user-attachments/assets/a4e21426-b792-4a0d-9882-39e3c0c6cf3e" />
</p>
<br>




<p>
   <b>Lastly, I will view event logs.</b>  
    <li>Go to windows menu type event viewer.</li>
    <li>Click on window log dropdown menu> Find > Type in user account you wish to observe.</li>
    <li>This will show you logins, logouts, failure etc.</li>
  <li>You can do the same process by login in client-1 > go to the start menu > event viewer > right click and run as administrator.</li>
    <li>This concludes the demonstration on group policies in Active directory.</li><br>  
<img width="1921" height="1044" alt="image" src="https://github.com/user-attachments/assets/38e317e6-1f50-4dfe-bb7d-824b43eba704" />
</p><br> 





