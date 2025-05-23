# **Process for Configuring and Managing Domain Controllers, and Server Roles in a Windows Server 2022 Environment**

---

## **Process Name and Purpose**

**Process Name**:  
**Configuring and Managing Domain Controllers, and Server Roles in a Windows Server 2022 Environment**

**Purpose**:  
The purpose of this document is to provide a comprehensive, step-by-step guide for the configuration and management of **Domain Controllers**, **Group Policies**, and key **server roles** within a **Windows Server 2022** environment. This process ensures the secure setup of Active Directory, DNS, and other server roles essential for an efficient and stable IT infrastructure. By following this guide, system administrators can establish a domain environment that meets both functional and security requirements, while ensuring compliance with organizational standards and best practices.

The goal is to ensure that:
- The domain is set up with the necessary services (DNS, DHCP, Active Directory, etc.)
- Group Policies are applied to enforce security settings across the domain.
- Proper role configurations are in place, ensuring that all servers and workstations can interact securely within the domain environment.

This document is intended for IT administrators and system engineers tasked with configuring and managing domain services, users, and group policies in a production environment.

---

## **Scope and Boundaries**

**Scope**:  
This document covers the complete process of configuring and managing a **Windows Server 2022** domain environment. The steps outlined will include:

- Setting up IP addressing and network configuration for servers and computers.
- Configuring and installing the necessary server roles on the Domain Controller (DC), including **Active Directory Domain Services (AD DS)**, **DNS**, **DHCP**, and **Windows Backup Server**.
- Creating and managing users and organizational units (OUs) in Active Directory using both **Graphical User Interface (GUI)** and **PowerShell**.
- Applying **Group Policy Objects (GPOs)** to ensure security policies are implemented across the domain.
- Configuring shared folders and access permissions on the **File Server**.
- Ensuring all devices are successfully joined to the domain and are functioning according to organizational and security standards.

**Boundaries**:  
This document does **not** cover:

- Installation of **Windows Server 2022** itself. It is assumed the operating system is already installed on the servers prior to following this guide.
- Advanced troubleshooting or disaster recovery scenarios for domain controllers or related services.
- Configuration or management of non-Windows-based servers or systems outside of the domain environment.
- Implementation of additional third-party software or services not related to the core Windows Server roles mentioned.
  
**Exclusions**:
- This document excludes the setup of **cloud-based services** such as **Azure AD** or hybrid environments, focusing exclusively on a traditional on-premise Active Directory domain.
- It does not cover configurations specific to external DNS or public-facing services like **web hosting** or **email servers**.

**Expected Outcomes**:  
Upon completion of this process:
- The domain environment will be properly configured, secure, and scalable.
- All relevant servers and workstations will be integrated into the domain.
- Proper access control and user management will be established.
- Group policies ensuring security best practices will be applied.

This document is intended to guide IT administrators through all aspects of a **Windows Server 2022** domain setup, from initial configuration through to user management and security enforcement.

---

## **Roles and Responsibilities**

This section outlines the roles and responsibilities of the personnel involved in the process of configuring and managing the **Windows Server 2022** environment. Clear definitions of each role ensure accountability and provide guidance for all involved stakeholders.

**1. IT Administrator**  
**Primary Responsibility**:  
- Configure and install **Active Directory Domain Services (AD DS)** on the Domain Controller.
- Set up **DNS**, **DHCP**, and **File Server roles** on the appropriate servers.
- Create and configure **Group Policy Objects (GPOs)** to enforce security settings across the domain.
- Manage **user accounts**, **organizational units (OUs)**, and group memberships using both **GUI** and **PowerShell**.
- Ensure all **servers and workstations** are joined to the domain and configured according to organizational policies.
- Set up **network configurations** including static IP assignments and DHCP reservations.
- Conduct **backup configurations** for the domain controller and ensure proper **Windows Backup** scheduling.
  
**2. Network Administrator**  
**Primary Responsibility**:  
- Design and implement the **IP addressing scheme** for the domain, ensuring proper static IP assignments for servers and dynamic IP assignment for workstations via DHCP.
- Configure network-related services such as **DNS** to ensure proper name resolution across the domain.
- Provide support in the configuration of the **domain controller**'s network settings and ensure connectivity across the entire network.
  
**3. Security Administrator**  
**Primary Responsibility**:  
- Ensure that the security settings within the domain are applied properly via **Group Policy Objects (GPOs)**.
- Implement password policies, account lockout policies, and restrict access to sensitive features such as **Control Panel** and **Command Prompt**.
- Monitor and assess domain activity for compliance with security best practices, including the enforcement of access restrictions to domain resources.
  
**4. Helpdesk Support**  
**Primary Responsibility**:  
- Provide user support for day-to-day operations of domain resources.
- Assist in troubleshooting connectivity issues with the domain and resolve any issues related to user access or group policies.
- Help onboard new users to the domain, ensuring the appropriate group memberships and access permissions are applied.

**5. Server Administrator**  
**Primary Responsibility**:  
- Install, configure, and maintain all server roles on the **Domain Controllers** and other servers in the domain.
- Ensure that **File Server** roles are properly configured and shared folders are accessible to authorized users.
- Manage **security groups** for each department and ensure access control to shared resources is configured according to the organization's policies.

**6. End Users**  
**Primary Responsibility**:  
- Ensure they follow the organization's IT policies when accessing domain resources.
- Report any issues with domain connectivity, password issues, or access restrictions to Helpdesk Support or IT Administrators.
- Adhere to security guidelines, including creating strong passwords and securing their workstations.

**7. Backup Administrator**  
**Primary Responsibility**:  
- Configure and maintain regular backup schedules for all critical domain data, including **system state** backups of domain controllers.
- Ensure backup jobs run on schedule and that backups are tested regularly for data integrity and restoration.

By defining and assigning clear roles and responsibilities, each participant in the process can understand their duties and the collaborative effort needed to establish and maintain the domain environment successfully.

---

## **Inputs and Outputs**

This section outlines the required **inputs** for the process, as well as the expected **outputs**. By identifying the inputs and outputs, this section ensures that all necessary resources are available and that the desired results are achieved upon completing the process.

