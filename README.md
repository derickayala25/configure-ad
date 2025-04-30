![image](https://github.com/user-attachments/assets/5c340900-dc0d-41c6-aa9e-49c0022d09e4)<p align="center">
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
<img src="https://github.com/user-attachments/assets/711634b9-28e6-4d8e-8b18-c2f20f68a404" height="80%" width="80%" alt="Change DNS Settings"/>
</p></br>


<b>Restart Client-1</b></br>
Back in the <b>Virtual Machines</b> window, tick the box next to the <b>Client-1</b> vm and click `Restart` at the top

<p>
<img src="https://github.com/user-attachments/assets/82d92f53-a57e-4bb0-b885-8eddea29c859" height="80%" width="80%" alt="Restart Client-1"/>
</p></br>


<b>Ping the Domain Controller from Client-1</b></br>
Now we need to verify that Client-1 and DC-1 are on the same network
1. Remote into the <b>Client-1</b> vm
2. Open <b>Windows Powershell</b> by typing <em>Powershell</em> in the search bar and select <b>Run as administrator</b>
3. Type `ping 10.0.0.4` and press <b>Enter</b>. You should see four replies from <b>DC-1</b>

<p>
<img src="https://github.com/user-attachments/assets/916cc4c5-c26c-4265-98cb-ce49aaa7236c" height="80%" width="80%" alt="Ping DC-1"/>
</p></br>


<b>View Client-1's Network Configurations</b></br>
To further verify that <b>Client-1</b> is on the same network as <b>DC-1</b>:
1. While still in <b>Windows Powershell</b> type `ipconfig /all` and press <b>Enter</b>
2. Look for <b>DNS Servers</b> at the bottom and you'll see <b>DC-1's</b> Private IP address
3. To exit <b>Powershell</b> you can type <b>exit</b> and press <b>Enter</b> or just close the window


<p>
<img src="https://github.com/user-attachments/assets/8b485c3a-2b91-413a-af04-c6df105633e4" height="80%" width="80%" alt="Network Configurations"/>
</p></br>


<b>Installing Active Directory</b></br>
Now that we have the <b>Client-1</b> VM connected to the Domain Controller, we will install Active Directory on DC-1
1. Type <em>Server Manager</em> in the search box and select
2. Click on selection number 2, <b>Add roles and features</b>
3. Click `Next` until you get to the <b>Server Roles</b> section
4. On the <b>Server Roles</b> section click on <b>Active Directory Domain Services</b>
5. A window will open. Click the `Add Features` button. The window will close.
6. Click `Next` on the <b>Server Roles</b> section and again until you get to the <b>Confirmation</b> section
7. In the <b>Confirmation</b> section check the box that says <em>Restart the destination server automatically if required</em>
8. Click `Yes` on the pop-up window, and click `Install`. Once finished, click `Close`.


<p>
<img src="https://github.com/user-attachments/assets/d9eee87e-b780-4c1e-a1c6-7bf1cfb224ea" height="80%" width="80%" alt="Installing AD"/>
</p></br>


<b>Promote the DC-1 VM/Server as a Domain Controller</b></br>
Now we'll make <b>DC-1</b> a Domain Controller, which is a server that hosts the Active Directory Domain Services
1. On the <b>Server Manager</b> window you will see a yellow notifications triangle at the top. Click on it and click the link that says <em>Promote this server to a domain controller</em>.
2. On the <b>Active Directory Domain Services Configuration Wizard</b> window click on the <b>Add a new forest</b> radial button
3. In the <b>Root domain name:</b> box type <em>mydomain.com</em> and click `Next`
4. In the <b>Domain Controller Options</b> section enter a password and confirm. Click `Next`.
5. In the <b>DNS Options</b> section UNCHECK the <b>Create DNS delegation</b> box. Click `Next`.
6. In the “DNS Options” section UNCHECK the “Create DNS delegation” box. Click “Next”
7. Click `Next` until you get to the <b>Prerequisites Check</b> section
8. Once done running the prerequisites check, click `Install`
9. When done, the VM will restart automatically, therefore you will have to remote in again

<p>
<img src="https://github.com/user-attachments/assets/e2b888b3-434f-4454-ba82-a216f8698b0e" height="80%" width="80%" alt="Promoting DC-1"/>
</p></br>


<b>Logging into DC-1 as a domain user</b></br>
Now we'll log in to <b>DC-1</b> as a domain user by using <em>mydomain.com\your username</em>
1. Remote in to <b>DC-1</b>
2. For <b>Username</b> type <em>mydomain.com\your username</em> and use the password you used to create the VM

<p>
<img src="https://github.com/user-attachments/assets/a3fd60ef-8822-4208-bf76-f2b21ebe3306" height="80%" width="80%" alt="Domain User Login"/>
</p></br>

<p>
<img src="https://github.com/user-attachments/assets/dd2dff72-6ae0-4032-9629-3b5354a7dcb1" height="80%" width="80%" alt="DC-1 Login"/>
</p></br>

<b>Create Organizational Units within the domain</b></br>
Organizational Units are containers used to organize and manage users, computers, groups, and other objects
1. Type <em>Active Directory Users and Computers</em> in the search box and select
2. On the left side-panel of the window, right-click on <b>mydomain.com</b> and select `New` > `Organizational Unit`
3. Type <b>_EMPLOYEES</b> in the <b>Name</b> box and click `OK`
4. Right-click once more on <b>mydomain.com</b> and select `New` > `Organizational Unit`
5. Type <b>_ADMINS</b> in the <b>Name</b> box and click `OK`
6. Right-click on <b>mydomain.com</b> and click `Refresh` in order to organize the folders

<p>
<img src="https://github.com/user-attachments/assets/f1f31fee-e979-4d95-9443-0fc966ad261e" height="80%" width="80%" alt="Organizational Units"/>
</p></br>


<b>Create a Domain Admin user within the domain</b></br>
The Domain Administrator in Active Directory has the highest level of access and control over the network domain. 
Let's create a new employee/administrator named <b>Jane Doe</b>.
1. Right-click on the <b>_ADMINS</b> folder and select `New` > `User`
2. Under <b>First name</b> type <em>Jane</em>, <b>Last name</b> will be <em>Doe</em>. <b>User logon name</b> will be <em>jane_admin</em>. Click `Next`.
3. For simplicity's sake, use the same password used when creating the VM.
4. UNCHECK the box that says <b>User must change password at next logon</b> and CHECK the box that says <b>Password never expires</b>
5. Click `Next` and `Finish`

<p>
<img src="https://github.com/user-attachments/assets/010663b9-761a-4ee2-b125-2e315c2fcb87" height="80%" width="80%" alt="Domain Admin"/>
</p></br>


<b>Add jane_admin to the Domain Admins Security Group</b></br>
Now that Jane Doe is in the <b>_ADMINS</b> organizational unit, we need to add her to the <b>Domain Admins</b> Security Group.
1. Back in the <b>Active Directory Users and Computers</b> window, click on the <b>_ADMINS</b> folder
2. Inside the right side-panel, right-click the username <b>Jane Doe</b> and select `Properties`
3. Click on the `Member Of` tab and click `Add`
4. Type <em>Domain Admins</em> in the <b>Enter the object names to select (examples)</b> box.
5. Click `Check Names` and click `OK`
6. In the <b>Jane Doe Properties</b> window, click `Apply` and then `OK`

<p>
<img src="https://github.com/user-attachments/assets/20e55d6c-4844-4224-8574-6f9ffb44b1b8" height="80%" width="80%" alt="Domain Admin SG"/>
</p></br>


<b>Log out of DC-1 and log back in as mydomain.com\jane_admin</b></br>
Use <b>jane_admin</b> as your admin account from now on

<p>
<img src="https://github.com/user-attachments/assets/6a022d87-7277-40d1-a061-c76a4e702e4e" height="80%" width="80%" alt="Logging in as Admin"/>
</p></br>


<b>Join Client-1 to your domain (mydomain.com)</b></br>

1. Log in to the <b>Client-1</b> VM as the original local admin
2. Once logged in, right-click on the <b>Start</b> icon and select <b>System</b>
3. On the right side of the window you will see a link that says <b>Rename this PC (advanced)</b>, click it
4. A window will open named <b>System Properties</b>, click on `Change`
5. A window named <b>Computer Name/Domain Changes</b> will open. Select <b>Domain</b> and type <em>mydomain.com</em>. Click `OK`
6. A <b>Windows Security</b> window will open. Type <em>mydomain.com\jane_admin</em> as User name and then type your password. Click `OK`.
7. A pop-up window will appear (most likely behind another window) that says <b>Welcome to the mydomain.com domain.</b> Click `OK`.
8. Another pop-up window will appear that says <b>You must restart your computer to apply these changes</b>. Click `OK`.
9. Click `Close` in the <b>System Properties</b> window. Close the <b>Settings</b> window.
10. Click on `Restart Now`

<p>
<img src="https://github.com/user-attachments/assets/6d34e54c-fdae-4d5a-af62-a6c96c39c6d1" height="80%" width="80%" alt="Joining Client-1 to the Domain"/>
</p></br>


<b>Verify Client-1 shows up in Active Directory Users and Computers</b></br>

1. Back in the DC-1 VM, go to <b>Active Directory Users and Computers</b>
2. Expand <b>mydomain.com</b>, click on the <b>Computers</b> folder and you should see <b>Client-1</b> on the right side-panel.
3. On the right side of the window you will see a link that says <b>Rename this PC (advanced)</b>, click it
Create a new OU named “_CLIENTS” and drag Client-1 into there
<p>
<img src="https://github.com/user-attachments/assets/82ff4f85-e165-447c-a1ea-06edbc4921cc" height="80%" width="80%" alt="Client-1 in AD"/>
</p></br>


<b>Create a new Organizational Unit _CLIENTS and drag Client-1 into it</b></br>

1. Right-click on <b>mydomain.com</b> and select `New` > `Organizational Unit`
2. Type <b>_CLIENTS</b> in the <b>Name</b> box and click `OK`
3. Right-click on <b>mydomain.com</b> and click `Refresh` in order to organize the folders
4. Click on the <b>Computers</b> folder in order to see <b>Client-1</b> on the right side-panel
5. Drag <b>Client-1</b> to the <b>_CLIENTS</b> folder
6. A window will open titled <b>Active Directory Domain Services</b>. Click `Yes`.

<p>
<img src="https://github.com/user-attachments/assets/085555f7-229f-4a7f-97eb-2def9876716b" height="80%" width="80%" alt="Move Client-1"/>
</p></br>

7. Click on the <b>_CLIENTS</b> folder to confirm the move

<p>
<img src="https://github.com/user-attachments/assets/5af1cfbc-8adf-4dbb-8dbe-eb212b8222d3" height="80%" width="80%" alt="Confirm Client-1 Move"/>
</p></br>


<b>Set up Remote Desktop for non-administrative users on Client-1</b></br>

1. Log into <b>Client-1</b> as <em>mydomain.com\jane_admin</em>
2. Right-click on the <b>Start</b> icon and select <b>System</b>
3. Click on the <b>Remote desktop</b> link on the right side
4. Click on the link at the bottom that says <b>Select users that can remotely access this PC</b>. Click `Add`.
5. In the <b>Select Users or Groups</b> window, type <em>Domain Users</em> and click on `Check Names`. Click `OK`.
6. Back in the <b>Remote Desktop Users</b> window, click `OK`

<p>
<img src="https://github.com/user-attachments/assets/7f7affc3-e6ca-4e92-be18-b7a3735de6a7" height="80%" width="80%" alt="Non-Admin Setup"/>
</p></br>


<b>Create additional users</b></br>

1. Log into <b>DC-1</b> as <em>mydomain.com\jane_admin</em>
2. Open <b>Windows PowerShell ISE</b> as an administrator
3. Click on the blue arrow next to the word <b>script</b> to open the script pane
4. Right-click on this [script](https://github.com/derickayala25/derickayala25/blob/main/Generate-Names-Create-Users.txt) link and select `Open link in new tab`
5. Select all the script and copy. Go back to the script pane in <b>DC-1</b> and paste the script in it. Notice that the default password for users will be <em>Password1</em>
6. Click on the green `Run Script` arrow at the top of the window and observe the accounts being created

<p>
<img src="https://github.com/user-attachments/assets/6d687c44-6202-46fe-973f-880fba59d3ad" height="80%" width="80%" alt="Creating Additional Users"/>
</p></br>

7. When finished, open <b>Active Directory Users and Computers</b> and observe the accounts in the appropriate <b>Organizational Unit</b> `(_EMPLOYEES)`

<p>
<img src="https://github.com/user-attachments/assets/910f8c13-74a5-419f-ab50-d386f81b64ae" height="80%" width="80%" alt="New Users"/>
</p></br>


<b>Log into Client-1 with one of the newly created accounts</b></br>
Now all of the new employees created are part of the <b>mydomain.com</b> domain. Let's log in as one of them.
1. Select one of the usernames created (in this case I'll select <b>bepa.toli</b>). As noted before, the default password will be <em>Password1</em>.
2. Log in to the <b>Client-1</b> VM as <em>mydomain.com\bepa.toli</em>

<p>
<img src="https://github.com/user-attachments/assets/9a637cd7-5e68-4d74-a8b6-d48587d16a06" height="80%" width="80%" alt="First Login-1"/>
</p>

<p>
<img src="https://github.com/user-attachments/assets/08261c74-7753-43a4-b450-0cbfbc7da332" height="80%" width="80%" alt="First Login-2"/>
</p>

3. Now that you've confirmed that the setup has been done correctly, you can log off the <b>Client-1</b> VM.</br></br>


<b>Dealing with Account Lockouts</b></br>
If you're not already logged in to <b>DC-1</b> as the admin, log in as <em>mydomain.com\jane_admin</em>. Now, we'll configure account lockout threshold in <b>Group Policy</b>.

1. In <b>DC-1</b>, type <em>gpmc.msc</em> in the search box and select. This will open the <b>Group Policy Management Console</b>.
2. On the left side of the window you will see `Forest: mydomain.com`. Expand it unto you see <b>Default Domain Policy</b>.
3. Right-click on <b>Default Domain Policy</b> and select `Edit`. A new window will open called <b>Group Policy Management Editor</b>.
4. In the <b>Group Policy Management Editor</b> window, expand the following: `Computer Configuration` > `Policies` > `Windows Settings` > `Security Settings` > `Account Policies`
5. Click on <b>Account Lockout Policy</b>. Double-click on <b>Account lockout duration</b>, check the box that says <b>Define this policy setting</b>, and set the minutes to 30




<p>
<img src="https://github.com/user-attachments/assets/9a637cd7-5e68-4d74-a8b6-d48587d16a06" height="80%" width="80%" alt="Non-Admin Setup"/>
</p>




