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

<h2>Deployment and Configuration Steps</h2></br>

<b>Setup Domain Controller in Azure</b></br>
First, we'll go to Azure. There we will set up a domain controller, which is a type of server that handles authentication, authorization, and other identity management tasks.
1. In Azure, create a Resource Group by clicking on <b>Resource Groups</b> and then <b>+ Create</b>.
2. Make a note of the <b>Region</b> (you'll need it later). Name the resource group <em>Active-Directory-1</em>. Click on the blue `Review + Create` button.

<p>
<img src="https://github.com/user-attachments/assets/37b4b6a7-b450-419f-a136-b867bbc70f19" height="80%" width="80%" alt="Domain Controller Setup"/>
</p></br>

<b>Create a Virtual Network</b></br>
Next, we'll need to create a Virtual Network. We'll need this in order to tie the domain controller and the client computer in a network.
1. In Azure, enter "virtual networks" in the search bar and select.
2. Click the <b>+ Create</b> tab. Name the virtual network <em>Active-Directory-VNet</em>.
3. Make sure the resource group is <em>Active-Directory-1</em> and that the selected Region is the same as the Resource Group's.
4. Click on the blue `Review + Create` button and then `Create`.

<p>
<img src="https://github.com/user-attachments/assets/86255197-2661-4b56-8401-9f0bae7e2385" height="80%" width="80%" alt="Virtual Network Setup"/>
</p></br>

<b>Create the Domain Controller VM</b></br>
Now we'll create a virtual machine that will serve as the domain controller.
1. In Azure, navigate to "virtual machines". Click on the <b>+ Create</b> tab and select <b>Azure virtual machine</b>.
2. Make sure the resource group is <em>Active-Directory-1</em>. Name the VM <b>DC-1</b> and make sure the selected Region is the same as the Resource Group's.
3. For <b>Image</b> select <b>Windows Server 2022 Datacenter</b>. For size, select a size that has at least 2 vcpus.
4. Should the username and password be the same as the client-1 machine? Click `Next` until you get to the <b>Networking</b> section.
5. In the <b>Networking</b> section, make sure the <b>Virtual network</b> is <em>Active-Directory-VNet</em>.
6. Click on the blue `Review + Create` button and then `Create`.

<p>
<img src="https://github.com/user-attachments/assets/3e16cc92-a486-47d2-ad01-a11670d8189d" height="80%" width="80%" alt="Domain Controller Creation"/>
</p></br>


<b>Set Domain Controller’s NIC Private IP address to be static</b></br>
In order to avoid disrupting clients' hability to connect to the domain controller, we need a static domain controller IP.
1. In Azure, enter <em>virtual machines</em> in the search bar and click on the <b>DC-1</b> name.
2. On the left-side panel, click on <b>Networking</b> > <b>Network settings</b>
3. Click on the <b>Network interface / IP configuration</b> link at the top
4. In the <b>IP Settings</b> section, click on the <b>Name</b> at the bottom (in this case it's <em>ipconfig1</em>).
5. Select the <b>Static</b> radial button and click `Save`.

<p>
<img src="https://github.com/user-attachments/assets/bc764d2d-8acd-47d9-9f5a-f9317086f656" height="80%" width="80%" alt="Domain Controller NIC"/>
</p></br>


<b>Disable Windows Firewall</b></br>
For testing connectivity, we need to disable Windows Firewall.
1. In the domain controller virtual machine, type <b>wf.msc</b> in the search box and select.
2. Click the link that says <b>Windows Defender Firewall Properties</b>
3. In the `Domain Profile` tab, change the <b>Firewall state</b> to <b>Off</b>
4. In the `Private Profile` tab, change the <b>Firewall state</b> to <b>Off</b>
5. In the `Public Profile` tab, change the <b>Firewall state</b> to <b>Off</b>
6. Click `Apply` and `OK`. Close the <b>Firewall</b> window.

<p>
<img src="https://github.com/user-attachments/assets/69c8f95e-71c7-4899-855e-256e9301befe" height="80%" width="80%" alt="Disable Windows Firewall"/>
</p></br>


<b>Set up <b>Client-1</b> in Azure</b></br>
Now we'll create the Client VM. We'll name it <b>Client-1</b>
1. Back in Azure, create a VM that's in the same <b>Resource Group</b> and <b>Region</b> as the domain controller.
2. We'll use the same <b>Username</b> and <b>Password</b> as the domain controller.
3. For <b>Image</b> select <b>Windows 10 Pro</b>. For size, select a size that has at least 2 vcpus.
4. Click on the blue `Review + Create` button and then `Create`.

<p>
<img src="https://github.com/user-attachments/assets/8ef72746-0415-409c-ad2c-2698cd85c06d" height="80%" width="80%" alt="Set up Client-1"/>
</p></br>

<b>Set Client-1’s DNS settings to DC-1’s Private IP address</b></br>
Now we need to get Client-1 in the same network as DC-1
1. Back in Azure, type <b>Virtual Machines</b> in the search bar and click on DC-1
2. In the window that will open, look for DC-1's Private IP address under <b>Networking</b> and take note of it.
3. In Azure still, go back to the Virtual machines window and click <b>Client-1</b>
4. On the left side, under <b>Networking</b>, click on <b>Network settings</b> 
5. Go to the <b>Network interface / IP configuration</b> link at the top and click it
6. Click on <b>DNS servers</b> on the left side panel
7. Click on <b>Custom</b> and type DC-1's private IP address in the box. In this case it's 10.0.0.4. Click `Save`.

<p>
<img src="https://github.com/user-attachments/assets/711634b9-28e6-4d8e-8b18-c2f20f68a404" height="80%" width="80%" alt="Set up Client-1"/>
</p></br>


<b>Restart Client-1</b></br>
Back in the <b>Virtual Machines</b> window, tick the box next to the <b>Client-1</b> vm and click `Restart` at the top

<p>
<img src="https://github.com/user-attachments/assets/82d92f53-a57e-4bb0-b885-8eddea29c859" height="80%" width="80%" alt="Set up Client-1"/>
</p></br>


<b>Ping the Domain Controller from Client-1</b></br>
Now we need to verify that Client-1 and DC-1 are on the same network
1. Remote into the <b>Client-1</b> vm
2. Open <b>Windows Powershell</b> by typing <em>Powershell</em> in the search bar and select <b>Run as administrator</b>
3. Type `ping 10.0.0.4` and press <b>Enter</b>. You should see four replies from <b>DC-1</b>

<p>
<img src="https://github.com/user-attachments/assets/916cc4c5-c26c-4265-98cb-ce49aaa7236c" height="80%" width="80%" alt="Set up Client-1"/>
</p></br>


<b>View Client-1's Network Configurations</b></br>
To further verify that <b>Client-1</b> is on the same network as <b>DC-1</b>:
1. While still in <b>Windows Powershell</b> type `ipconfig /all` and press <b>Enter</b>
2. Look for <b>DNS Servers</b> at the bottom and you'll see <b>DC-1's</b> Private IP address


<p>
<img src="https://github.com/user-attachments/assets/8b485c3a-2b91-413a-af04-c6df105633e4" height="80%" width="80%" alt="Set up Client-1"/>
</p></br>










