
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Overview of Deployment and Configuration Steps</h2>

<h3>Setup Domain Controller in Azure</h3>

- Create a Resource Group
- Create a Virtual Network and Subnet
- Create the Domain Controller VM (Windows Server 2022) named “DC-1”
- Username: labuser
- Password: Cyberlab123!
- After VM is created, set Domain Controller’s NIC Private IP address to be static

<h3>Setup Client-1 in Azure</h3>

- Create the Client VM (Windows 10) named “Client-1”
- Username: labuser
- Password: Cyberlab123!
- Attach it to the same region and Virtual Network as DC-1
- After VM is created, set Client-1’s DNS settings to DC-1’s Private IP address
- From the Azure Portal, restart Client-1
- Login to Client-1
- Attempt to ping DC-1’s private IP address
- Ensure the ping succeeded
- From Client-1, open PowerShell and run ipconfig /all
- The output for the DNS settings should show DC-1’s private IP Address
  

<h2>Deployment and Configuration Steps</h2>

<h3>1) Install Active Directory on to DC-1</h3>
<p>
  
![image](https://github.com/user-attachments/assets/2b795c35-c572-4bcc-a139-f248dd16db78)  
</p>
<p>
Click start and open "Server Manager"
</p>

<p>
  
![image](https://github.com/user-attachments/assets/28c9b109-6b02-4c13-b422-fe7a73c81376)
</p>
<p>
Select "Add roles and features"
</p>

<p>
  
![image](https://github.com/user-attachments/assets/da588ff8-4b05-4b17-9ad6-af82f71d9950)
</p>
<p>
Click "Next" until you reach "Server Roles".Click the box next to "Active Directory Domain Services" and select "Add Features"
</p>

<p>

![image](https://github.com/user-attachments/assets/20cfe9f1-177e-43e4-84f2-b15a0f13879c)  
</p>
<p>
Clcik next until you reach "Confirmation". Check the box at the top and click "Yes". Click "Install"
</p>

<h3>2) Promote DC-1 to a DC</h3>

<p>
  
![image](https://github.com/user-attachments/assets/47e079e3-3c7e-4952-b784-1b27f84e027d)
</p>
<p>
Return to the Server Manager Dashboard. Clcik the flag at the top and select "Promote this server to a domain controller"
</p>

<p>
  
![image](https://github.com/user-attachments/assets/91393966-5521-4232-ab11-dd5930bc1bc5)
</p>
<p>
  Select "Add a new forest"
  
  - Root domain name: mydomain.com

Click "Next"

- Password: Password1

Click next and select "Install" 

Select "Close" and log back into DC-1 after a moment (IMPORTANT: When RDP'ing back into DC-1, make sure to type "mydomain.com\labuser" into the username)
</p>

<h3>3) Create a Domain Admin user within the domain</h3>

<p>
  
![image](https://github.com/user-attachments/assets/cae114a0-404e-4e98-82ad-c634d2df32fd)
</p>
<p>
Click the start menu. Expand "Windows Administrative Tools". Select "Active Directory Users and Computers"
</p>

<p>
  
![image](https://github.com/user-attachments/assets/b354c9e8-4ba0-44f1-a4ec-46f2bb86882d)
</p>
<p>
Right-click "mydomain.com". Select "New" --> "Organizational Unit". Namwe the OU "_EMPLOYEES". Perform the same tasks and make another OU named "_ADMINS"
</p>

<p>

![image](https://github.com/user-attachments/assets/037a165d-f48d-4a55-845d-6026a9fdee68)
</p>
<p>
Click the "_ADMINS" OU and select "New" --> "User"

- First name: Jane
- Last name: Doe
- User logon name: jane_admin
- Password: Cyberlab123!
- Uncheck the box that says "User must change password at next logon"
- Check the box that says "Password never expires"
- Click "Finish"
</p>

<p>

![image](https://github.com/user-attachments/assets/0a97e4fc-63e7-45a7-9c77-8eb4bc2e4809)
</p>
<p>
Click the "_ADMINS" OU. Right-click "Jane Doe" and select properties
</p>

<p>

![image](https://github.com/user-attachments/assets/3752608a-fdb7-4c20-9d18-1aea8cfdbb8d)
</p>
Select "Member Of", and click "Add"

<p>

![image](https://github.com/user-attachments/assets/0def8b20-11bd-449f-8d0e-2aaab55daf49)
</p>
<p>
Type "domain admins" into the text bar and click "Chheck Names". Click  "OK". Click "Apply". Click "OK"
</p>

<h3>4) Log out / close the connection to DC-1 and log back in as “mydomain.com\jane_admin”</h3>

 <h3>5) Join client-1 to the domain</h3>

 <p>

![image](https://github.com/user-attachments/assets/8fbadf8e-b4eb-4356-8c0b-576d219735c0)
</p>
<p>
Clcick on the start menu and select "System"
</p>

<p>

![image](https://github.com/user-attachments/assets/7aa7064f-64b5-46fd-a06a-c85c77cad770)
</p>
<p>
Click "Rename this PC (advanced)". Select "Change"
</p>

<p>

![image](https://github.com/user-attachments/assets/050af6ed-416c-4b12-b3b7-e9cdd5e16ca6)
</p>
<p>
Select "Domain:" underneath "Memeber of". Type "mydomain.com" into the text bar and click "OK"

- Username: mydomain.com\jane_admin
- Password: Cyberlab123!
</p>
<p> 
Minimize the "Settings" tab. Select "OK" and "Close" on all of the pop up tabs. Click "Restart Now" 
</p>