**Inputs**:
- **Windows Server 2022 ISO**: Required to install and configure the domain controller and other server roles.
- **Hardware Resources**: Servers and workstations with sufficient resources for Windows Server 2022 and required roles.
  - Server hardware (e.g., CPU, RAM, storage) must meet the minimum system requirements for Windows Server 2022.
  - Network infrastructure capable of supporting static IP assignments, DHCP scope, and DNS records.
- **IP Addressing Scheme**: A predefined plan for static IP assignments (for servers) and DHCP scope (for workstations).
- **Active Directory Domain Name**: The domain name as specified earlier (e.g., "yourinitials.nsamsitt.ca").
- **Group Policy Template**: A template or set of predefined policies that need to be applied across the domain.
- **User Information**: A list of users and their details (e.g., usernames, first names, last names) for Active Directory creation.
- **Shared Folder Structure**: A plan or design for organizing and sharing folders for different departments.
- **Backup Schedule**: Details on backup frequency (e.g., daily incremental, weekly full) and location (e.g., network share for backups).
- **Security Group Structure**: A list of security groups based on departments or access levels for file share permissions.

**Outputs**:
- **Configured Domain Environment**:  
  - Domain Controller (DC) set up and configured with **Active Directory Domain Services (AD DS)**.
  - DNS server configured and integrated with the domain.
  - DHCP scope set up with static IP reservations for servers and dynamic IP allocation for workstations.
- **Successfully Joined Devices**:  
  - All servers and workstations have been successfully joined to the domain.
  - Devices properly receive IP addresses according to the scope defined in the DHCP configuration.
- **Group Policy Objects (GPOs)**:  
  - **Password Policy** applied across all domain users.
  - **Account Lockout & Reset Policy** applied.
  - Restrictions on **Command Prompt** and **PowerShell** for regular users.
  - Restrictions on **Control Panel** access.
  - Disabled **Shut Down/Power Off** options for standard users.
  - Disabling the view of **recently logged-on users**.
- **User Accounts and OUs**:  
  - User accounts created in Active Directory with appropriate **organizational units (OUs)**.
  - Users grouped into appropriate **security groups** for access control.
- **Configured File Shares**:  
  - Shared folders structured according to department requirements, with appropriate **permissions** configured for each department’s security group.
  - **Group policies** deployed to automatically map shared folders to domain users.
- **Scheduled Backup**:  
  - Backup job set up for a one-time backup to the designated **network drive**.
  - The backup is successfully executed, and data is backed up per the defined schedule.
- **Secure Access Control**:  
  - Only authorized users have access to specific shared resources based on their department’s security group.
  - Permissions and ownership settings ensure that users cannot delete files or change folder configurations.

**Expected Final Output**:  
A fully configured and secure domain environment in **Windows Server 2022** where all required roles are installed, services are running correctly, devices are joined to the domain, user access is controlled via group policies, and backup configurations are set to ensure data protection.

---

## **Step-by-Step Procedure for Deploying Windows Server 2022**

## **Step 1: Verify Hardware Requirements**  
Before installing Windows Server 2022, ensure that the target system meets the minimum and recommended hardware specifications.

### **1.1. Minimum Hardware Requirements**
| Component | Minimum Requirement | Recommended Requirement |
|----------|---------------------|-------------------------|
| **Processor** | 1.4 GHz 64-bit processor | 2 GHz or faster, multi-core |
| **RAM** | 2 GB (Server Core), 4 GB (Desktop Experience) | 8 GB or more |
| **Storage** | 32 GB minimum | 80 GB or more |
| **Network** | 1x Ethernet Adapter (1 Gbps) | 1 Gbps or higher |
| **Firmware** | UEFI-based firmware with Secure Boot | UEFI with TPM 2.0 enabled |

### **1.2. Compatibility Check**
1. **Boot into BIOS/UEFI:**  
   - Restart the machine.  
   - Press the appropriate key (usually **F2**, **F10**, or **DEL**) to enter BIOS/UEFI.  

2. **Verify UEFI and Secure Boot:**  
   - Confirm that **UEFI** is enabled.  
   - Enable **Secure Boot** if supported.  

3. **Check Processor and Virtualization:**  
   - Confirm processor supports **64-bit architecture**.  
   - If using a VM, enable **Virtualization Technology (VT-x/AMD-V)** in BIOS.  

4. **Verify TPM (Trusted Platform Module):**  
   - Navigate to **Security Settings** in BIOS.  
   - Confirm **TPM 2.0** is enabled (if available).  

### **1.3. Disk Configuration**
1. Confirm that the storage meets size requirements.  
2. If using RAID, configure the RAID controller before installation:  
   - Access RAID setup utility (usually **CTRL + R** or **CTRL + I**).  
   - Create RAID volume (**RAID 1** for redundancy or **RAID 5** for performance).  
3. If using NVMe, confirm that NVMe drivers are available during installation.  

### **1.4. Network Configuration**
1. Connect to a physical network using a **1 Gbps** or higher Ethernet port.  
2. If installing on a VM:  
   - Create a **NAT network** for external internet access.  
   - Create a **VNET** for internal network communication.  
   - **Example:**  
     - **NAT** → External IP Assignment  
     - **VNET 3** → 192.168.30.0/24  

### **1.5. Power and Peripheral Setup**
1. Ensure the system is connected to a **reliable power source** (UPS recommended).  
2. Connect the following peripherals:  
   - **Monitor**  
   - **Keyboard**  
   - **Mouse**  
   - **Network Cable**  

## **Step 2: Install Windows Server 2022**  
This section provides a detailed guide on how to install Windows Server 2022 from an ISO file onto physical hardware or a virtual machine.

### **2.1. Prepare the Installation Media**  

### **(a) Download the Windows Server 2022 ISO File**
1. Open a browser and go to the [Microsoft Evaluation Center](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022).  
2. Select **"Windows Server 2022"** from the list of available products.  
3. Choose the option to download the **ISO file**.  
4. Save the file to a known location on your computer.  
5. Verify the integrity of the downloaded file by checking the SHA256 hash value (optional).  

### **(b) Create a Bootable USB (for Physical Hardware Installation)**
> If you’re installing on a virtual machine, skip this step and go to **2.2**.  

