<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h1 align="center">Azure Active Directory Lab - Deploy and Configure AD</h1>

<p>The goal of this lab is to deploy and configure Active Directory (AD) in a virtualized environment using Azure. You will create a Domain Controller (DC), join a client machine to the domain, and configure user accounts and Remote Desktop access for both administrative and non-administrative users.</p>

<h2>High-Level Deployment and Configuration Steps</h2>
<ol>
  <li>Set up resources in Azure: Create a Domain Controller and a Client VM.</li>
  <li>Ensure network connectivity between the Client and the Domain Controller.</li>
  <li>Install Active Directory Domain Services (AD DS) on the Domain Controller.</li>
  <li>Create administrative and standard user accounts in Active Directory.</li>
  <li>Join the Client machine to the domain.</li>
  <li>Configure Remote Desktop access for non-administrative users on the Client.</li>
  <li>Create additional user accounts using a script and verify access.</li>
</ol>

<h2>Environments and Technologies Used</h2>
<ul>
  <li>Microsoft Azure (Virtual Machines/Compute)</li>
  <li>Remote Desktop</li>
  <li>Windows Server 2022 (Domain Controller)</li>
  <li>Windows 10 (Client)</li>
</ul>

<h2>Operating Systems Used</h2>
<ul>
  <li>Windows Server 2022</li>
  <li>Windows 10 (21H2)</li>
</ul>

<h2>Step 1: Setup Resources in Azure</h2>
<ul>
  <li>Create the Domain Controller VM (Windows Server 2022) named “DC-1”.</li>
  <li>Take note of the Resource Group and Virtual Network (Vnet) created for the Domain Controller.</li>
  <li>Set DC-1's NIC Private IP address to static to ensure consistent network connectivity.</li>
  <li>Create the Client VM (Windows 10) named “Client-1” in the same Resource Group and Vnet as DC-1.</li>
  <li>Ensure both VMs (DC-1 and Client-1) are in the same Vnet. You can verify this with Azure's Network Watcher topology tool.</li>
</ul>

<h2>Step 2: Ensure Connectivity between Client and Domain Controller</h2>
<ul>
  <li>Login to Client-1 using Remote Desktop and run a continuous ping to DC-1’s private IP address (<code>ping -t &lt;ip_address&gt;</code>).</li>
  <li>Login to DC-1 and enable ICMPv4 (ping) in the local Windows Firewall settings.</li>
  <li>Check Client-1 to confirm that the ping now succeeds, ensuring connectivity between the Client and Domain Controller.</li>
</ul>

<h2>Step 3: Install Active Directory</h2>
<ul>
  <li>Login to DC-1 and install the <strong>Active Directory Domain Services</strong> (AD DS) role.</li>
  <li>Promote DC-1 to a Domain Controller and set up a new forest with a domain name (e.g., <code>mydomain.com</code>).</li>
  <li>Restart DC-1 after promotion and log in using the domain account: <code>mydomain.com\labuser</code>.</li>
</ul>

<h2>Step 4: Create Admin and Normal User Accounts in Active Directory</h2>
<ul>
  <li>In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called <strong>_EMPLOYEES</strong> for employee accounts.</li>
  <li>Create another OU named <strong>_ADMINS</strong> for administrative users.</li>
  <li>Create a new user named “Jane Doe” with the username <code>jane_admin</code> and add her to the <strong>Domain Admins</strong> security group.</li>
  <li>Log out and back in to DC-1 as <code>mydomain.com\jane_admin</code> to use this admin account moving forward.</li>
</ul>

<h2>Step 5: Join Client-1 to the Domain</h2>
<ul>
  <li>In the Azure Portal, set Client-1’s DNS settings to DC-1's Private IP address.</li>
  <li>Restart Client-1 from the Azure Portal.</li>
  <li>Login to Client-1 using the local admin account <code>labuser</code>, and join it to the domain <code>mydomain.com</code>.</li>
  <li>Once joined, restart Client-1 and verify that it appears in ADUC under the <strong>Computers</strong> container.</li>
  <li>(Optional) Create an OU called <strong>_CLIENTS</strong> and move Client-1 into this OU for organizational purposes.</li>
</ul>

<h2>Step 6: Configure Remote Desktop for Non-Administrative Users</h2>
<ul>
  <li>Log into Client-1 as <code>mydomain.com\jane_admin</code> and open the system properties.</li>
  <li>Click on "Remote Desktop" and allow <strong>domain users</strong> access to Remote Desktop on Client-1.</li>
  <li>This step enables regular, non-administrative users to log in via Remote Desktop. Typically, this is done using Group Policy for larger networks to manage multiple systems at once.</li>
</ul>

<h2>Step 7: Create Additional Users and Test Logins</h2>
<ul>
  <li>Log into DC-1 as <code>jane_admin</code> and open PowerShell ISE as an administrator.</li>
  <li>Use PowerShell to create multiple user accounts in AD, following a scripted process.</li>
  <li>Once the script completes, check ADUC to ensure the new users are created in the appropriate OU.</li>
  <li>Attempt to log into Client-1 with one of the newly created users to verify the account works correctly.</li>
</ul>