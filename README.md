<p align="center">

</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
<p>This project outlines the implementation of on-premises Active Directory within Azure Virtual Machines.</p>

<p>In many organizations, the number of end users outweighs the number of resources available. Fortunately, tools like Active Directory allow you to centrally manage network resources so that resources can be shared.</p>

<p>This walkthrough provides a quick overview of how Microsoft Azure and Active Directory Domain Services can be set up so that multiple users can seamlessly remote desktop onto the same virtual desktop.</p>

<h2>Environments and Technologies Used</h2>

- Microsoft Azure Virtual Machines
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10, version 22H2

<h2>High-Level Deployment and Configuration Steps</h2>

- Microsoft Azure Setup
- Domain Controller Configuration
- Active Directory Users and Groups
- Client Machine DNS Setup
- Create Multiple Users with Powershell

<h2>Deployment and Configuration Steps</h2>

<h3>Microsoft Azure Setup</h3>
<p>In Microsoft Azure, we'll need two virtual machines.</p>

**Create the VMs**
<p>
On one, we'll run Windows Server, which we'll name "DC-1". On the other, we'll run Windows 10 and name "Client-1".Both VMs will need to share the same resource group and be configured onto the same VNET.
</p>

<img width="1079" alt="Screen Shot 2023-07-20 at 8 37 50 AM" src="https://github.com/yeahglo/configure-ad/assets/91516100/9c99b5f9-26cc-4e6c-a039-97b0c861e360">

**_The topology confirms both DC-1 and Client-1 are on the same VNET, named "DC-1-vnet"._**

</p>
Once both VMs have been created, we'll set the domain controller to have a static IP address. That way we'll ensure connectivity and communication between DC-1 and Client-1, as Client-1 will look to DC-1 to act as its DNS server.
<p>

<img width="477" alt="Screen Shot 2023-07-22 at 10 12 43 AM" src="https://github.com/yeahglo/configure-ad/assets/91516100/e76160cd-930e-42ea-a40b-a9bee25c5d6f">

**_The IP Configuration for DC-1's NIC private IP address needs to be static, inside Azure._**

<br/>

**Ensure Client-1 and DC-1 Connectivity**
<p>
Proper configuration is needed to ensure the client and domain controller can communicate. For this reason, we'll ping DC-1 from Client-1. The first attempts of our perpetual ping will be unsuccessful, but once we enable the firewall settings for ICMPv4, the ping requests will be successful.
</p>

<img width="958" alt="Screen Shot 2023-07-22 at 10 29 36 AM" src="https://github.com/yeahglo/configure-ad/assets/91516100/894b2bcd-8068-4966-8943-9bdb5fe64877">

**_At first, the Ping requests from Client-1 to DC-1 will be unsuccessful._**

<br/>

<img width="1278" alt="Screen Shot 2023-07-20 at 8 47 28 AM" src="https://github.com/yeahglo/configure-ad/assets/91516100/38efba49-ba22-4b8c-93f9-ae29a7d0d3c4">

**_In DC-1's VM, we'll enable the rules on the 2 ICMP Echo Requests to allow the pings to go through._**


<h3>Domain Controller Configuration</h3>

<p></p>

**Install Active Directory Services**

<p>
To configure DC-1 as a domain controller, we'll install Active Directory Domain Services and then promote the server as a domain controller. From there, we'll create a new forest and assign a domain.
</p>

<img width="789" alt="Screen Shot 2023-07-20 at 8 52 38 AM" src="https://github.com/yeahglo/configure-ad/assets/91516100/c6ae4e2a-66a5-444f-b10f-f839ea907201">

**_Active Directory Services will need to be installed onto DC-1._**

<br/>

<img width="760" alt="Screen Shot 2023-07-20 at 8 55 38 AM" src="https://github.com/yeahglo/configure-ad/assets/91516100/b7d722f6-308b-4835-803f-75aa85c67433">

**_We'll promote DC-1 as a domain controller to set up the domain in a new forest._**


<h3>Active Directory Users and Groups</h3>

**Creating Organizational Units**

<p>
With Active Domain Services (AD DS) installed, we'll have the capability to centrally manage users and admins. This is a crucial part of network management and data protection. Using the AD DS, we'll create 2 Organizational Units (OU), named "_ADMINS" and "_EMPLOYEES".
</p>

