# Windows Domain Controller
<h2>Description</h2>
In this lab, I deploy a Windows Server instance on AWS EC2 and configure it as a functioning Active Directory Domain Controller (AD DC). This exercise enables me to understand how identity services work in enterprise environments, practice secure cloud deployment, and learn foundational AD setup tasks—such as domain creation, DNS integration, and user/group management.

Key learning objectives:
- Spin up a Windows Server VM in the cloud (AWS EC2).
- Install and configure Active Directory Domain Services (AD DS).
- Promote the VM to a Domain Controller and establish a domain.
- Understand typical directory services security principles, like domain accounts and access control
- Create OUs, add users, and add computers

<h2>Enviroments and Utilities</h2>

<b>Enviroment:</b>
  - <b>Cloud Platform:</b> Amazon Web Services (AWS).
  - <b>Compute:</b> EC2 instance running Windows Server (e.g., Windows Server 2025).
  - <b>Networking:</b> EC2 Virtual Private Cloud (VPC).

<b>Utilities and Tools:</b>
  - AWS Console for provisioning and managing AWS resources
  - PowerShell for automation
  - Remote Desktop Protocol (RDP) for connecting to the server
  - Active Directory Users and Computers console for managing directory objects

<h2>Project Setup</h2>

1. <b>Sign up for an account at aws.amazon.com</b>
  - Sign up for a free account (will require a card).

2. <b>Launch an EC2 Instance</b>
  - Navigate to services the Amazon Elastic Compute Cloud (EC2).
  - Hit the Orange "Launch Instance" Button on the right.

<img src="" height="80%" width="80%" alt="AzureSiemLab"/>

3. <b>Set up a Windows Server</b>
  - Name the server (Example: Demo Lab AD Server).
  - Select the base image of Windows Server 2025 for Amazon Machine Image (AMI).
  - Select t3.small for instance type.
  - Leave Network Settings as is or select whatever IP that will be used to remote in.
  - Create Key Pair and save it.
  - Leave everything else as default and hit the orange "Launch Instance" Button.
    
<img src="" height="80%" width="80%" alt="AzureSiemLab"/>

<img src="" height="80%" width="80%" alt="AzureSiemLab"/>

<img src="" height="80%" width="80%" alt="AzureSiemLab"/>

<img src="" height="80%" width="80%" alt="AzureSiemLab"/>

4. <b>Remote into EC2 Instance of Windows Server 2025</b>
  - Navigate back to Amazon Elastic Compute Cloud (EC2) dashboard.
  - Select the correct instance and hit connect.
  - Got to RDP and hit "Get Password" and upload your saved key then hit "decrypt password"
  - Download RDP File or record IP to remote in
  - Use the windows app on MacOS to remote into EC2 Instance

<img src="" height="80%" width="80%" alt="AzureSiemLab"/>

<img src="" height="80%" width="80%" alt="AzureSiemLab"/>

<img src="" height="80%" width="80%" alt="AzureSiemLab"/>

<img src="" height="80%" width="80%" alt="AzureSiemLab"/>

5. <b>Install and configure Active Directory Domain Services (AD DS), promote the VM to a Domain Controller and establish a domain.</b>
  - Open Powershell and run two commands one at a time.
  - Install-WindowsFeature -Name AD-Domain-Services-IncludeManagementTools
  - InstallADDSForest -DomainName "lab.local" -CreateDnsDelegation:$false -DatabasePath "C:\Windows\NTDS" -DomainMode "WinThreshold" -ForestMode "WinThreshold" -InstallDns:$true -LogPath "C:\Windows\NTDS" -NoRebootOnCompletion:$false -SysvolPath "C:\Windows\SYSVOL" -Force
  - After rebooting using the GUI search "Active Directory Administrative Center"

<img src="" height="80%" width="80%" alt="AzureSiemLab"/>

<img src="" height="80%" width="80%" alt="AzureSiemLab"/>

<img src="" height="80%" width="80%" alt="AzureSiemLab"/>

<img src="" height="80%" width="80%" alt="AzureSiemLab"/>

6. <b>Create New Users & Groups (Powershell or GUI)</b>
  - Using the GUI Search Active Directory Users & Groups. Add users, create security groups and computers here.
  - Example Powershell Scripts:
New-ADOrganizationalUnit -Name "Lab Users" -Path "DC=lab,DC=local"

New-ADUser -Name "John Doe" -GivenName "John" -Surname "Doe" '
  -SamAccountName "john" -UserPrincipalName "john@lab.local" '
  -Path "OU=Lab Users,DC=lab,DC=local" -AccountPassword (ConvertTo-SecureString "UserPass!234" -AsPlainText -Force) '
  -Enabled $true -ChangePasswordAtLogon $false

New-ADGroup -Name "Helpdesk" -GroupScope Global -GroupCategory Security -Path "DC=lab,DC=local"
Add-ADGroupMember -Identity "Helpdesk" -Members "john"

<img src="" height="80%" width="80%" alt="AzureSiemLab"/>

<img src="" height="80%" width="80%" alt="AzureSiemLab"/>

<img src="" height="80%" width="80%" alt="AzureSiemLab"/>

<img src="" height="80%" width="80%" alt="AzureSiemLab"/>

<img src="" height="80%" width="80%" alt="AzureSiemLab"/>

<img src="" height="80%" width="80%" alt="AzureSiemLab"/>
