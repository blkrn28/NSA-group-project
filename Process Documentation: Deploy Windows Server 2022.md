# **Process Documentation: Deploy Windows Server 2022**

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

#### **3.1.1 Assign IP Addresses**  for an example

- **Domain Controller (DC) Server**  
  - **IP Address**: `192.168.30.100`  
  - **Subnet Mask**: `255.255.255.0`  
  - **Gateway**: `192.168.30.1`  
  - **DNS Server**: `192.168.30.100` (DNS will be configured later)  

- **Other Servers (SVR01, SVR02, etc.)**  
  - **IP Address**: `192.168.30.110` for SVR01, `192.168.30.120` for SVR02  
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
3. Enter the **Root domain name**: `yourinitialsnsamitt.ca` (replace with your domain name).  
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
3. Follow the wizard to create a **Primary Zone** with the domain name `yourinitialsnsamitt.ca`.  
4. Confirm that the DNS server is correctly resolving domain names within the domain by performing a test from a client computer:  
   - Open **Command Prompt** and type `nslookup yourinitialsnsamitt.ca`.  

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
3. Click **Change**, select **Domain**, and enter the domain name `yourinitialsnsamitt.ca`.
4. Enter domain administrator credentials when prompted.
5. Restart the server for the changes to take effect.
6. Confirm the server is joined to the domain by checking **System Properties**.

#### **3.7.2 Join Computers to the Domain**
1. On each workstation, go to **Control Panel** → **System and Security** → **System**.
2. Click **Change settings** under **Computer Name, Domain, and Workgroup Settings**.
3. Click **Change**, select **Domain**, and enter `yourinitialsnsamitt.ca`.
4. Enter domain administrator credentials when prompted.
5. Restart the computer for the changes to take effect.
6. Confirm that the computer is part of the domain by checking **System Properties**.

### **3.8 Verify Domain Configuration**
1. On the **Domain Controller**, open **Active Directory Users and Computers** to verify that all joined devices (servers and workstations) are listed under the **Computers** Organizational Unit (OU).
2. Verify that the domain is functional by using the following command on any device:
   ```powershell
   ping yourinitialsnsamitt.ca

## **Step 4: Create and Configure Users and OUs for Environment**

### **4.1 Create Organizational Units (OUs) with GUI**
1. On the **Domain Controller**, open **Active Directory Users and Computers** (ADUC) from **Server Manager** → **Tools** → **Active Directory Users and Computers**.
2. In the **ADUC** window, right-click on the domain name (`yourinitialsnsamitt.ca`), then select **New** → **Organizational Unit**.
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
  