<img width="539" alt="Screen Shot 2023-07-20 at 9 05 52 AM" src="https://github.com/yeahglo/configure-ad/assets/91516100/02a22f31-0928-458e-8f29-d41766f0a01e">

**_Under the domain created, we'll create 2 organizational units for Admins and Employees._**

<br/>

**Creating an Admin and Normal User**

<p>
Within the OU "_ADMINS", we'll create an admin user called "jane-admin" and add them to the built-in Domain Admins group within AD DS.
</p>

<img width="668" alt="Screen Shot 2023-07-20 at 9 11 22 AM" src="https://github.com/yeahglo/configure-ad/assets/91516100/b33cd8f4-50fd-480d-b8a8-1865192142ef">

**_We'll create this admin manually and set the password._**

<img width="518" alt="Screen Shot 2023-07-20 at 9 13 11 AM" src="https://github.com/yeahglo/configure-ad/assets/91516100/1cd3f771-dd9d-4203-ab65-4f59e1dab9a1">

**_Then, we'll add "jane_admin" to the built-in Domain Admins group._**

<br/>

<h3>Client Machine DNS Setup</h3>

<p>
To share network resources, we'll need to join Client-1 to the domain. Doing this allows the domain to authenticate any user by using their domain credentials to log into Client-1.
</p>

<img width="643" alt="Screen Shot 2023-07-20 at 9 23 40 AM" src="https://github.com/yeahglo/configure-ad/assets/91516100/406081e3-8e12-42d0-88e2-15b32d0c8d52">

**_We'll configure Client-1's DNS server to DC-1's private IP address and then restart the VM._**

<br/>

<img width="1027" alt="Screen Shot 2023-07-20 at 9 27 05 AM" src="https://github.com/yeahglo/configure-ad/assets/91516100/e354500b-1814-4908-a042-4ec55359e3f0">

**_After restarting Client-1, we'll run an ipconfig/all to ensure the DNS server has updated._**

<br/>

<img width="1162" alt="Screen Shot 2023-07-20 at 9 28 10 AM" src="https://github.com/yeahglo/configure-ad/assets/91516100/6eb9c20a-af5a-47e7-ac95-d9ee4f8a3514">

**_In Client-1's systems settings, we'll reset the name for Client-1 to "mydomain.com" and restart Client-1._**

<br/> 

<h3>Setup Remote Desktop for Non-Admin Users</h3>

<p>
In live production, Group Policy within AD DS could be used to set up access for multiple users and systems. However, for this walkthrough, we'll use remote desktop under the system settings to allow access for any authorized user to log into Client-1 with their own credentials. 
</p>

<img width="494" alt="Screen Shot 2023-07-20 at 9 30 42 AM" src="https://github.com/yeahglo/configure-ad/assets/91516100/fae950fe-9940-42b6-b406-7a248f80980f">

**_Once Client-1 has restarted, we'll configure remote desktop._**

<br/>

<img width="665" alt="Screen Shot 2023-07-20 at 9 31 20 AM" src="https://github.com/yeahglo/configure-ad/assets/91516100/3ffc26bc-282d-426f-925b-31d8bcf5d288">

**_Then, we'll select the built-in group "Domain Users" to be able to log into Client-1._**

<br/>

<h3>Create 10,000 Users with Powershell</h3>

<p>
Using Powershell ISE, we'll create 10,000 users using a script. Domain Users already have access to Client-1. Once the users are created, we'll take a random user name and password and log into Client-1.
</p>

<img width="1213" alt="Screen Shot 2023-07-20 at 9 44 36 AM" src="https://github.com/yeahglo/configure-ad/assets/91516100/6adddd2c-70bb-449e-a146-0e9cbfe139ed">

**_New users go into the OU for employees so they'll be able to log into Client-1._**

<br/>

<!--
**New User Login**

<p>

</p>

<img width="832" alt="Screen Shot 2023-07-20 at 9 45 53 AM" src="https://github.com/yeahglo/configure-ad/assets/91516100/8ed2c8bf-7bdb-45db-b9ff-b4f7d5a810d2">

<img width="634" alt="Screen Shot 2023-07-20 at 9 46 57 AM" src="https://github.com/yeahglo/configure-ad/assets/91516100/0b633095-5091-4b77-a770-3bd8a951a358">

<img width="993" alt="Screen Shot 2023-07-20 at 9 47 49 AM" src="https://github.com/yeahglo/configure-ad/assets/91516100/45be4f18-2f7e-4e24-bbb5-09853b0d395c">

-->

