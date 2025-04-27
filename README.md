<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
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
- Windows 10 (22H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Domain Controller in Azure (Server)
- Setup Client-1 in Azure (Host Computer)
- Install Active Directory in Domain Controller
- Step 4

<h2>Deployment and Configuration Steps</h2>

<b>Setup Domain Controller in Azure</b></br>
First, we'll go to Azure. There we will set up a domain controller, which is a type of server that handles authentication, authorization, and other identity management tasks.
1. In Azure, create a Resource Group by clicking on <b>Resource Groups</b> and then <b>+ Create</b>.
2. Make a note of the <b>Region</b> (you'll need it later). Name the resource group <em>Active-Directory-1</em>. Click on the blue `Review + Create` button.

<p>
<img src="https://github.com/user-attachments/assets/37b4b6a7-b450-419f-a136-b867bbc70f19" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p></br>

<b>Create a Virtual Network</b></br>
Next, we'll need to create a Virtual Network. We'll need this in order to tie the domain controller and the client computer in a network.
1. In Azure, enter "virtual networks" in the search bar and select.
2. Click the <b>+ Create</b> tab. Name the virtual network <em>Active-Directory-VNet</em>.
3. Make sure the resource group is <em>Active-Directory-1</em> and that the selected Region is the same as the Resource Group's.
4. Click on the blue `Review + Create` button and then `Create`.

<p>
<img src="https://github.com/user-attachments/assets/86255197-2661-4b56-8401-9f0bae7e2385" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p></br>

<b>Create the Domain Controller VM</b></br>
Now we'll create a virtual machine that will serve as the domain controller.
1. In Azure, navigate to "virtual machines" and click on the <b>+ Create</b> tab and select <b>Azure virtual machine>/b>.
2. Click the <b>+ Create</b> tab. Name the virtual network <em>Active-Directory-VNet</em>.
3. Make sure the resource group is <em>Active-Directory-1</em> and that the selected Region is the same as the Resource Group's.
4. Click on the blue `Review + Create` button and then `Create`.

<p>
<img src="https://github.com/user-attachments/assets/86255197-2661-4b56-8401-9f0bae7e2385" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p></br>







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
