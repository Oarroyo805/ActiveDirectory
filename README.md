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
- If it pings successfully, congratulations you are done!

</p>
