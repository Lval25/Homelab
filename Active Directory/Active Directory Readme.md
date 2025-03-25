Active Directory Home Lab Environment (VirtualBox)

This project is a step-by-step walkthrough of how I built a virtual Active Directory (AD) home lab environment using VirtualBox. The goal of this lab is to simulate a real-world business network for hands-on experience with system administration and cybersecurity practices.
🔧 What I Did

    Set up a Microsoft Server 2019 to run Active Directory services.
    Configured a Domain Controller (DC) to manage users and devices within a custom domain.
    Deployed a PowerShell script to automatically create over 1,000 user accounts in Active Directory.
    Connected a Windows 10 Enterprise client machine to the domain and successfully logged into the newly created user accounts.

🏢 Why This Lab Matters

    Simulates a corporate IT infrastructure with user management and domain control.
    Provides hands-on practice for managing user authentication, group policies, and network permissions.
    Lays the foundation for testing cybersecurity tools, running penetration tests, and identifying system vulnerabilities in a safe environment.

📦 Tools & Resources Used

    [Microsoft Server 2019 ISO] – For the Active Directory setup.
    [Windows 10 Enterprise ISO] – As a client machine for domain interaction.
    [VirtualBox] – For creating and managing virtual machines.
    PowerShell Script – Used to bulk-create user accounts in AD.

