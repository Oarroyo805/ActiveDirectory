<p align="center">
<img src="https://i.imgur.com/pU5A58S.png"/>
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

<h2>High-Level Deployment and Configuration Steps</h2>

- Create Resources

- Ensure Connectivity between the client and Domain Controller

- Install Active Directory

- Create an Admin and Normal User Account in AD

- Join Client-1 to your domain (mydomain.com)

- Setup Remote Desktop for non-administrative users on Client-1

- Create additional users and attempt to log into client-1 with one of the users

<h2>Installation Steps</h2>

<p>

![1](https://github.com/user-attachments/assets/2a4b6bd9-6035-4805-8e3d-2f9064aaa437)

  
</p>
<p>
  
- Create a Resource group and name it "Active-Directory-Lab"
  
</p>
<br />

<p>

![2](https://github.com/user-attachments/assets/8e3c8a7e-e728-43cf-b91f-9c796e5cdbde)

</p>
<p>
  
- Create a new virtual network and name it "Active-Directory-Vnet"
  
- Make sure you put the virtual network in the right resource group (Active-Directory-Lab)
- After click review and create
  
</p>
<br />

<p>
  
![3](https://github.com/user-attachments/assets/8c2d010f-8012-4a28-ba7e-8d11b323ef7f)

</p>

<p>

- Then create the Domain Controller VM (Windows Server 2022) and name it “DC-1”
- Make sure for the image you choose " Windows Server 2022 Datacenter: Azure Edition"
- For the size option choose anything that has at least 2 vcpus
- Set username to : labuser
- Set password to : Cyberlab123!
- Next check the licensing box and click next on disk and then next for networking

</p>
<br />

<p>

![4](https://github.com/user-attachments/assets/27f65c0a-4e40-4ae9-9235-ec23a0c4461b)

</p>
<p>

  - Make sure the virtual network is on the vnet we created
  - Leave the subnet to default
  - Click review and create and create
 
</p>
<br />

<p>
  
![5](https://github.com/user-attachments/assets/193d2cf3-0b97-43f0-9da5-c7e991ed233c)

</p>
<p>

- Now create the client VM and name it "client-1"
- For the image use Windows 10 pro
- Again for the size use anything that has at least 2 vcpus
- For the username use : labuser
- For the password use : Cyberlab123!

</p>
<br />

<p>
  
![6](https://github.com/user-attachments/assets/bb49f3cb-61a0-4f3d-9fc7-a44f4b618212)

</p>
<p>

- Now go back to the Virtual Machine you named "DC-1" and you are going to set Domain Controller’s NIC Private IP address to be static
- In azure go to Virtual Machines and select DC-1 and go to its networking settings and then click on the Network Interface card to get into its settings and be able to change address to be static
</p>
<br />

<p>

![7](https://github.com/user-attachments/assets/77e4a01b-dab6-4ae5-b5d9-769af55b11ad)

</p>
<p>

- Click on ipconfig1 and edit IP configuration
- Switch from Dynamic to Static and save it

</p>
<br />

<p>
  
![8](https://github.com/user-attachments/assets/33f7bdf8-4338-4870-be21-e4f0ae18750d)

</p>
<p>

- After we are going to login to the Domain Controller and disable the Windows Firewall (for testing connectivity)
- Get DC-1 public's Ip address and remote in using Microsoft remote desktop and enter username and password you created
- Username: labuser
- Password: Cyberlab123!


</p>
<br />

<p>

![9](https://github.com/user-attachments/assets/5321e233-1cac-474b-8c8f-8c82c53c2637)

</p>
<p>

- Right click the start menu and select run and type wf.msc
- Next click on windows defender firewall properties
</p>
<br />

<p>
  
![10](https://github.com/user-attachments/assets/049b565b-1f92-447b-9711-4c5c99c07e0b)

</p>
<p>

- Then under domain profile, turn off option on Firewall state
- Go to next tab called private profile and turn off option on Firewall State
- Last go on public profile tab and turn off option there as well and select apply at the bottom and click ok
  
</p>
<br />

<p>

![11](https://github.com/user-attachments/assets/04c82e70-947e-4f35-a78b-19205fdde352)

</p>
<p>

- Now you are going to set Client-1’s DNS settings to DC-1’s Private IP address
- Get DC-1 private IP address by going into the Virtual Machines in Azure
- Next go to client-1 Network settings and click on Network Interface/ IP configuration

</p>
<br />

<p>

![12](https://github.com/user-attachments/assets/7e0700be-7016-4730-918c-ace6c0b58d7f)

</p>
<p>

- On the left, click on DNS servers
- Then select custom and enter DC-1 private Ip address and save it
- After you want to go restart client-1 to make the DNS settings become active so go into Virtual Machines in Azure and select client-1 and choose restart

</p>
<br />

<p>

![13](https://github.com/user-attachments/assets/0f094096-1d97-4901-83c1-855120225d6d)

</p>

<p>

- Next to see if you did everything right, attempt to ping DC-1’s private IP address and ensure the ping is successful
- Get DC-1 private IP address (Azure>Virtual Machines>DC-1
- Using RDP go back to Client-1
- Once in, open PowerShell and ping (DC-1 private IP address)
- If it pings successfully, you are good! If you get an error I recommend starting the lab all over again. 

</p>

<p>

![14](https://github.com/user-attachments/assets/d8f2751c-dfc0-4616-aa80-f3c8d2ed302e)

</p>
<p>

- Next you are going to Install Active Directory
- Login to DC-1 and install Active Directory Domain Services by selecting start and select Server Manager
- Then select add roles and features
- Select next until you get to Server Roles and add Active Directory Domain Services and select add features and select next after
- Before installing make sure you check the box that says "Restart the destination server automatically if required" and select yes after and install
<br />

<p>

![15](https://github.com/user-attachments/assets/9f006c77-a5b4-4164-a1e1-07f0f347ac3f)

</p>
<p>

- Next you are going to promote DC-1 as an actual domain controller: Setup a new forest as mydomain.com (can be anything, just remember what it is)
- Go into DC-1 and click " Promote this server to a domain controller"
- Select Add a new forest
- Root domain name : mydomain.com
- Select next and when you get to Restore Mode Password you can put Password1 (likely never going to use this)
- Click next and unselect " Create DNS Delegation" and then Next until you get to install
- Most likely the VM will restart. Restart and then log back into DC-1 as user: mydomain.com\labuser
- (Make sure you use the correct slash here "\" )
- Password is Cyberlab123!

</p>
<br />

<p>

![16](https://github.com/user-attachments/assets/4301a3df-9f54-47b4-bb1e-283f948dbcbd)

</p>
<p>

- Now you are going to create a Domain Admin user within the domain
- Click start and then select windows administrative tools and then select Active Directory Users and Computers

![17](https://github.com/user-attachments/assets/cd1fe03b-5454-41f0-8451-1c78222e1c15)

- Once in, right click mydomain.com and select new and choose Organizational Unit and name it _EMPLOYEES and click ok
- You are going to create another one so go back and this time name it _ADMINS

</p>
<br />

<p>

![18](https://github.com/user-attachments/assets/5b352cc4-4f7f-4eee-81fe-14d840487669)

</p>
<p>

- Now you are going to create a new employee named “Jane Doe” 
- Go to _ADMINS and right click and select new and then user
- Fill out first and last name "Jane Doe"
- The username of “jane_admin” and password Cyberlab123!
- uncheck "User must change password at next login"
- Check the box "Password never expires" for the sake of this lab ONLY

</p>
<br />

<p>

![19](https://github.com/user-attachments/assets/d37a42d8-e8f0-449b-aa28-e6af6b39c551)

</p>
<p>

- Next add jane_admin to the “Domain Admins” Security Group
- In Active Directory Users and Computers go to _ADMINS and right click on Jane Doe and select properties then go to the tab that says member of and add and enter domain admins and select check names after and click ok

![image](https://github.com/user-attachments/assets/3d4be82b-81ac-4ff1-8082-ce874fb8f223)


</p>
<br />

<p>

![image](https://github.com/user-attachments/assets/25eb3a5c-8ae0-4af6-833d-20d7b24b093f)

</p>
<p>

- Now log out / close the connection to DC-1 and log back in as “mydomain.com\jane_admin”

![image](https://github.com/user-attachments/assets/90e797fb-dd78-4076-a2eb-aa726f798915)

<br />

<p>

![image](https://github.com/user-attachments/assets/31ee378f-1210-449a-b0e7-348cc53d1943)

</p>
<p>

- Reconnect to DC-1 using remote desktop (Get DC-1 public IP adress if you dont have it in Azure>Virtual Machines)

</p>
<br />

<p>

![image](https://github.com/user-attachments/assets/fe0a52c3-8aeb-4552-b509-b0db7908d812)

</p>
<p>

- Next you are going to join Client-1 to your domain (mydomain.com)
- Get client-1 public IP adress and login using remote desktop
- Once you login, right click the start menu and select system
- Under Related Settings (right hand side) click on Rename this PC (Advanced)
- Under the computer name tab click on change and then switch it from Workgroup to Domain
- Enter mydomain.com in the domain and select ok after
</p>
<br />

<p>

![image](https://github.com/user-attachments/assets/0b1ebf84-815b-44d9-83fa-bb445fc1fdb8)

</p>
<p>

- Next join the domain with "mydomain.com\jane_admin" credentials
- Restart client-1 VM after

</p>
<br />

<p>

![image](https://github.com/user-attachments/assets/21e19c8b-1c26-44fc-bf60-1fff35a7a4da)

</p>
<p>

- Go back to DC-1
- Next create a new Organizational Unit named “_CLIENTS” and drag Client-1 into the Domain
- Right click mydomain.com
- Select New and then Organizational Unit
- Name it _CLIENTS and select ok
</p>
<br />

<p>

![image](https://github.com/user-attachments/assets/54450142-0e6f-42e2-a3b3-fa0beb1f69be)

</p>
<p>

- After click on Computers and drag client-1 to _CLIENTS
- When a message appears just click yes
- Next right click on mydomain.com and select refresh to organize everything
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
