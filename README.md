# Active-Directory-Configurations-in-Azure
In this lab, I configured Active Directory using VMs in Azure
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

<h2>Deployment and Configuration Steps</h2>


<p>
<img width="1470" alt="admin and employees created" src="https://github.com/user-attachments/assets/50a1a00e-456d-4667-935a-686c5e96d100" />
<p>
<img width="1470" alt="jane doe" src="https://github.com/user-attachments/assets/29c5866b-3581-487d-8d8c-d7b0f8bffa9a" />
</p>
<p>
With Active Directory now installed on the domain controller VM, the next step is to create Organizational Units (OUs) and Users. Using the Active Directory Users and Computers console, I right-clicked on the domain I created and added two new OUs: _EMPLOYEES and _ADMINS. This naming scheme is intentional, as it will align with a PowerShell script used later.

Within the _ADMINS OU, I created a new user account named Jane Doe. To grant Jane administrative privileges, I used a Security Group. By right-clicking on Jane’s account and navigating to Properties > Member Of > Add, I added her to the Domain Admins security group. Moving forward, I will use Jane’s account for administrative tasks. I logged off as the default labuser and logged in as jane_admin.

c
<img width="1436" alt="adding DNS servers:static" src="https://github.com/user-attachments/assets/f6935baf-4a13-4bb6-98a2-181d564446fb" />

</p>
<p>
Before joining the client to the domain, the DNS settings must be configured to point to the domain controller's private IP address. In the Azure portal, navigate to the Networking tab and select the Network Interface for the client VM. Under DNS servers, enter the domain controller's private IP address and save the changes. To ensure the DNS settings are applied, restart the client VM.
</p>
<br />

<p>
<img width="1470" alt="Screenshot 2024-11-26 at 4 20 26 PM" src="https://github.com/user-attachments/assets/0804f472-7942-45ee-99d5-9df6f9f9671f" />

</p>
<p>
Next, it’s time to join the client VM to the domain. On the client VM, open the System menu, click on Rename this PC (advanced), and select Change. Enter the domain name and provide the necessary credentials to authenticate the connection. For this lab, I am logging in as Jane Doe. It’s important to input the login credentials in the correct domain path format. Once completed, the client VM will successfully join the domain. To confirm, navigate to the Active Directory Users and Computers panel on the domain controller, where the client VM will now appear under Computers.
</p>
<br />
Before domain users can access the client computer, Remote Desktop must be enabled for non-administrative users. While logged in as the administrator (Jane, in this case), open System Properties and navigate to the Remote Desktop section. Click on Select Users to specify who can remotely access the PC, and grant access to Domain Users. This allows non-administrative users to log in to the client computer (Client-1). Although this can typically be managed using a Group Policy to apply changes across multiple systems, for the purposes of this lab, a Group Policy will not be used.
</p>
<br />
<img width="1470" alt="Screenshot 2024-11-26 at 4 34 04 PM" src="https://github.com/user-attachments/assets/9a9b8229-87a3-4b2a-9626-b79877ad4529" />
<p>
<img width="1470" alt="Screenshot 2024-11-26 at 4 35 51 PM" src="https://github.com/user-attachments/assets/dcdf365a-5c6d-4244-b6f4-d5ae1da95ffd" />
</p>
<p>
Users can be created manually or through a script. For this lab, I will utilize a PowerShell script, which can be found <a href="https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1">here</a>
  On the domain controller, open PowerShell ISE as an administrator (ensuring you are logged in with an admin account on the domain controller). Create a new file, paste the script into the ISE editor, and run it. You will see the user accounts being created successfully.

  After creating the users, Client-1 can now be signed in as one of the new users that were created from the PowerShell script. Pick a name and simply sign in to the client with the context of the domain.

<h2>Lessons Learned</h2>

Completing this lab allowed me to gain hands-on experience in setting up Active Directory and joining client machines to a domain. I also learned how to create users and assign the necessary permissions. While Active Directory involves a fair amount of menu navigation, it is not difficult to grasp. This lab serves as a stepping stone for me to explore DNS settings in greater depth and understand file permissions in action, which I will cover in detail in future labs.