🚀 Getting Started
Prerequisites
  - [VirtualBox](https://www.virtualbox.org/wiki/Downloads) – Free virtualization software for managing virtual machines.  
  - [Microsoft Windows Server 2019 ISO (180-day trial)](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019) – Server OS used to set up the Domain Controller and run Active Directory.  
  - [Windows 10 Enterprise ISO (90-day trial)](https://www.microsoft.com/en-us/evalcenter/download-windows-10-enterprise) – Client OS used to join the Active Directory domain.  
    Clone or download the PowerShell Script.

Setup Steps

    Install VirtualBox and create two virtual machines: one for Windows Server 2019 and one for Windows 10 Enterprise.
    Install Active Directory Domain Services (AD DS) on the Windows Server VM.
    Promote the server to a Domain Controller (DC) and configure the domain.
    Run the PowerShell script to generate user accounts in Active Directory.
    Join the Windows 10 VM to the domain and test logging in with newly created user accounts.



____________________________________________________________________________________________________________________________________________


# Active Directory Home Lab Setup
--
![Setup](https://github.com/user-attachments/assets/15990368-9895-47cb-a2aa-e60fda2c96d1)

# Diagram


## Languages Used
- **PowerShell**

## Environments Used
- **VirtualBox**
- **Windows Server 2019**
- **Windows 10**

## Tools and Services Used
- **AD DS**: Active Directory Domain Services  
- **NAT**: Network Address Translation  
- **DHCP**: Dynamic Host Configuration Protocol  

---

## Getting Started

### Step 1: Download and Install VirtualBox  
Download and install [Oracle VirtualBox](https://www.virtualbox.org/) from the official website.

### Step 2: Download ISO Files  
Ensure you have the necessary ISO files for your environment setup:  
- [Windows 10 ISO](https://www.microsoft.com/software-download/windows10)  
- [Windows Server 2019 ISO](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019)  

---

Follow the detailed steps in this guide to configure your Active Directory environment and get hands-on experience with server administration and networking.

## Step 3: Create a Virtual Machine in VirtualBox

1. Open **VirtualBox** and click on **New** to create a new virtual machine.  
2. Name the virtual machine **Domain Controller**.  
3. Select the operating system type and version:  
   - **Type:** Microsoft Windows  
   - **Version:** Windows Server 2019 (64-bit)
   -  <img width="971" alt="Screenshot 2025-01-19 at 11 11 05 AM" src="https://github.com/user-attachments/assets/77671939-b93a-4f7b-b58f-dbbc5acd0e84" />
4. Allocate resources:  
   - **Memory (RAM):** Recommended 4GB or more.  
   - **Virtual Hard Disk:** At least 50GB dynamically allocated.
   - <img width="963" alt="Screenshot 2025-01-19 at 11 04 22 AM" src="https://github.com/user-attachments/assets/4c4bb9bf-dde3-45c2-9397-9e05bc80914d" />
   -  <img width="982" alt="Screenshot 2025-01-19 at 11 18 40 AM" src="https://github.com/user-attachments/assets/7b927be4-c528-4492-b965-b44b02df609a" />

5. Select the **Windows Server 2019 ISO** as the boot media:  
   - Go to **Settings > Storage**.  
   - Under **Controller: IDE**, click the empty disk icon.  
   - Choose the **Windows Server 2019 ISO** file you downloaded.

6. Click **Start** to begin the installation process.

## Step 4: Configure Network Adapters for Internet and Private Network

To set up dual network connectivity, configure two network adapters in VirtualBox:

### Add the First Network Adapter (Internet Access)
1. Open **VirtualBox** and select your virtual machine.
2. Click on **Settings** and go to the **Network** tab.
3. Under **Adapter 1**, configure the following:
   - **Enable Network Adapter:** Checked
   - **Attached to:** NAT (Network Address Translation)
   - **Adapter Type:** Default (e.g., Intel PRO/1000 MT Desktop)
   - <img width="970" alt="Screenshot 2025-01-19 at 11 07 34 AM" src="https://github.com/user-attachments/assets/ac24ef74-79e0-41bc-8619-a5da97be699b" /> 

4. Save the changes.

### Add the Second Network Adapter (Private Network)
1. In the same **Network** tab, switch to **Adapter 2**.
2. Configure the following:
   - **Enable Network Adapter:** Checked
   - **Attached to:** Internal Network
   - **Name:** PrivateNetwork (or another distinguishable name)
   - **Adapter Type:** Default
3. Save the changes.
<img width="985" alt="Screenshot 2025-01-19 at 11 07 42 AM" src="https://github.com/user-attachments/assets/2ce4bcf6-9fb3-4122-92d6-baf0f1435515" />

### Verify the Configuration in the Virtual Machine
1. Start the virtual machine and log into **Windows Server 2019**.
2. Open **Network and Sharing Center**:
   - Confirm the presence of two adapters:
     - **Ethernet 1** (NAT for internet access)
     - **Ethernet 2** (Internal Network for the private domain)
   - Rename the networks to clearly identify their roles:
     - Rename **Ethernet 1** to `Internet Network`.
     - Rename **Ethernet 2** to `Private Network`.

### Test the Connections
1. Open **Command Prompt** in the virtual machine and run:
   ```powershell
   ipconfig /all

## Step 5: Install Windows Server 2019 and Configure Networking

### Install Windows Server 2019
1. Start the virtual machine with the **Windows Server 2019 ISO** as the boot media.
2. Follow the installation wizard:
   - Select **Windows Server 2019 Standard with Desktop Experience**.
   - Set your region, language, and keyboard preferences.
   - Create an administrator account with a strong password.
3. Complete the installation and log in as the administrator.

---
<img width="1170" alt="Screenshot 2025-01-15 at 4 57 52 PM" src="https://github.com/user-attachments/assets/11c07dc7-f016-4053-9002-760b2a5d216b" />
<img width="1042" alt="Screenshot 2025-01-15 at 4 58 41 PM" src="https://github.com/user-attachments/assets/ccee5097-2b6c-4349-a160-5f17c6477f37" />



### Assign IP Addressing for the Internal Network
1. **Open the Network Settings**:
   - Go to **Control Panel > Network and Sharing Center**.
   - Click **Change adapter settings** on the left menu.
   - Right-click the adapter for the internal network (e.g., `Ethernet 2`) and select **Properties**.

2. **Set a Static IP Address**:
   - Select **Internet Protocol Version 4 (TCP/IPv4)** and click **Properties**.
   - Enter the following settings:
     - **IP Address:** `192.168.1.10`
     - **Subnet Mask:** `255.255.255.0`
     - **Default Gateway:** Leave blank (this is for an isolated internal network).
     - **Preferred DNS Server:** `192.168.1.10` (server's own IP).

3. **Rename the Network Adapters**:
   - In **Network and Sharing Center**, rename the adapters for clarity:
     - **Ethernet 1**: `Internet Network` (for NAT adapter).
     - **Ethernet 2**: `Private Network` (for internal adapter).

---

### Verify the Configuration
1. Open **Command Prompt** and check the IP configuration:
   ```powershell
   ipconfig /all


## Step 6: Name the Server and Install Active Directory

---
<img width="1271" alt="Screenshot 2025-01-15 at 5 07 40 PM" src="https://github.com/user-attachments/assets/db086bbe-0635-45fe-804a-dbb5d34cfd60" />


<img width="1244" alt="Screenshot 2025-01-19 at 12 08 31 PM" src="https://github.com/user-attachments/assets/f77a4dca-5637-4d62-ac59-6220f9c1b7cc" />




### Rename the Server
1. **Open the Server Manager**:
   - Click **Start** and open **Server Manager**.
2. **Access System Properties**:
   - In Server Manager, click **Local Server** in the left-hand menu.
   - Locate **Computer Name** and click on the current name to open the **System Properties** window.
3. **Rename the Server**:
   - Click **Change**, and under **Computer Name**, enter a descriptive name, such as `DC01` (Domain Controller 01).
   - Click **OK** and restart the server when prompted.

---

### Install Active Directory Domain Services (AD DS)
1. **Open Server Manager**:
   - Click **Add Roles and Features** in the Dashboard.
2. **Start the Role Installation Wizard**:
   - Select **Role-based or Feature-based Installation** and click **Next**.
   - Choose the local server (`DC01`) and click **Next**.
3. **Add the AD DS Role**:
   - In the **Server Roles** section, select **Active Directory Domain Services (AD DS)**.
   - When prompted, click **Add Features** to include required services.
   - Click **Next** until you reach the confirmation page, then click **Install**.
4. **Complete the Installation**:
   - Wait for the installation to complete, but do not restart the server yet.

---

### Promote the Server to a Domain Controller
1. **Open Post-Deployment Configuration**:
   - In **Server Manager**, click the **Flag Notification** icon and select **Promote this server to a domain controller**.
2. **Create a New Domain**:
   - Select **Add a new forest** and enter your desired domain name (e.g., `example.local`).
3. **Set Domain Controller Options**:
   - Choose the **Forest Functional Level** and **Domain Functional Level** (default is Windows Server 2016 or higher).
   - Check **DNS Server** (required for AD DS) and click **Next**.
4. **Verify the Prerequisites**:
   - Review the configuration options, fix any warnings if needed, and click **Install**.
5. **Restart the Server**:
   - Once the installation is complete, the server will automatically restart.

---

### Verify the Active Directory Installation
1. After the server restarts, log in using the domain credentials (e.g., `Administrator@example.local`).
2. Open **Active Directory Users and Computers** to confirm the domain is operational.

---

This step finalizes your server as a domain controller, allowing you to manage your new Active Directory environment.


## Step 7: Configure Routing for Private Network Internet Access

To allow clients on the private network to access the internet through the domain controller, configure the server to act as a router.

![image](https://github.com/user-attachments/assets/79ab0896-f417-4a55-8905-54abdd55ca6a)


<img width="1170" alt="Screenshot 2025-01-15 at 4 57 52 PM" src="https://github.com/user-attachments/assets/793f3b66-2e09-4473-a268-e735274f5320" />

---

### Enable Routing and Remote Access (RRAS)
1. **Install the RRAS Role**:
   - Open **Server Manager** and click **Add Roles and Features**.
   - Select **Role-based or Feature-based Installation** and click **Next**.
   - Choose the local server and click **Next**.
   - Under **Server Roles**, expand **Remote Access** and select:
     - **Routing**
     - Click **Add Features** when prompted.
   - Click **Next** until you reach the confirmation page, then click **Install**.

2. **Configure RRAS**:
   - After installation, open **Routing and Remote Access** from the **Tools** menu in **Server Manager**.
   - Right-click your server name and select **Configure and Enable Routing and Remote Access**.
   - In the wizard, select **Custom Configuration** and check **NAT** (Network Address Translation).
   - Complete the wizard and start the RRAS service.

---

### Set Up Network Address Translation (NAT)
1. **Configure the NAT Interface**:
   - In the **Routing and Remote Access** console, expand your server name and go to **IPv4 > NAT**.
   - Right-click **NAT** and select **New Interface**.
   - Select the network adapter connected to the internet (`Internet Network` adapter).
   - Choose **Public Interface connected to the Internet** and check **Enable NAT on this interface**.
   - Click **OK**.

2. **Configure the Private Network Interface**:
   - Add the private network adapter (`Private Network` adapter) as a NAT interface:
     - Right-click **NAT** and select **New Interface**.
     - Select the private network adapter.
     - Choose **Private Interface connected to a private network**.
     - Click **OK**.

---

### Test Internet Connectivity for Clients
1. **Set Up a Client Machine**:
   - On a client connected to the private network, configure the IP settings:
     - **IP Address:** A unique static IP in the private network (e.g., `192.168.1.20`).
     - **Subnet Mask:** `255.255.255.0`.
     - **Default Gateway:** The domain controller's private network IP (e.g., `192.168.1.10`).
     - **Preferred DNS Server:** The domain controller's private network IP (e.g., `192.168.1.10`).

2. **Verify Internet Access**:
   - On the client, open **Command Prompt** and test connectivity:
     ```powershell
     ping 8.8.8.8
     ```
   - Open a web browser and confirm access to external websites.

---

This configuration enables clients on the private network to access the internet through the domain controller, ensuring seamless connectivity.



## Step 8: Set Up DHCP on the Domain Controller

To allow the domain controller to dynamically assign IP addresses to clients on the private network, configure it as a DHCP server.

---

### Install the DHCP Server Role
1. **Open Server Manager**:
   - Click **Add Roles and Features** from the Dashboard.
2. **Start the Installation Wizard**:
   - Select **Role-based or Feature-based Installation** and click **Next**.
   - Choose your server (e.g., `DC01`) and click **Next**.
3. **Add the DHCP Server Role**:
   - In the **Server Roles** section, select **DHCP Server**.
   - When prompted, click **Add Features** to include required components.
   - Click **Next** until you reach the confirmation page, then click **Install**.
4. **Complete the Installation**:
   - Once the installation is finished, click **Complete DHCP Configuration** in the notification area.
   - Follow the wizard to authorize the DHCP server using the domain administrator credentials.

---

### Configure a DHCP Scope
1. **Open DHCP Manager**:
   - In **Server Manager**, go to **Tools > DHCP**.
   - Expand your server name and select **IPv4**.
2. **Create a New Scope**:
   - Right-click **IPv4** and select **New Scope**.
   - Follow the wizard to configure the scope:
     - **Scope Name:** PrivateNetworkScope
     - **Starting IP Address:** `192.168.1.20`
     - **Ending IP Address:** `192.168.1.100`
     - **Subnet Mask:** `255.255.255.0`
     - **Default Gateway:** `192.168.1.10` (IP address of the domain controller)
     - **Preferred DNS Server:** `192.168.1.10`
3. **Activate the Scope**:
   - After completing the wizard, right-click the new scope and select **Activate**.

---

### Verify DHCP Configuration
1. **Test on a Client Machine**:
   - Configure the client machine to obtain an IP address automatically:
     - Go to **Network and Sharing Center > Change adapter settings**.
     - Right-click the network adapter, select **Properties**, and enable **Obtain an IP address automatically** and **Obtain DNS server address automatically**.
   - Run the following command in **Command Prompt**:
     ```powershell
     ipconfig /renew
     ```
   - Verify the client receives an IP address within the configured scope.

2. **Ping the Domain Controller**:
   - Test connectivity by pinging the domain controller:
     ```powershell
     ping 192.168.1.10
     ```

---

This step allows your domain controller to dynamically assign IP addresses to clients on the private network.


## Step 9: Run a PowerShell Script to Create 1000 Users in Active Directory

This step uses a PowerShell script to bulk-create 1000 users in your Active Directory environment.

---

### Prepare the PowerShell Script
1. **Create the Script File**:
   - Open a text editor (e.g., Notepad) and paste the following script:
     ```powershell
     # Import the Active Directory module
     Import-Module ActiveDirectory

     # Loop to create 1000 users
     for ($i = 1; $i -le 1000; $i++) {
         $Username = "User$i"
         $Password = ConvertTo-SecureString "Password123!" -AsPlainText -Force
         $UserPrincipalName = "$Username@example.local"

         # Create the user account
         New-ADUser -Name $Username `
                    -GivenName "FirstName$i" `
                    -Surname "LastName$i" `
                    -SamAccountName $Username `
                    -UserPrincipalName $UserPrincipalName `
                    -AccountPassword $Password `
                    -Path "CN=Users,DC=example,DC=local" `
                    -Enabled $true
     }

     Write-Host "1000 users successfully created."
     ```
   - Save the file with a `.ps1` extension, such as `CreateUsers.ps1`.

2. **Modify the Script for Your Domain**:
   - Replace `example.local` with your actual domain name.
   - Adjust the **Path** (`CN=Users,DC=example,DC=local`) to match your Active Directory structure.

---

### Run the Script
1. **Open PowerShell as Administrator**:
   - Log in to your domain controller and open **PowerShell** with administrative privileges.

2. **Run the Script**:
   - Navigate to the location of your script file:
     ```powershell
     cd C:\Path\To\Your\Script
     ```
   - Execute the script:
     ```powershell
     .\CreateUsers.ps1
     ```

3. **Monitor the Output**:
   - The script will output the progress and confirm when all 1000 users are created.

---

### Verify the Users in Active Directory
1. Open **Active Directory Users and Computers**:
   - Navigate to the **Users** container (or the custom Organizational Unit specified in the script).
   - Verify that the new users (e.g., `User1`, `User2`, ... `User1000`) are listed.
2. Test a user account:
   - Log in with one of the new accounts (e.g., `User1`) to ensure it works:
     - Username: `User1@example.local`
     - Password: `Password123!`

---

This step automates user creation, saving time and providing a scalable method to populate your Active Directory environment.

## Step 10: Create a New Virtual Machine Named "Client1"

This step involves creating a new virtual machine for the client system using the Windows 10 ISO file.

<img width="1100" alt="Screenshot 2025-01-15 at 4 18 49 PM" src="https://github.com/user-attachments/assets/ed25ec15-35cc-4100-ae73-6deba9704b67" />

---

### Create the Virtual Machine in VirtualBox
1. **Open VirtualBox**:
   - Launch **Oracle VirtualBox** and click **New** to create a new virtual machine.
2. **Name the Virtual Machine**:
   - Enter the name as **Client1**.
   - Set the following parameters:
     - **Type:** Microsoft Windows
     - **Version:** Windows 10 (64-bit)
   - Click **Next**.
3. **Allocate System Resources**:
   - **Memory (RAM):** Assign at least **2 GB** (recommended: 4 GB).
   - **Virtual Hard Disk:** Choose **Create a virtual hard disk now**, then:
     - Select **Dynamically Allocated**.
     - Set the size to **50 GB** or higher.
   - Click **Create**.

---

### Attach the Windows 10 ISO File
1. **Open Settings**:
   - Select the **Client1** virtual machine and click **Settings**.
2. **Attach the ISO**:
   - Go to the **Storage** tab.
   - Under **Controller: IDE**, click the empty disk icon (**Empty**).
   - Click the **Choose a disk file** button and select your **Windows 10 ISO** file.
   - Click **OK** to save the settings.

---

### Start and Install Windows 10
1. **Boot the Virtual Machine**:
   - Select **Client1** and click **Start**.
2. **Begin Installation**:
   - Follow the Windows 10 installation wizard:
     - Choose your language, time, and keyboard preferences.
     - Enter a product key (if available) or select **I don't have a product key** to continue.
     - Select the **Windows 10 Pro** version (if prompted).
     - Accept the license terms and click **Next**.
3. **Partition the Disk**:
   - Select the **Unallocated Space** and click **Next** to install Windows.
4. **Complete the Setup**:
   - Create a local administrator account and set a password.
   - Customize system settings as needed.

---

### Verify the Client Configuration
1. **Check Network Settings**:
   - Ensure the virtual machine's network adapter is set to the private network (e.g., `PrivateNetwork`).
   - Open **Command Prompt** on the client and run:
     ```powershell
     ipconfig
     ```
     - Verify the client receives an IP address from the domain controller.
2. **Join the Domain**:
   - Open **Settings > Accounts > Access work or school**.
   - Click **Connect**, then **Join this device to a local Active Directory domain**.
   - Enter the domain name (e.g., `example.local`) and follow the prompts to join.

---

This step sets up a client virtual machine for testing and connects it to your Active Directory domain.



## Step 11: Connect the Client Machine to the Private Network and Join the Domain

This step connects the client machine (e.g., **Client1**) to the private network and adds it to your Active Directory domain.
<img width="765" alt="Screenshot 2025-01-14 at 11 32 57 AM" src="https://github.com/user-attachments/assets/b7e5ce4a-6299-4483-b7bb-a5992a6cad37" />

<img width="822" alt="Screenshot 2025-01-14 at 11 32 33 AM" src="https://github.com/user-attachments/assets/fce16577-da77-4284-8e54-c73edb0736e0" />


---

### Step 1: Configure the Network Adapter for the Private Network
1. **Open VirtualBox Settings**:
   - Select the **Client1** virtual machine and click **Settings**.
   - Go to the **Network** tab.
2. **Attach the Private Network**:
   - Under **Adapter 1**, configure the following:
     - **Enable Network Adapter:** Checked
     - **Attached to:** Internal Network
     - **Name:** PrivateNetwork (or the name you used for the internal network).
   - Click **OK** to save the settings.

3. **Verify the Network Configuration**:
   - Start the **Client1** virtual machine and log in.
   - Open **Command Prompt** and run:
     ```powershell
     ipconfig
     ```
   - Ensure the client has received an IP address from the domain controller (e.g., `192.168.1.x`) and that the **Default Gateway** and **DNS Server** point to the domain controller's IP (e.g., `192.168.1.10`).

---

### Step 2: Join the Client to the Domain
1. **Open System Properties**:
   - Right-click **This PC** on the desktop or in File Explorer and select **Properties**.
   - Click **Advanced system settings** > **Computer Name** tab.
2. **Change the Computer Name/Domain**:
   - Click **Change**.
   - In the **Member of** section, select **Domain** and enter your domain name (e.g., `example.local`).
   - Click **OK**.
3. **Provide Domain Credentials**:
   - When prompted, enter the credentials of a domain administrator account (e.g., `Administrator@example.local`).
   - Click **OK** to complete the process.
4. **Restart the Client**:
   - Restart the **Client1** virtual machine to apply the changes.

---

### Step 3: Verify the Domain Connection
1. **Log In Using a Domain Account**:
   - After restarting, log in using a domain account:
     - Username: `example\YourDomainUser`
     - Password: The domain account password.
2. **Check the Domain Membership**:
   - Open **Command Prompt** and run:
     ```powershell
     systeminfo | findstr /B /C:"Domain"
     ```
   - Verify the output shows the correct domain name (e.g., `example.local`).

---

### Step 4: Test Connectivity
1. **Ping the Domain Controller**:
   - Open **Command Prompt** and run:
     ```powershell
     ping 192.168.1.10
     ```
   - Ensure the domain controller responds successfully.
2. **Access Network Resources**:
   - Open **File Explorer**, type the domain controller's hostname or IP in the address bar (e.g., `\\192.168.1.10`), and verify access to shared resources.

---

This step successfully integrates the client machine into the private network and the Active Directory domain, enabling centralized management and resource sharing.



## Step 12: Log Into the Client Machine with a Domain Account

<img width="801" alt="Screenshot 2025-01-14 at 11 34 40 AM" src="https://github.com/user-attachments/assets/19ea0b15-aa38-4cd8-9c4e-bf339951a1ca" />

After joining the client machine to the domain, log in using a domain account to verify the connection and access domain resources.

---

### Step 1: Switch to a Domain Account at Login
1. **Restart the Client Machine**:
   - If not already restarted, reboot the client machine to apply the domain settings.
2. **Access the Login Screen**:
   - On the Windows login screen, click **Other User**.
3. **Enter the Domain Credentials**:
   - Use the following format for the username:
     - `DomainName\Username` (e.g., `example\User1`)
   - Enter the password for the domain account (e.g., `Password123!`).
   - Click **Sign In**.

---

### Step 2: Verify Domain Account Login
1. **Confirm Login**:
   - Once logged in, the desktop should load with the new domain account settings.
   - Open **File Explorer** and navigate to **This PC** to ensure the user's profile directory (e.g., `C:\Users\User1`) has been created.
2. **Check Domain Membership**:
   - Open **Command Prompt** and run:
     ```powershell
     whoami /upn
     ```
   - The output should display the user’s UPN (User Principal Name), such as `User1@example.local`.

---


### Step 3: Test Access to Domain Resources
1. **Access Shared Folders**:
   - Open **File Explorer** and type the domain controller’s hostname or IP address (e.g., `\\192.168.1.10`) in the address bar.
   - Verify that shared resources are accessible.
2. **Check DNS Resolution**:
   - Open **Command Prompt** and run:
     ```powershell
     nslookup example.local
     ```
   - Ensure the domain controller's IP address is returned.

---

This step confirms that the domain account is functional on the client machine and has access to domain resources.