1. Insert a USB flash drive (minimum **8 GB**).  
2. Download and install **Rufus** from [https://rufus.ie](https://rufus.ie).  
3. Open **Rufus** and configure the following:  
   - **Device** → Select your USB drive.  
   - **Boot Selection** → Select the downloaded ISO file.  
   - **Partition scheme** → Set to **GPT** (for UEFI) or **MBR** (for legacy BIOS).  
   - **File System** → NTFS.  
4. Click **Start** to create the bootable USB.  
5. Once the process is complete, eject the USB drive.  

### **2.2. Boot from Installation Media**  

### **(a) For Physical Hardware:**
1. Insert the bootable USB drive into the server.  
2. Restart the system.  
3. Press the correct key to enter the **boot menu** (depends on the manufacturer):  
   - **F2** – Dell  
   - **F10** – HP  
   - **F12** – Lenovo  
4. Select the USB drive from the boot menu.  
5. Press **Enter**.  

### **(b) For Virtual Machine (VMware, Hyper-V, VirtualBox):**
1. Open your virtualization software (e.g., **VMware Workstation**).  
2. Create a new virtual machine:  
   - **Guest OS:** Windows Server 2022  
   - **Processors:** 2 (2 cores)  
   - **Memory:** 4–8 GB  
   - **Disk:** 80 GB (SCSI/NVMe)  
   - **Network:** NAT and Internal (VNET)  
3. Attach the Windows Server 2022 ISO as a boot device.  
4. Start the VM.  

### **2.3. Start the Installation**  
1. When the **Windows Setup** screen appears, select the following options:  
   - **Language to install:** English (United States)  
   - **Time and currency format:** English (United States)  
   - **Keyboard or input method:** US  
2. Click **Next**.  

3. Click **Install Now**.  

### **2.4. Enter Product Key**  
1. If you have a product key, enter it.  
2. If you don’t have a key, select **"I don’t have a product key"** to proceed with an evaluation installation.  
3. Click **Next**.  

### **2.5. Select the Operating System**  
1. Choose one of the following options:  
   - **Windows Server 2022 Standard (Desktop Experience):** Includes GUI  
   - **Windows Server 2022 Standard (Server Core):** No GUI  
2. Click **Next**.  

### **2.6. Accept License Terms**  
1. Check the box for **"I accept the license terms."**  
2. Click **Next**.  

### **2.7. Select Installation Type**  
1. **Custom: Install Windows Only (Advanced):**  
   - Use this option for a clean installation.  
2. **Upgrade:**  
   - Use this option only if upgrading from a previous version of Windows Server.  
3. Select **Custom** for a fresh install.  

### **2.8. Partition the Disk**  
1. On the **"Where do you want to install Windows?"** screen:  
   - If no partitions exist:  
     - Select **New** → Allocate the recommended size for each partition.  
     - Format the partitions.  
   - If partitions already exist:  
     - Delete any existing partitions if you are performing a clean install.  
2. Recommended partitioning:  
   - **C:** – 80 GB (OS)  
   - **D:** – 70 GB (Data)  
   - **E:** – 4 x 10 GB (NVMe partitions)  
3. Click **Next**.  

### **2.9. Start Installation**  
1. Click **Next** to start the installation.  
2. Windows will begin copying and expanding files (this may take 10–30 minutes).  
3. The system will automatically restart once the installation is complete.  

### **2.10. Initial Setup After Installation**  
1. After the system restarts, you will see the **Set Up Windows** screen.  
2. Create an **Administrator Password**:  
   - Must be at least 8 characters.  
   - Include uppercase, lowercase, number, and special character.  
3. Click **Finish**.  

### **2.11. First Sign-In**  
1. When the login screen appears, press **Ctrl + Alt + Delete**.  
2. Sign in with:  
   - **Username:** `Administrator`  
   - **Password:** YourConfiguredPassword  
3. After login, the Server Manager will open automatically.  

### **2.12. Configure Windows Updates**  
1. Open **Settings** → **Update & Security** → **Windows Update**.  
2. Check for updates.  
3. Install all available updates.  
4. Restart the server if required.  

### **2.13. Install Network Drivers (if required)**  
1. Open **Device Manager** (Right-click Start → Device Manager).  
2. Expand **Network Adapters**.  
3. If drivers are missing, right-click → **Update Driver**.  
4. If using a physical machine, download drivers from the manufacturer’s website.  

### **2.14. Enable Remote Desktop**  
1. Open **Settings** → **System** → **Remote Desktop**.  
2. Toggle **Enable Remote Desktop** to **ON**.  
3. Allow Remote Desktop through Windows Firewall:  
   - Open **Control Panel** → **System and Security** → **Windows Defender Firewall** → **Allow an app through firewall**.  
   - Check **Remote Desktop**.

## **Step 3: Create and Configure Environment** 

### **3.1 IP Address Scheme**  
Before configuring the domain, set up the IP scheme for the servers and workstations to ensure they are part of the same network and can communicate with each other. For exmaple:-

#### **3.1.1 Assign IP Addresses (Examples)**

- **Domain Controller (DC) Server**  
  - **IP Address**: `192.168.30.100`  
  - **Subnet Mask**: `255.255.255.0`  
  - **Gateway**: `192.168.30.1`  
  - **DNS Server**: `192.168.30.100` (DNS will be configured later)  

- **Other Servers (SVR01, SVR02, etc.)**  
  - **IP Address**: `192.168.30.110`
  - **Subnet Mask**: `255.255.255.0`  
  - **Gateway**: `192.168.30.1`  
  - **DNS Server**: `192.168.30.100`

- **Workstations/Computers**  
  - **IP Address Range**: `192.168.30.121 - 192.168.30.199`  
  - **Subnet Mask**: `255.255.255.0`  
  - **Gateway**: `192.168.30.1`  
  - **DNS Server**: `192.168.30.100`

Ensure each device gets a unique IP within the specified ranges.

### **3.2 Set up the Domain Controller (DC)**  

#### **3.2.1 Install Active Directory Domain Services (AD DS)**  
1. Open **Server Manager** on the server designated as **Domain Controller (DC)**.  
2. Click on **Add roles and features**.  
3. Select **Role-based or feature-based installation**, then select the server to install the roles.  
4. In the **Roles** section, select **Active Directory Domain Services (AD DS)**.  
5. Click **Next** and continue with the installation.  

#### **3.2.2 Promote the Server to Domain Controller**  
1. After AD DS is installed, click **Promote this server to a domain controller**.  
2. Select **Add a new forest** under the **Deployment Configuration**.  
3. Enter the **Root domain name**: `domain.com` (replace with your domain name).  
4. Set the **Forest functional level** and **Domain functional level** to **Windows Server 2022**.  
5. Enter a **Directory Services Restore Mode (DSRM)** password for recovery.  
6. Complete the wizard by clicking **Next** and **Install**. The server will restart once the installation is complete.  

### **3.3 Install DNS Server Role**  
1. During the Domain Controller promotion process, the **DNS Server** role will be installed automatically.  
2. Once the server has restarted, confirm the DNS role installation by navigating to **Server Manager** → **Tools** → **DNS**.  
3. Ensure the DNS server is resolving domain names by testing with `nslookup` from a client device.  

### **3.4 Configure DNS Server**  
1. Open **DNS Manager** from **Server Manager** → **Tools** → **DNS**.  
2. Right-click **Forward Lookup Zones** and select **New Zone**.  
3. Follow the wizard to create a **Primary Zone** with the domain name `domain.com`.  
4. Confirm that the DNS server is correctly resolving domain names within the domain by performing a test from a client computer:  
   - Open **Command Prompt** and type `domain.com`.  

### **3.5 Install Windows Server Backup Role**  
1. In **Server Manager**, go to **Add roles and features**.  
2. Select the **Windows Server Backup** feature and click **Next**.  
3. Follow the wizard to install the backup feature.  
4. After installation, open **Windows Server Backup** from **Server Manager** → **Tools**.  
5. Configure the backup schedule as needed:  
   - **Full backup** weekly.  
   - **Incremental backup** daily at a specified time.  
   - **Custom backup** of specific directories as required.  

### **3.6 Apply Network Configuration to All Servers**  
1. On each server, configure the network settings to match the IP scheme:  
   - **IP Address**: Set to the assigned IP address.  
   - **Subnet Mask**: `255.255.255.0`  
   - **Gateway**: `192.168.30.1`  
   - **DNS Server**: `192.168.30.100` (Primary Domain Controller)  

2. Verify connectivity by using **ping** to test communication between servers and other devices in the network. For example:  
   ```bash
   ping 192.168.30.100

### **3.7 Join Servers and Computers to the Domain**

#### **3.7.1 Join Servers to the Domain**
1. On each server (e.g., **SVR01**), right-click **This PC** → **Properties**.
2. Click **Change Settings** under **Computer Name, Domain, and Workgroup Settings**.
3. Click **Change**, select **Domain**, and enter the domain name `domain.com`.
4. Enter domain administrator credentials when prompted.
5. Restart the server for the changes to take effect.
6. Confirm the server is joined to the domain by checking **System Properties**.

#### **3.7.2 Join Computers to the Domain**
1. On each workstation, go to **Control Panel** → **System and Security** → **System**.
2. Click **Change settings** under **Computer Name, Domain, and Workgroup Settings**.
3. Click **Change**, select **Domain**, and enter `domain.com`.
4. Enter domain administrator credentials when prompted.
5. Restart the computer for the changes to take effect.
6. Confirm that the computer is part of the domain by checking **System Properties**.

### **3.8 Verify Domain Configuration**
1. On the **Domain Controller**, open **Active Directory Users and Computers** to verify that all joined devices (servers and workstations) are listed under the **Computers** Organizational Unit (OU).
2. Verify that the domain is functional by using the following command on any device:
   ```powershell
   ping domain.com

## **Step 4: Create and Configure Users and OUs for Environment**

### **4.1 Create Organizational Units (OUs) with GUI**
1. On the **Domain Controller**, open **Active Directory Users and Computers** (ADUC) from **Server Manager** → **Tools** → **Active Directory Users and Computers**.
2. In the **ADUC** window, right-click on the domain name (`domain.com`), then select **New** → **Organizational Unit**.
3. In the **New Object – Organizational Unit** window, enter the **OU Name** (e.g., `Sales` or `IT`).
4. Optionally, check **Protect container from accidental deletion** to prevent accidental removal of the OU.
5. Click **OK** to create the OU.
6. Repeat the process to create additional OUs as needed (e.g., `HR`, `IT`, `Support`).

### **4.2 Create Users with GUI**
1. In **Active Directory Users and Computers** (ADUC), right-click on the desired **OU** (e.g., `IT`) and select **New** → **User**.
2. In the **New Object – User** window, enter the following user details:
   - **First Name**: `Lime`
   - **Last Name**: `User`
   - **User logon name**: `lime.user`
3. Click **Next**.
4. Set the **Password** for the user and confirm the password. You can choose to make the password changeable or set it to not expire.
   - **Password**: `P@ssw0rd123!`
   - **Password never expires** (optional).
5. Click **Next**, then **Finish** to create the user.
6. Verify the new user appears under the selected **OU**.

Repeat the above process to create additional users as necessary.

### **4.3 Create Bulk Users with PowerShell**

To create users in bulk using PowerShell, follow these steps:

1. **Open PowerShell** with administrative privileges on the Domain Controller.
2. Use the following **PowerShell script** to create a batch of users:

```powershell
$users = @(
    @{FirstName="Lime"; LastName="User"; Username="lime.user"},
    @{FirstName="John"; LastName="Doe"; Username="john.doe"},
    @{FirstName="Jane"; LastName="Smith"; Username="jane.smith"}
)

foreach ($user in $users) {
    $FirstName = $user.FirstName
    $LastName = $user.LastName
    $Username = $user.Username
    $Password = ConvertTo-SecureString "P@ssw0rd123!" -AsPlainText -Force
    $OU = "OU=IT,DC=yourinitialsnsamitt,DC=ca"  # Adjust the OU as needed

    New-ADUser -GivenName $FirstName `
               -Surname $LastName `
               -SamAccountName $Username `
               -UserPrincipalName "$Username@yourinitialsnsamitt.ca" `
               -Name "$FirstName $LastName" `
               -DisplayName "$FirstName $LastName" `
               -Path $OU `
               -AccountPassword $Password `
               -Enabled $true `
               -ChangePasswordAtLogon $true
}
```

### **4.4 Assign Administrative Permissions (e.g., IT Members)**

1. **Open Active Directory Users and Computers (ADUC)**.
   - On the **Domain Controller**, open **Active Directory Users and Computers** (ADUC) from **Server Manager** → **Tools** → **Active Directory Users and Computers**.

2. **Locate the User Account**.
   - In **ADUC**, navigate to the **Organizational Unit (OU)** where the user is located (e.g., **IT**).
   - Find the user account (e.g., `lime.user`).

3. **Assign User to a Group**.
   - Right-click on the user account and select **Add to Group**.
   - In the **Enter the object names to select** field, type the name of the group to which you want to assign the user, such as:
     - **Domain Admins** (for administrative permissions)
     - **Enterprise Admins** (for higher-level permissions)
     - **IT** (for internal IT staff)
   - Click **Check Names** to verify the group, then click **OK**.

4. **Verify Group Membership**.
   - To ensure the user has been added to the group, right-click the user account again and select **Properties**.
   - Navigate to the **Member Of** tab and verify that the correct group (e.g., **Domain Admins**) is listed.

5. **Repeat for Additional Users**.
   - Repeat the steps for any other users who need administrative permissions (e.g., adding them to **Domain Admins** or **IT**).
  
## **Step 5: Configure New Roles on the Domain Controllers**

### **Install All Roles and Services on the Domain Controller**
1. **Open Server Manager** on the **Domain Controller**.
2. Click on **Manage** → **Add Roles and Features**.
3. Follow the wizard to install the following roles and services:
   - **DHCP Server**
   - **DNS Server**
   - **File and Storage Services** (for File Server role)
   - Other roles as required for your network environment.

### **Configure DHCP on the Domain Controller**

1. **Open DHCP Management Console**:
   - On the Domain Controller, go to **Server Manager** → **Tools** → **DHCP**.

2. **Create Reserved Addresses**:
   - In the **DHCP Management Console**, expand your DHCP server.
   - Right-click **Reservations** and select **New Reservation**.
   - Enter the following:
     - **Reservation Name**: Give the reservation a descriptive name (e.g., "Server 01").
     - **IP Address**: Enter the IP address you want to reserve for the server.
     - **MAC Address**: Enter the MAC address of the device.
     - **Description**: Optional, but can describe the device.

   - Click **Add** to create the reservation.

3. **Configure Static IP Reservations for Servers**:
   - Ensure that **all servers** in your environment are reserved in the DHCP server.
   - Repeat the process above for each server (e.g., DC01, SVR01).

4. **Configure PCs to Get IP Addresses via DHCP Scope**:
   - On your **DHCP Server**, go to **Scope Options**.
   - Set the appropriate **scope** for the PCs. 
   - Make sure that the PCs are configured to receive IP addresses automatically from the DHCP scope (default is typically 192.168.30.0/24).

### **Configure DNS Requirements**

1. **Configure Domain Controller with Appropriate DNS Records**:
   - On the **Domain Controller**, open the **DNS Manager** (from **Server Manager** → **Tools** → **DNS**).
   - Right-click on your domain (e.g., **yourdomain.com**) and select **Properties**.
   - Under the **Name Servers** tab, ensure that the Domain Controller is listed as a DNS server.
   - Ensure that all relevant DNS records (such as A records for servers) are created automatically by Active Directory.

2. **Create a Reverse Lookup Zone for Your Domain**:
   - In **DNS Manager**, right-click **Reverse Lookup Zones** and select **New Zone**.
   - Choose **Primary Zone** and ensure the zone type is set to **Reverse Lookup Zone**.
   - Define the **Network ID** for your addressing space (e.g., for the 192.168.30.0/24 network, the Network ID would be 192.168.30).
   - Complete the wizard to create the reverse lookup zone.

### **Configure File Server Requirements**

1. **Create the Shared Folder Structure**:
   - On the **File Server**, create a folder structure for the shared files:
     - Example: 
       - `\\fileserver\Sales`
       - `\\fileserver\HR`
       - `\\fileserver\IT`
   - Right-click on the folder and select **Properties** → **Sharing** tab.
   - Click **Advanced Sharing**, and then click **Share this folder**.

2. **Ensure File Server Role is Configured Correctly**:
   - In the **File and Storage Services** section of **Server Manager**, click **Shares**.
   - Ensure that the shared folders are listed and accessible.
   - Set permissions to ensure users from the domain can access the share. You may need to add appropriate **Domain Users** group permissions.

3. **Configure Security Groups for Departments**:
   - Create a **security group** for each department (e.g., **Sales**, **HR**, **IT**).
   - Go to **Active Directory Users and Computers (ADUC)**.
   - Right-click on the **Groups** container and select **New Group**.
   - Name the group (e.g., **Sales**), and assign it to the **appropriate organizational unit** (OU).

4. **Add Users to the Appropriate Security Group**:
   - In **ADUC**, locate each department's group (e.g., **Sales**).
   - Right-click on the group, and select **Properties**.
   - Under the **Members** tab, click **Add** and select the users that belong to that department.

5. **Configure Group Policy to Automatically Deploy Shared Folders**:
   - Open the **Group Policy Management Console (GPMC)** from **Server Manager** → **Tools** → **Group Policy Management**.
   - Right-click on **Group Policy Objects** and select **New** to create a new GPO.
   - Name the GPO (e.g., **Deploy Shared Folders**).
   - Right-click the GPO and select **Edit**.
   - Navigate to: **User Configuration** → **Preferences** → **Windows Settings** → **Drive Maps**.
   - Right-click and select **New** → **Mapped Drive**.
   - Set the following:
     - **Location**: The path to the shared folder (e.g., `\\fileserver\Sales`).
     - **Reconnect**: Check the box to reconnect the drive at logon.
   - Link the GPO to the **appropriate Organizational Unit (OU)** that contains the users.

### **Folder/Share Minimum Requirements (Best Practices)**

1. **Delete Permission is Disabled**:
   - In the shared folder properties, under the **Security** tab, ensure **Delete** permission is **disabled** for users.
   
2. **Users Shouldn’t Be Able to Make Changes to the Settings of the Folder/Share**:
   - Ensure **Full Control** permission is granted only to administrators and relevant users.
   - In the **Security** tab, limit the permissions for the users to **Read** and **Write**, but not modify the folder settings.

3. **Appropriate Ownership to Allow the Correct Access**:
   - Set the **Owner** of the shared folder to an appropriate administrator group.
   - Right-click on the folder, select **Properties**, and go to the **Security** tab.
   - Click **Advanced** and then set the **Owner** to an administrative group like **Domain Admins**.

## **Step 6 (Optional): Schedule a One-Time Backup on a Network Drive**

### **1. Open Windows Server Backup**
   - On the **Domain Controller** or the **Backup Server**, open **Server Manager**.
   - From **Server Manager**, click **Tools** → **Windows Server Backup**.

### **2. Initiate Backup Wizard**
   - In the **Windows Server Backup** window, click on **Local Backup** in the left-hand pane.
   - In the right-hand pane, click **Backup Once** under the **Actions** section.
   - This will start the **Backup Once Wizard**.

### **3. Select Backup Configuration**
   - In the **Backup Once Wizard**, choose **Different options** and click **Next**.
   - Select **Custom** to configure a one-time backup manually, and click **Next**.

### **4. Select Items to Back Up**
   - In the **Select Items for Backup** window, choose the appropriate volumes and folders that you want to back up.
   - For example, select the **C:\** drive and any other critical data folders (e.g., **D:\Backup**).
   - Click **Next**.

### **5. Choose Backup Destination**
   - Select **Network share** as the backup destination.
   - Enter the **network path** for the shared backup folder on the network drive (e.g., `\\BackupServer\BackupShare\`).
   - Enter the **username** and **password** to authenticate access to the network drive.
   - Click **Next**.

### **6. Specify Backup Type**
   - Select the appropriate backup type:
     - **Full Backup** (if you want a complete backup of the selected data).
     - **Incremental Backup** (if you want only the changes made since the last full backup).
   - For a one-time backup, it’s recommended to choose a **Full Backup**.
   - Click **Next**.

### **7. Set Backup Schedule**
   - In the **Schedule** section, select **Once** to schedule a one-time backup.
   - Specify the **date and time** you want the backup to occur.
   - Make sure to set a time that aligns with the least disruptive period for the system.
   - Click **Next**.

### **8. Review the Backup Settings**
   - The **Backup Once Wizard** will show a summary of your backup settings.
   - Review all selections:
     - Backup source: Folders and volumes selected.
     - Destination: Network share.
     - Backup type: Full or Incremental.
     - Schedule: One-time backup with the specified date and time.
   - If everything is correct, click **Backup** to start the process.

### **9. Monitor the Backup Process**
   - The backup will begin and can take some time depending on the amount of data being backed up.
   - The progress will be displayed in the **Windows Server Backup** window.

### **10. Verify Backup Completion**
   - After the backup process is completed, you will receive a notification stating whether the backup was successful.
   - To verify, go to the **network share folder** and ensure the backup files are present.
   - Ensure the backup folder has a new timestamp matching the scheduled backup time.

### **11. Set Up Email Notifications (Optional)**
   - To receive an email notification upon completion of the backup, configure email alerts in **Windows Server Backup** settings.
   - Click on **Action** → **Configure Email Notification** in the **Windows Server Backup** window.
   - Enter SMTP settings and recipient email address to receive backup notifications.

## **Step 7 (Recommended): Configure and Apply the GPOs to the Domain**

### **1. Open Group Policy Management Console (GPMC)**
   - On your **Domain Controller** or **Server**, open **Server Manager**.
   - From the **Server Manager** window, click on **Tools** → **Group Policy Management**.
   - This opens the **Group Policy Management Console (GPMC)**.

### **2. Create a New Group Policy Object (GPO)**
   - In **Group Policy Management**, expand the **Forest** → **Domains** → select your domain.
   - Right-click on **Group Policy Objects** and select **New**.
   - Name the new GPO something meaningful like **"Security Policies GPO"**.
   - Click **OK**.

### **3. Edit the GPO**
   - Right-click the newly created GPO and select **Edit**.
   - The **Group Policy Management Editor** opens, where you can configure the policies.

### **4. Configure Password Policy**
   - In the **Group Policy Management Editor**, navigate to:
     - **Computer Configuration** → **Policies** → **Windows Settings** → **Security Settings** → **Account Policies** → **Password Policy**.
   - Configure the following settings:
     - **Enforce password history**: Set to **5 passwords remembered**.
     - **Maximum password age**: Set to **90 days**.
     - **Minimum password length**: Set to **8 characters**.
     - **Complexity requirements**: Set to **Enabled** to require strong passwords (e.g., upper and lowercase letters, numbers, symbols).
     - **Minimum password age**: Set to **1 day** to prevent immediate password changes.
   - Close the editor after configuring the settings.

### **5. Configure Account Lockout & Reset Policy**
   - In the **Group Policy Management Editor**, navigate to:
     - **Computer Configuration** → **Policies** → **Windows Settings** → **Security Settings** → **Account Policies** → **Account Lockout Policy**.
   - Configure the following settings:
     - **Account lockout threshold**: Set to **5 invalid logon attempts**.
     - **Account lockout duration**: Set to **15 minutes**.
     - **Reset account lockout counter after**: Set to **15 minutes**.
   - This will ensure accounts are locked after multiple failed login attempts and will reset after a specified duration.

### **6. Restrict Access to Command Prompt & PowerShell for Regular Users**
   - In the **Group Policy Management Editor**, navigate to:
     - **User Configuration** → **Policies** → **Administrative Templates** → **System**.
   - Configure the following settings:
     - **Prevent access to the command prompt**: Set to **Enabled**.
     - **Disable the Command Prompt script processing**: Set to **Enabled**.
   - For PowerShell, navigate to:
     - **User Configuration** → **Policies** → **Administrative Templates** → **System** → **Prevent access to the command prompt**.
   - Set **Prevent access to PowerShell**: Set to **Enabled**.
   - This will restrict regular users from accessing the command prompt and PowerShell.

### **7. Restrict Access to Control Panel**
   - In the **Group Policy Management Editor**, navigate to:
     - **User Configuration** → **Policies** → **Administrative Templates** → **Control Panel**.
   - Enable the setting:
     - **Prohibit access to Control Panel and PC settings**: Set to **Enabled**.
   - This will restrict regular users from accessing the Control Panel.

### **8. Disable Shut Down/Power Off Options**
   - In the **Group Policy Management Editor**, navigate to:
     - **User Configuration** → **Policies** → **Administrative Templates** → **Start Menu and Taskbar**.
   - Enable the setting:
     - **Remove and prevent access to the Shut Down, Restart, Sleep, and Hibernate commands**: Set to **Enabled**.
   - This will prevent regular users from shutting down or restarting the computer.

### **9. Disabling the View of Recently Logged-On Users**
   - In the **Group Policy Management Editor**, navigate to:
     - **User Configuration** → **Policies** → **Administrative Templates** → **Start Menu and Taskbar**.
   - Enable the setting:
     - **Do not display the ‘Recently Opened Documents’ on Start**: Set to **Enabled**.
     - **Do not keep a history of recently opened documents**: Set to **Enabled**.
     - **Do not show recent user names in the Start menu**: Set to **Enabled**.
   - This will ensure that recently logged-on users are not visible on the Start Menu.

### **10. Link the GPO to the Domain**
   - After configuring the policies, close the **Group Policy Management Editor**.
   - Back in the **Group Policy Management Console (GPMC)**, right-click on **Domain** and select **Link an Existing GPO**.
   - Select the **Security Policies GPO** you just created.
   - Click **OK** to apply the GPO to the domain.

### **11. Force GPO Update**
   - On the **Domain Controller**, open **Command Prompt** as an Administrator.
   - Run the following command to force the GPO to apply immediately:
     ```bash
     gpupdate /force
     ```

### **12. Verify GPO Application**
   - To ensure that the GPO has been successfully applied, run the following command on a client machine or server:
     ```bash
     gpresult /r
     ```
   - This will display the resultant set of policies and confirm that the GPO is being applied correctly.

### **Best Practices**:
- Test GPO changes in a **test environment** before applying them to production.
- Regularly review and audit the GPOs to ensure compliance with security requirements.
- Document any changes made to GPOs for future troubleshooting and audits.

---

## **Reversion Information**

In case the process needs to be reversed or rolled back, follow the steps outlined below to undo the changes made during the server configuration:

### **Reversion for Network Configuration**
- **Action**: To revert the IP scheme, DHCP, and DNS settings:
  1. **Restore the original IP configuration** for servers and computers.
  2. **Revert DHCP configurations** to the previous settings if static reservations were modified.
  3. **Reset DNS settings** on the Domain Controller to the default settings.
  
### **Reversion for Domain Controller Setup**
- **Action**: To remove a Domain Controller and revert Active Directory and DNS changes:
  1. **Demote the Domain Controller**: Run the `Remove-DomainController` PowerShell command on the server.
  2. **Uninstall Active Directory Domain Services**: Use the "Server Manager" to uninstall AD DS.
  3. **Remove DNS Role**: Remove the DNS role using "Add Roles and Features" in Server Manager.
  4. **Restore previous network configurations** if necessary.

### **Reversion for Group Policy**
- **Action**: To revert applied Group Policy Objects (GPOs):
  1. **Open Group Policy Management Console (GPMC)**.
  2. **Delete or disable the GPOs** related to password policies, command prompt restrictions, etc.
  3. **Force a GPO update** using the `gpupdate /force` command to apply changes immediately.

### **Reversion for File Server Configuration**
- **Action**: To revert file server roles and permissions:
  1. **Unshare folders**: Remove any shared folders configured during the setup.
  2. **Revert file system permissions**: Modify the folder access permissions back to their original state.
  3. **Delete security groups** created for department-specific shares.
  
#### 8.5 **Reversion for Backup Configuration**
- **Action**: To undo scheduled backup configuration:
  1. **Remove backup jobs** created for the system by navigating to the "Windows Server Backup" utility.
  2. **Delete backup schedules** by disabling any existing tasks in Task Scheduler.
  3. **Delete backup data** from the network share.

### **General Reversion Procedure**
- **Action**: If a general rollback is required:
  1. **Restore from backup**: Restore the system state from the last valid backup if critical configuration steps were incorrect.
  2. **Revert all changes made**: Uninstall roles, remove users or groups created, and reset settings to the default state before the configuration began.

### Notes:
- Ensure that backups are performed before making any changes to configurations.
- Testing the reversal steps in a controlled environment is recommended before applying in production.
- Any configuration changes should be documented to ensure proper rollback if needed.

---

## **Security and Compliance Considerations**

In this section, we discuss the critical security and compliance requirements for deploying, configuring, and maintaining the domain controllers, file servers, and network infrastructure as part of this process.

### **Securing Domain Controllers**

1. **Physical Security**:
   - Ensure that all domain controllers are stored in physically secure locations with restricted access.
   - Use server racks with lockable doors, and limit physical access to authorized personnel.

2. **Account Security**:
   - Configure the Domain Admin account with strong, complex passwords and enable multi-factor authentication (MFA) where possible.
   - Disable the default `Administrator` account and create a unique administrator account with appropriate permissions.

3. **Group Memberships**:
   - Regularly review and audit group memberships, especially for high-level access groups like Domain Admins and Enterprise Admins.
   - Use role-based access control (RBAC) to ensure users are only members of security groups that align with their job responsibilities.

4. **Secure Network Traffic**:
   - Configure domain controllers to enforce the use of secure protocols, such as LDAP over SSL (LDAPS), for communication.
   - Use IPsec or other encryption methods to secure data transmission between servers and clients on the network.

### **Network and System Security**

1. **Firewall Configuration**:
   - Ensure firewalls are properly configured to only allow necessary ports for server communication (e.g., DNS, LDAP, HTTP/HTTPS, RDP).
   - Block all unused ports to reduce attack surface.

2. **Regular Patching and Updates**:
   - Schedule regular patching of all servers and workstations to ensure they are protected from vulnerabilities.
   - Use Windows Server Update Services (WSUS) or other patch management systems to deploy updates.

3. **Secure DNS Configuration**:
   - Ensure DNS records are accurate and validated to prevent DNS spoofing or man-in-the-middle attacks.
   - Use DNSSEC (DNS Security Extensions) to secure the DNS infrastructure and prevent tampering.

4. **Network Segmentation**:
   - Segment the network into multiple VLANs to isolate sensitive data and systems from general access (e.g., separate server VLANs from user VLANs).
   - Use network access control lists (ACLs) to control communication between different network segments.

### **File Server Security**

1. **Permissions and Access Control**:
   - Ensure that NTFS permissions on shared folders are configured with the principle of least privilege.
   - Use security groups to control access to specific folders for different departments (e.g., `HR_Group`, `Finance_Group`).

2. **Data Encryption**:
   - Enable BitLocker or other disk encryption technologies to protect data at rest on file servers.
   - Consider using EFS (Encrypting File System) for additional file-level encryption.

3. **Backup Security**:
   - Ensure that backups are encrypted, both during transfer and while stored on the network drive.
   - Set up access controls for backup files so that only authorized users can restore data.

4. **Audit Logging**:
   - Enable file access auditing on shared folders to track who accessed files and what actions they performed.
   - Regularly review audit logs to identify suspicious activity or unauthorized access.

### **Compliance Requirements**

1. **Data Protection Regulations**:
   - Ensure compliance with data protection laws such as GDPR, CCPA, HIPAA, or other relevant regulations for the organization.
   - Implement data retention policies, ensuring sensitive data is not kept longer than necessary.

2. **Backup Retention and Disaster Recovery**:
   - Develop a disaster recovery plan that includes regular backups, off-site storage, and clear procedures for restoring critical systems.
   - Store backups in a secure, encrypted location and test recovery processes regularly.

3. **Security Monitoring and Alerts**:
   - Use security information and event management (SIEM) tools to monitor the network and domain controllers for suspicious activity.
   - Set up alerts to notify administrators about potential security breaches or misconfigurations.

4. **Employee Awareness and Training**:
   - Regularly conduct security awareness training for employees to prevent social engineering attacks and phishing attempts.
   - Ensure that employees know how to handle sensitive data and adhere to security policies.

### **Secure Group Policy Configuration**

1. **Password and Account Lockout Policies**:
   - Configure password policies to enforce strong password creation rules, including length, complexity, and expiration.
   - Set account lockout policies to prevent brute force attacks by locking accounts after a specified number of failed login attempts.

2. **Restricting Access to Sensitive Systems**:
   - Use Group Policy Objects (GPOs) to restrict access to command prompt and PowerShell for non-administrative users.
   - Apply GPOs to hide or remove access to the Control Panel and Settings for regular users.

3. **User and Device Monitoring**:
   - Configure Group Policy to enforce logging of user logins and critical system changes.
   - Monitor the audit logs regularly to ensure compliance and detect potential security issues early.

### **Final Recommendations**

1. **Regular Audits**:
   - Perform periodic security audits of the domain, server roles, and user permissions to ensure compliance with security best practices.
   - Conduct penetration tests and vulnerability assessments to identify potential threats.

2. **Continuous Improvement**:
   - Continuously review and update security measures as new threats and vulnerabilities emerge.
   - Implement a continuous improvement cycle for security practices, ensuring all measures are up to date.

3. **Incident Response**:
   - Establish an incident response plan to quickly address any security breaches, data leaks, or attacks.
   - Train staff on the incident response process, ensuring swift action in case of an emergency.

---

## **Appendix**

The Appendix section provides supplementary information, such as additional resources, references, and any related documentation that support the process described in the document. This section ensures that users have access to any additional details that may be necessary for implementing the process successfully. It also includes any troubleshooting steps, FAQs, and contact information for further assistance.

### **Additional Resources**

1. **Windows Server Documentation**:
   - Official Microsoft Documentation: [Windows Server Documentation](https://docs.microsoft.com/en-us/windows-server/)
   - Windows Server Backup Guide: [Windows Server Backup](https://docs.microsoft.com/en-us/windows-server/administration/windows-server-backup/windows-server-backup)

2. **Active Directory Resources**:
   - Understanding Active Directory: [Active Directory Overview](https://docs.microsoft.com/en-us/windows-server/identity/active-directory-domain-services)
   - Managing Active Directory: [Active Directory Administration](https://docs.microsoft.com/en-us/windows-server/identity/active-directory-domain-services/active-directory-administrative-tools)

3. **DHCP Configuration**:
   - Configuring DHCP on Windows Server: [DHCP Server Configuration](https://docs.microsoft.com/en-us/windows-server/networking/technologies/dhcp/dhcp-server)

4. **DNS Configuration**:
   - DNS in Windows Server: [DNS Server Configuration](https://docs.microsoft.com/en-us/windows-server/networking/dns/dns-topology)

5. **Group Policy Overview**:
   - Group Policy Settings: [Group Policy Settings Documentation](https://docs.microsoft.com/en-us/windows-server/group-policy/group-policy-settings)

### **Troubleshooting**

- **Problem**: **DNS Not Resolving Domain Name**
  - **Solution**: Verify that DNS records for the domain controller are properly configured. Ensure the domain controller's IP address is statically set and that the DNS service is running on the server.
  - Check the network settings to ensure there are no conflicts with IP addresses and that the proper DNS server is specified on the client machines.

- **Problem**: **Users Cannot Log In After Domain Join**
  - **Solution**: Ensure the domain controller is up and running. Verify the client machine's network settings and ensure it can communicate with the domain controller.
  - Confirm that the user accounts exist in Active Directory and are not locked or disabled.

### **Frequently Asked Questions (FAQs)**

1. **Q**: What happens if I forget to back up Active Directory regularly?
   - **A**: If Active Directory is not regularly backed up, you risk losing user accounts, group memberships, and domain settings in case of system failure. It's critical to schedule daily backups of the system state, including Active Directory, to prevent data loss.

2. **Q**: Can I restore a system from a backup on different hardware?
   - **A**: Yes, you can restore a system from a backup on different hardware, but hardware-specific drivers and settings may require adjustment after the recovery process. It's recommended to perform hardware-independent restores and ensure compatibility with the new hardware.

3. **Q**: How do I secure my backups?
   - **A**: To secure backups, encrypt the backup files both in transit and at rest. Ensure that access to backup locations is restricted to authorized personnel only. Regularly audit backup access logs and apply security patches to the backup server.

### **Glossary**

- **Active Directory (AD)**: A directory service developed by Microsoft for Windows domain networks. It stores information about objects on the network and makes this information easy for administrators and users to find and access.
  
- **DNS (Domain Name System)**: A system for translating domain names (like `www.example.com`) into IP addresses (like `192.0.2.1`) so that computers can communicate with each other.

- **DHCP (Dynamic Host Configuration Protocol)**: A network management protocol used on IP networks. DHCP automatically assigns IP addresses to devices on the network, ensuring proper addressing and configuration.

- **Group Policy**: A feature of Microsoft Windows that allows IT administrators to define configurations for users and computers within an Active Directory environment.
