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

<h2>High-Level Deployment and Configuration Steps</h2>

- Install Active Directory Domain Services (AD DS) on DC-1
- Create a Domain Admin User
- Join Client-1 to the Domain
- Enable Remote Desktop for Domain Users
- Setup Remote Desktop and Create Users
- Create Multiple User Accounts with a Script

<h2>Deployment and Configuration Steps</h2>

Before setting up Active Directory, we first need to create two virtual machines on Azure. One will be a Windows Server to serve as the domain controller, and the other will be a Windows Pro machine to act as the client. We can name the server machine "DC-1" and the client machine "Client-1." It's important to ensure that both machines are connected to the same virtual network.

Once everything is set up, we'll log into "DC-1" to proceed.

<h2>Install Active Directory Domain Services (AD DS) on DC-1s</h2>

<p>

</p>
<img width="450" alt="1st server manager image" src="https://github.com/user-attachments/assets/14783c61-9e06-4345-be06-5aa63fb4d3b8" />
<img width="450" alt="beginning add role" src="https://github.com/user-attachments/assets/2867cb99-8fd2-424e-a854-526e54608441" />
<img width="450" alt="add role (role based) (installation type)" src="https://github.com/user-attachments/assets/75bedcc4-5bcc-4ddf-b674-b7c697bd8e20" />
<img width="450" alt="add role (server selection)" src="https://github.com/user-attachments/assets/862303f1-8665-479e-bdab-be4ed635ee74" />
<img width="450" alt="add roles active directory add features" src="https://github.com/user-attachments/assets/570b03d3-a481-4bf7-b652-9cde582856b9" />

<img width="450" alt="add role active directory domain services" src="https://github.com/user-attachments/assets/8e79f672-d008-41f1-ba7a-f0c653c0a418" />

<img width="450" alt="add roles (features)" src="https://github.com/user-attachments/assets/e955ea4c-37b4-4148-a0fa-04fe9494c296" />

<img width="450" alt="Add role server manager install confirm" src="https://github.com/user-attachments/assets/b8c9758e-42fb-450c-84bf-474f85f8979c" />

<img width="450" alt="add roles (results)" src="https://github.com/user-attachments/assets/586972ad-6640-4f1f-9d19-bc1f9c2b403e" />

<img width="450" alt="adding new forrest" src="https://github.com/user-attachments/assets/35fe86d3-eb48-4408-81bb-79d4ea30f729" />


<p>
Using Server Manager on DC-1, I install the Active Directory Domain Services role, promote DC-1 to a Domain Controller, and create a new forest with a root domain. This sets up the foundation for the Active Directory environment.</p>
<br />


<h2>Create a Domain Admin User</h2>


<p>
<img width="450" alt="acd users n cpu (Users)" src="https://github.com/user-attachments/assets/2e5c3cc1-fa93-4709-baa2-93fa6441be0d" />
<img width="450" alt="Add folder _ADMIN _EMPLOYEES" src="https://github.com/user-attachments/assets/7e674d42-a9bf-4a8e-b595-044422c525f3" />
<img width="450" alt="adding new user (Jane)" src="https://github.com/user-attachments/assets/138fbdb2-20b0-4256-83c5-81529a3da2fd" />
<img width="450" alt="adding user (Jane doe)" src="https://github.com/user-attachments/assets/bb6c4d76-d4f1-4cef-81b2-8df5ed6adc6b" />
<img width="450" alt="adding folder (_ADMIN _EMPLOYEES) top list" src="https://github.com/user-attachments/assets/15e19bd7-8689-4a72-b2de-0abc6c9bdf4e" />

</p>
<p>
In the Active Directory Users and Computers (ADUC) tool, I create two Organizational Units (OUs) named "_EMPLOYEES" and "_ADMINS." Within the "_ADMINS" OU, I add a new user with administrative privileges, assigning them to the Domain Admins group to manage the domain.</p>
<br />


<h2>Join Client-1 to the Domain</h2>


<p>
<img width="450" alt="jane logging in" src="https://github.com/user-attachments/assets/89863708-bee6-40ce-a3f1-b116bd680f92" />
<img width="450" alt="created mydomain com" src="https://github.com/user-attachments/assets/0e3d5530-f442-418c-b43d-007ef48fc565" />
<img width="450" alt="client-1 in computer folder" src="https://github.com/user-attachments/assets/c1435533-2b58-4fc1-8654-78c260cee8e8" />
<img width="450" alt="client-1 in _CLIENTS folder" src="https://github.com/user-attachments/assets/cc81bd5a-988f-4b04-aaf6-da65f6d96947" />

</p>
<p>
In the Azure portal, I ensure Client-1’s DNS settings point to DC-1’s private IP, restart the VM, and log in as labuser to join it to the domain mydomain.com. After another restart, I confirm in ADUC that Client-1 appears in the domain. I create a new OU named _CLIENTS and move Client-1 into it. This integrates Client-1 into the domain and organizes it within the directory for easier management.

</p>
<br />


<h2>Setup Remote Desktop and Create Users</h2>


<p>
<img width="700" alt="remote desktop user" src="https://github.com/user-attachments/assets/ffb5e494-3a1b-4bd2-b297-f997a7b8abe5" />


</p>
<p>
I log in to Client-1 as mydomain.com\jane_admin, open System Properties, and allow Remote Desktop access for all domain users. This configuration allows non-administrative users to remotely access Client-1, providing them with secure remote access capabilities.
</p>
<br />

<h2>Create Multiple User Accounts with a Script</h2>

<p>
<img width="550" alt="adding users in powershell" src="https://github.com/user-attachments/assets/7baa1ec6-a33a-4166-8653-7649de4f7315" />
<img width="550" alt="picking user to login with" src="https://github.com/user-attachments/assets/7e385a0a-b63e-4b13-a56e-fbb630c9c5ce" />


</p>
<p>
On DC-1, I log in as jane_admin, open PowerShell ISE, and run a provided script to generate multiple user accounts. I verify the accounts in the _EMPLOYEES OU in ADUC and test one by logging into Client-1 with its credentials. Using a script streamlines the user creation process and ensures the accounts are correctly configured for domain use.</p>
<br />


