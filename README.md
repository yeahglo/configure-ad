<!--
<p align="center">

</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
<p>This project outlines the implementation of on-premises Active Directory within Azure Virtual Machines.</p>

<p>In many organizations, the number of end users outweigh the number of resources available. Fortunately, there are tools like Active Directory that allow you to centrally manage network resources so that resources can be shared.</p>

<p>This walkthrough provides a high-level overview of how to implement Microsoft Azure and Active Directory Domain Services so that multiple users can seamlessly remote desktop onto the same virtual desktop.</p>

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
- End User Access

<h2>Deployment and Configuration Steps</h2>

<p>
In Microsoft Azure, we'll need two virtual machines, one Windows Server and the other running Windows 10. We'll name the server "DC-1" and it will be domain controller. "Client-1" will be the end-user computer running Windows 10. Both VMs will need to share the same resource group and be configured onto the same VNET.
</p>

<img width="1079" alt="Screen Shot 2023-07-20 at 8 37 50 AM" src="https://github.com/yeahglo/configure-ad/assets/91516100/9c99b5f9-26cc-4e6c-a039-97b0c861e360">

**_The topology confirms both DC-1 and Client-1 are on the same VNET, named "DC-1-vnet"._**

<p>

</p>
The domain controller needs to have a static IP address to ensure connectivity and communication between DC-1 and Client-1.
<p>

<img width="897" alt="Screen Shot 2023-07-20 at 8 35 05 AM" src="https://github.com/yeahglo/configure-ad/assets/91516100/b68114d6-6520-478b-a592-8a14a898f15d">

**_DC-1 has a private IP address and it is set to static within Azure._**

-->
