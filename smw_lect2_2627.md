Computer script on a screen

## **ST2612 – Secure Microsoft Windows**

***Lecture 2***

*Managing and Administering DNS*

*Perform system administration on the Windows	Server Network	Operating System*

**Recap**

Active Directory (or ADDS) is a directory service to house information about network resources

Servers housed the Active Directory are called domain controllers (DCs)

- They all host an identical Active Directory Database. (Ideally)

An Active Directory is made up with many **objects**.

- The Active Directory is a database to store all the objects.

The **global catalog** stores reference information about every object, replicates key Active Directory elements

- provide forest wide directory lookup
- provide cross domain logon support
- reduce replication overhead (implicit)

Active Directory is a hierarchy of logical containers: **forests**, **domains**, **organizational units** and **sites**

**Learning Objectives – Part 1**

1. **Perform the configuration and testing of DNS**

2. **Configure DNS Services**

3. **Describe the relationship between DNS Zones, DNS Replication and Stub Zone**

4. **Use DNS Management Console**

**DNS Fundamentals**

**Understanding the DNS Role: Domain Name System (DNS)** is a hierarchical namespace used to identify computers on IP networks, primarily by resolving **Fully Qualified Domain Names (FQDNs)** to IP addresses.

1. The client computer checks its own DNS cache for the IP address.
2. If not found, it asks the organization’s DNS server.
3. If the organization DNS has it cached, it replies immediately. If not, the request is sent to the ISP’s DNS server.
4. If the ISP DNS has it cached, it replies immediately. If not, the ISP DNS contacts the .com top-level DNS server.

5. The .com server points to the microsoft.com DNS server.
6. The ISP DNS asks the microsoft.com DNS server for the IP.
7. The microsoft.com DNS server returns the IP, which gets cached and passed back (ISP → organization → client).
8. ISP DNS passed back and the IP cache on Organization DNS
9. Organization DNS passed back the IP to client
10. The client caches the IP and uses it to connect to the website.

**Introduction to DNS Configuration Prerequisites**

**Understanding DNS:** The Domain Name System (DNS) is a hierarchical namespace that identifies computers on IP networks, mapping easy-to-remember hostnames to IP addresses.

**Active Directory Integration:** DNS is a **requirement for Active Directory (AD)** to function properly; it provides locator records that allow clients to find domain controllers.

**Installation:** The DNS Server role can be installed using the **Add Roles and Features Wizard** in Server Manager or via PowerShell using the Install-WindowsFeature -Name DNS command.

**Installation via Server Manager:**

- Open **Server Manager** and select **Add Roles and Features**.
- Choose **Role-based or feature-based installation** and select the destination server.
- Check the **DNS Server** box and confirm the addition of required management tools.
- Complete the wizard and click **Install**.

**Administration Tools:** DNS is managed primarily through the **DNS Manager MMC** or Windows PowerShell cmdlets.

**Configuring Primary Forward and Reverse Lookup Zones**

**Forward Lookup Zones:** These are traditional zones used to resolve a hostname (FQDN) into an IP address.

**Reverse Lookup Zones:** These zones resolve an IP address back into a hostname, typically using **Pointer (PTR) records**.

**Active Directory Integrated Zones:** Storing zones in Active Directory ensures **fault tolerance** by replicating data across all domain controllers in a multi-master configuration.

**Forwarders and Name Resolution Flows**

**Recursive vs. Iterative Queries:** If a local DNS server cannot resolve a request, it performs a **recursive query** by contacting root servers or uses a **forwarder**.

**Default Forwarders:** DNS servers can be configured to relay requests for external zones they do not host to ISP DNS servers or other organization servers.

**Conditional Forwarders:** These allow you to forward queries for specific domains (e.g., a partner's network) to specific DNS server IP addresses, rather than using standard recursion.

**Root Hints:** These files contain the IP addresses of top-level DNS zones and are used as a **last resort** for recursion if forwarders are unavailable.

**DNSSEC:** Domain Name System Security Extensions **digitally sign zones** to prevent attackers from hijacking the lookup process via cache poisoning.

**Forwarder vs Root Hints**

Both approach have pros and cons in terms of operational concerns or security concerns

**Forwarder**

- Faster - Recursive query : try to provide answer (possibly non-authoritative)
- Local DNS has no way to ensure the security configuration of an external Forwarder
- Potential to enable customizable DNS service within an Enterprise (For security enhancement at DNS level) e.g., DNS filtering, logging

**Root Hints**

- Slower - due to interactive queries, try to provide the best answer
- More reliable (13 Root Hints are supported by > 200 actual load-balancing servers)

**Configuring DNS Zones**

**Zone Types and Authorities:**

- **Primary Zone:** The master read/write copy of a zone file.
- **Secondary Zone:** A read-only copy of a primary zone used for load balancing and fault tolerance; it receives updates via **zone transfers**.
- **Stub Zone:** Contains only essential records (SOA, NS, and A) to identify authoritative servers for another zone.

**Forward and Reverse Lookup Zones:**

- **Forward Lookup Zones** are the most common; they map hostnames to IP addresses.
- **Reverse Lookup Zones** map IP addresses back to hostnames, which is essential for certain network security and troubleshooting tools.

**Active Directory Integration:** Storing DNS zones within **Active Directory** allows for **multi-master replication** to all domain controllers and enables **secure dynamic updates**.

**Stub Zones**

**Definition**: A **Stub Zone** is a specialized, non-authoritative DNS zone that contains only the records necessary to identify the authoritative name servers for another zone.

**Contents**: Unlike a full zone, a stub zone only stores the **Start of Authority (SOA)** record, **Name Server (NS)** records, and necessary **glue Host (A)** records for the target zone.

**Functional Relationship**:

- **Efficiency**: It enables a DNS server to resolve records in a remote domain (such as a partner organization or a different business unit) more efficiently by going directly to the target's authoritative servers rather than using standard recursion.
- **Stub Zones vs. Conditional Forwarders**: While both perform similar tasks, stub zones automatically track changes if the target domain's authoritative name servers are updated, whereas conditional forwarders typically rely on manually configured static IP addresses.

**Relationship to Replication**: Like primary zones, stub zones can be **Active Directory-integrated**, allowing the authoritative server list to be replicated automatically to all domain controllers in the infrastructure.

**DNS Replication**

**Role of Replication**: **DNS Replication** is the process that ensures zone information remains identical across all authoritative DNS servers within a network.

**Traditional Zone Transfers**: For standard primary and secondary zones, replication occurs via **zone transfers**, where the secondary DNS server periodically copies updated records from the primary server.

**Active Directory (AD)-Integrated Replication**:

- Zones can be stored directly within the Active Directory database, allowing them to utilize **Active Directory replication** mechanisms instead of traditional zone transfers.
- **Multi-Master Model**: In an AD-integrated environment, any domain controller running the DNS service can accept updates, and those changes are automatically replicated to all other DNS servers in the domain or forest.

**Benefits**: Integration with Active Directory provides automatic fault tolerance, higher availability, and supports **secure dynamic updates**, ensuring only authenticated computers can register their own records.

**Managing DNS Resource Records**

Resource records hold specific information about services or FQDNs within a zone.

**Advanced Configuration**

**Forwarding Strategies:**

- **Default Forwarders:** DNS servers configured to relay any queries they cannot resolve locally to an external server, such as an ISP's DNS.
- **Conditional Forwarders:** Forwards queries for a **specific domain** directly to that domain's authoritative DNS servers, often used during company mergers.

**Managing Dynamic Updates and Scavenging:**

- **Dynamic DNS (DDNS)** allows clients to automatically register their records.
- **Scavenging** should be enabled to automatically remove **stale resource records** from computers no longer on the network.

**Testing and Troubleshooting DNS**

**nslookup Utility:** The standard command-line tool for testing forward and reverse lookups. It identifies if a response is **authoritative** (from a zone file) or **non-authoritative** (from cache).

**DNS Manager Monitoring:** Use the **Monitoring tab** in DNS server properties to perform manual or automatic simple and recursive query tests.

**Cache Management:**

- Use **ipconfig /flushdns** to clear the local client resolver cache.
- Use **Clear-DnsServerCache** in PowerShell or DNS Manager to clear the server-side cache.
- **Clear-DnsClientCache / ipconfig /flushdns:** Used to clear the local resolver cache on a client if it contains outdated information.

**PowerShell Verification:** Use Resolve-DnsName to query records and Test-NetConnection -Port 53 to verify the DNS service is responding over the network.

**Logging:** The **DNS Server log** in Event Viewer tracks service events, while **debug logging** can be enabled to record packet-by-packet query information.

**Service Restart:** Many DNS issues are resolved by simply **restarting the DNS Server service** via PowerShell or DNS Manager.

**Introduction to the DNS Manager**

**Accessing the Tool**: In Windows Server 2022, the primary way to manage DNS is through the **DNS Manager**, which is a specialized Microsoft Management Console (MMC) snap-in. You can access it by selecting **DNS from the Tools menu in Server Manager**.

**Interface Overview**:

- **Navigation Pane**: Located on the left, it displays the DNS server name and folders for **Forward Lookup Zones**, **Reverse Lookup Zones**, **Trust Points**, and **Conditional Forwarders**.
- **Details Pane**: Displays the specific **resource records** within a selected zone, including their name, type, data (IP address), and timestamps for dynamic updates.

**Server Actions**: By right-clicking the server object in the navigation pane, administrators can perform high-level tasks such as **restarting the DNS Server service**, **clearing the DNS cache**, or launching the **nslookup** command-line utility.

**Managing DNS Zones and Resource Records**

**Creating New Zones**: Using the **New Zone Wizard**, you can create various zone types by right-clicking the Lookup Zone folders.

**Configuring Resource Records**: Resource records map names to IP addresses and are the core data of the DNS database.

**Dynamic Updates**: When enabled, computers automatically create or update their own host records in the zone at boot time, reducing administrative effort.

**DNS Implementation Plan**

**Best practice** recommendations

- Implement Windows Server DNS servers instead of other versions of DNS, and use Active Directory
- The resource records and zones that can be set up for IPv4 can also be set up for IPv6
- Consider using namespaces to represent natural organizational boundaries
- Make sure the DNS servers on a private network are well secured in terms of Windows Server security options
- Plan to locate a DNS server across most site links
- Create two or more DNS servers to take advantage of the load balancing, the multi-master relationships (With AD-integrated zones), and the fault tolerance.
- Designate one DNS server as a forwarder to reduce traffic
- Blocking your network clients to access to external DNS directly to prevent data exfiltration via DNS tunneling.

**DNS Threats**

Enterprise Administrator shall watch out for the following possible DNS related threats

**DDOS with DNS Amplification Attack**

- Bogus queries are sent to a DNS to trigger it to send high volume of replies to the targeted victim

**Cache Poisoning**

- The DNS cache / Stub Zone / Forwarder is comprised,  causing the DNS to return incorrect IP addresses, diverting traffic to the attacker's computer (or any other computer).

**Registrar Hijacking**

- Also being known as Domain hijacking, is the act of changing the registration of a domain without the permission of its original registrant.
- It can be done in several ways, generally by exploiting a vulnerability in the domain name registration system or through social engineering.

**Learning objectives – part 2**

5. **Perform system administration on Windows Server Operating System**

6. **Explain User Account Management**

7. **Configure, Manage and Troubleshoot Resource Access**

8. **Configure and Manage Data Storage**

9. **Use PowerShell for administration efficiency**

**Windows Server 2022 Administration Tools**

**Server Manager** is the primary graphical user interface (GUI) tool for managing local and remote servers, allowing administrators to monitor server health and manage the lifecycle of server roles and features.

**Windows PowerShell** offers a powerful command-line shell and scripting environment that allows administrators to automate repetitive tasks and manage system configurations through cmdlets.

**Microsoft Management Console (MMC)** snap-ins remain essential for specialized administrative tasks, such as Group Policy management, and are frequently accessed via the Tools menu within Server Manager.

**Other essential tools**

**Dynamic Host Configuration Protocol (DHCP)** manager manages the distribution of IP addresses and network configuration settings, helping administrative effort.

**DHCP Scopes** define the range of addresses available for lease on a subnet, while **reservations** ensure that specific devices, such as printers, always receive the same IP address.

**Understanding Windows Registry with Registry Editor**

The Registry Editor is launched from the Start button Run or Command prompt option as **regedit**

The same command can be used to start the Registry Editor in Windows PowerShell

**Registry Contents**

The Registry is hierarchical in structure and are made up of keys, subkeys, and entries

**Registry key** - A category or division of information within the registry

**Registry subkeys** - A single key may contain one or more lower-level keys

**Registry entry** - A data parameter associated with a software or hardware characteristic under a key (or subkey)

Registry **Root** key (can be a shortcut to a subkey)

- **HKEY\_LOCAL\_ MACHINE** - Information about all hardware components, including the CPU, disk drives, network interface cards, optical drives, and more

- **HKEY\_CURRENT\_USER** - Contains information about the desktop setup for the account presently signed into the server console

- **HKEY\_USERS** - Contains profile information for each user who has logged onto the computer

- **HKEY\_CLASSES\_ROOT** - Holds data to associate file extensions with programs

- **HKEY\_CURRENT\_CONFIG** - Has information about the current hardware profile

**System File Checker & Sigverif**

Use the **System File Checker** to scan system files for integrity (Watch Video: [System File Checker](https://sp.video.yuja.com/V/Video?v=17656&node=119261&a=139325478))

You can run this utility to:

- Scan all system files to verify integrity
- Scan and replace files as needed
- Scan only certain files

The System File Checker can be manually run from the Command Prompt window or Windows PowerShell window with admin rights

- sfc /scannow
- sfc /scanfile:filename

**Sigverif** verifies system and critical files to determine if they have a signature (Watch Video: [Sigverif](https://sp.video.yuja.com/V/Video?v=17657&node=119262&a=115967821))

- Only scans files and does not overwrite inappropriate files, enabling you to use the tool while users are logged on to finds a file without a signature that need to be replaced.

**Monitoring and Troubleshooting**

**Task Manager** and **Resource Monitor** are the first tools administrators use to reactively identify rogue processes or hardware resource bottlenecks like high CPU and memory usage.

**Event Viewer** provides a historical roadmap of system activity by logging information, warnings, and errors from various system components and server roles.

**Performance Monitor** tracks detailed system behaviour over time using counters to help establish performance baselines and identify long-term trends.

**Fundamentals of User Account Types**

**User Account Management** is the foundational process of identifying, authenticating, and authorising individuals to access system resources. In Windows environments, accounts are categorised based on whether the system is part of a **Workgroup** or a **Domain**.

**Local User Accounts:** These are stored in a local database called the **Security Accounts Manager (SAM)** on a standalone server or workstation. They provide rights and permissions only to resources on that specific machine.

**Domain User Accounts:** These are managed centrally within **Active Directory (AD)** and can be managed with Active Directory User and Computers console

**Local Account Management and Tools**

For standalone systems, account management is decentralised, meaning each computer maintains its own unique set of users and groups.

**Default Local Accounts:** Following installation, Windows Server includes two primary accounts: the **Administrator** (full rights) and the **Guest** (minimal rights, disabled by default for security).

**Default Local Groups:** Systems include built-in groups like **Administrators**, **Users** (for non-administrative tasks), and **Backup Operators** to simplify permission assignments.

**Management Interfaces:**

- **Local Users and Groups MMC Snap-in:** A graphical tool used to create, rename, or delete accounts and manage group memberships.
- **Settings Menu:** A modern interface under "Accounts" for managing local identities.
- **Windows PowerShell:** Administrators can use cmdlets like New-LocalUser or Get-LocalUser to automate account tasks.

**Best Practice:** It is recommended to rename the default Administrator account and disable it once a new, uniquely named administrative account is created to reduce the system's **attack surface**.

**Active Directory and Organisational Units**

In an enterprise environment, Active Directory provides a **hierarchical structure** to manage thousands of user and computer objects efficiently.

**Organisational Units (OUs):** These act as containers (similar to folders) to group leaf objects for easier administration. OUs allow administrators to **delegate tasks** such as letting a specific team manage only the "Marketing" users and are the primary targets for **Group Policy Objects (GPOs)**.

**The AGDLP Strategy:** Microsoft recommends the **AGDLP approach** for managing permissions: Nest **A**ccounts into **G**lobal groups, then into **D**omain **L**ocal groups, and finally assign **P**ermissions to those local groups.

**Read-Only Domain Controllers (RODCs):** For high-risk areas like branch offices, RODCs can hold a read-only copy of the AD database, only replicating passwords for specific, non-privileged users.

**Active Directory Groups**

For resource permissions management, one of the best ways to manage accounts is by grouping accounts that have similar characteristics

Types of groups: **Domain Local, Global & Universal**

All these groups can be used for security or distribution groups purposes

- Security groups - To enable access to resources on a stand-alone server or in Active Directory
- Distribution groups - For e-mail or telephone lists to provide mass distribution of information

Domain Local security group

- Used when Active Directory is deployed to manage resources in a domain

**Active Directory Groups**

**Global** security group

- Intended to contain user accounts and other global group from a single domain
- Can also be set up as a member of a domain local group in the same or another domain
- A Global group can be converted to a universal group. If it is not nested in another global group or in a universal group

**Universal** security groups

- Provide a means to span domains
- Universal group membership can include user accounts from any domain, global groups from any domain, and other universal groups from any domain

Guidelines to help simplify how you plan to use groups:

- Use global groups to hold accounts as members
- Use domain local groups to provide access to resources in a specific domain
- Use universal groups to provide extensive access to resources

Figure 4-28, Page No. 179, Complexity level: Figure shows the “Nested global groups.” It shows three concentric semicircles. The interior semicircle is labeled “Budget\*\*\*.” The centre circle is labeled “Finance\*\*.” The outer semicircle is labeled “Managers*.”

Figure 4-31, Page No. 182, Complexity level: 2 Figure shows “Managing security through universal and global groups.” The figure shows a circle at the centre overlapping three other circles. The circle at the centre is labeled “UniExec a universal group with access to resources in all three domains.” The three circles are labeled “college.edu” at the left with two printers and a small circle named as GlobalExec global group with an arrow that points to the centre circle. “students.college.edu” at the top and “research.college.edu” at the right with a printer is shown.

**Account Controls**

Effective user management requires strict adherence to security principles to prevent unauthorised access and **privilege escalation**.

**Principle of Least Privilege (PoLP):** This rule dictates that users should only be granted the **minimum set of privileges** necessary to perform their job functions.

**User Account Control (UAC):** A security feature that prompts users for confirmation or administrative credentials before allowing tasks that require **elevated privileges**, stopping malware from making unauthorised changes.

**Password Policies:**

- **Default Domain Policy:** Sets global requirements for password length, complexity, and age for all users.
- **Fine-Grained Password Policies (FGPP):** Using **Password Settings Objects (PSOs)**, administrators can apply stricter requirements (e.g., longer passwords) to specific groups, such as IT staff, without affecting regular users.

**Multi-Factor Authentication (MFA):** Essential for modern security, MFA adds extra validation layers (like a code sent to a mobile device) beyond just a password. According to Microsoft, MFA can prevent over **99.9% of account compromise attacks**.

**File and folder Security**

**File System Support:** Windows Server 2022 primarily uses **NTFS** and **ReFS** for storing data on volumes, both of which support advanced security features like permissions and quotas.

**Folder and File Attributes:** Basic attributes include **read-only** and **hidden**, which are accessible via the General tab of a file's properties. NTFS offers advanced attributes such as **indexing, compression, and encryption (EFS)** to secure or optimize data.

**Access Control Lists (ACLs):** Resource access is managed through two types of ACLs:

- **Discretionary Access Control Lists (DACLs)**, which grant or deny specific permissions to users and groups
- **System Access Control Lists (SACLs)**, which are used for auditing access.

**Configuring Permissions:** Administrators assign basic permissions (**Full control**, **Modify**, **Read** & **execute**, etc.) or advanced permissions via the **Security tab**.

**Inheritance:** By default, permissions set on a parent folder are **inherited** by all files and subfolders contained within it, simplifying management for large directory structures.

**File and folder attributes**

NTFS offers advanced or extended attributes

- Accessed by clicking the General tab’s Advanced button
- The advanced attributes are archive, index, compress, and encrypt

Archive attribute

- Indicates that the folder or file needs to be backed up because it is new or changed

Compress attribute

- A folder and its contents can be stored on the disk in compressed format
- Compression saves space but increase CPU overhead to open the files and to copy them

Encrypt attribute  (Watch Video -  [Encrypt attributes](https://sp.video.yuja.com/V/Video?v=17832&node=119639&a=6959570))

- Protects folders and files so that only the user who encrypts the folder or file can read it
- An encrypted folder or file uses the Microsoft Encrypting File System (EFS)
- EFS uses both symmetric and asymmetric encryption technique
- When you move an encrypted file to another folder on the same computer, that file remains encrypted, even if you rename it

**File and folder permissions**

Permissions - Control access to an object, such as a folder or file

- When you configure a folder so that a domain local group has access to only read the contents of that folder, you are configuring permissions
- At the same time, you are configuring that folder’s discretionary access control list (DACL) of security descriptors

Watch Video: [NTFS Basic Permissions](https://sp.video.yuja.com/V/Video?v=17833&node=119640&a=39983379)

There are option to set up special permissions for a particular group or user

Watch Video: [Advanced NTFS Permissions](https://sp.video.yuja.com/V/Video?v=17834&node=119641&a=146431216)

Figure 5-5, Page No. 203, Complexity level: 2 Screenshot shows “JRyan Properties” dialog box. The Security tab is selected within the dialog box. The Window shows the Object name: C:\\Users\\Administrator\\Documents\\JRyan. The System option is selected under the Group or user names box. The cursor points to the Edit button to change permission. The OK button shown is selected.

Figure 5-8, Page No. 207, Complexity level: 3 Screenshot shows “Permission Entry for UtilitiesJR” dialog box. The window displays three boxes.  The first box shows Principal: Server Operators (JPCOMP\\ Server Operators) select a principal. The Allow option is entered within the Type drop down box. This folder, subfolders and files is entered within the Applies to drop down box. The second box shows the check boxes clicked for Traverse folder/ execute file, Read attributes, Read extended attributes, Create files/ write data, Create folders, append data, Write attributes, Write extended attributes, Delete, Read permission options under the Advanced permissions. Only apply these permissions to objects and/ or containers within this container text is shown below. The third box shows a text that reads “Add a condition to limit access. The principal will be granted the specified permissions only if conditions are met.”

**Sharing Resources and Permission**

**Sharing Protocols:** Windows Server supports **Server Message Block (SMB)**, the default protocol for Windows clients, and **Network File System (NFS)**, which is typically used for UNIX or Linux integration.

**Universal Naming Convention (UNC):** Users access shared resources using the syntax **\\\\servername\\sharedfoldername**.

**Combining Permissions:** When a user accesses a file via a network share, Windows evaluates both **share permissions** and **NTFS permissions**; the **most restrictive** permission always applies.

Watch Video: [Shared Folder Permission vs NTFS Permission](https://sp.video.yuja.com/V/Video?v=17835&node=119643&a=7177977)

Share permissions can be set by configuring sharing using the File Sharing window and a folder’s Sharing tab in its Properties dialog box.

- **Read** - Users can view file and subfolder names, read data in files, and run programs.
- **Change** - Users can do everything allowed by the “Read” permission, as well as add files and subfolders, change data in files, and delete subfolders and files.
- **Full Control** - Users can do everything allowed by the “Read” and “Change” permissions, and they can also change permissions for NTFS files and folders only.
- **Owner** - Automatic assigned to the share owner, with same effective permission as Full Control.

> Reference: [https://blog.netwrix.com/2018/05/03/differences-between-share-and-ntfs-permissions/](https://blog.netwrix.com/2018/05/03/differences-between-share-and-ntfs-permissions/)

**Managing and Troubleshooting Resource Access**

**Group Policy Automation:**

- **Drive Maps:** Group Policy can be used to **automatically map network drives** for users based on their department or organizational unit (OU).

**Troubleshooting Permissions:** When a user is unexpectedly denied access, administrators should use the **Effective Access tab** in Advanced Security Settings to calculate the user's actual permissions after accounting for **group memberships and inheritance**.

**Network Connectivity Tools:**

- **Test-NetConnection:** A powerful PowerShell cmdlet that can test connectivity to specific ports (e.g., SMB port 445) to ensure firewalls are not blocking access.

**Common Issues:** Access problems are often caused by **conflicting group memberships** (where a "Deny" permission overrides an "Allow") or **misconfigured DNS** resource records that prevent clients from locating the file server.

**Overview of Storage Devices and Partitioning Strategies**

**Storage Device Types:** Windows Server 2022 supports both traditional **hard disks** (cost-effective for large data) and **Solid State Disks (SSDs)** (fast transfer speeds for OS and applications).

**Partitioning Schemes:**

- **Master Boot Record (MBR):** Older standard; supports up to four primary partitions and volumes up to 2 TB.
- **GUID Partition Table (GPT):** Modern standard; required for disks larger than 2 TB and supports up to 128 partitions.

**RAID Strategies:** Common software RAID levels include **RAID 0** (striping for speed, no fault tolerance), **RAID 1** (mirroring for fault tolerance), and **RAID 5** (striping with parity for efficiency and fault tolerance).

**Data Storage and Resource Management**

**Disk Management** and the storage tools in Server Manager allow administrators to initialize disks, create partitions, and manage volumes using file systems like **NTFS** or **ReFS**.

**Storage Spaces Direct** allows for the creation of fault-tolerant RAID volumes by pooling local storage from multiple servers, enhancing both performance and data reliability.

**File Server Resource Manager (FSRM):** This role provides advanced management tools for file servers.

**Optimization, Deduplication, and Backup**

**Data Deduplication:** The Data Deduplication Service scans volumes for duplicate file content, storing only a single copy to save significant space. It is supported on both NTFS and ReFS volumes.

**Volume Maintenance:**

- **Optimizing:** Windows automatically defragments (hard disks) or trims (SSDs) volumes on a weekly basis to maintain speed.
- **Repairing:** Tools like **chkdsk** or the **Repair-Volume** PowerShell cmdlet can scan for and fix file system errors or bad sectors.

**Core Fundamentals of Administration Efficiency**

**Cmdlet Structure:** PowerShell uses an intuitive **action-object (verb-noun)** structure for its commands, known as cmdlets, which makes them easy to learn and recall.

**The Pipeline (|):** This powerful feature allows you to **chain commands together** by sending the output of one cmdlet directly into the next, enabling complex multi-step tasks to be completed in a single line of code.

**Tab Completion:** To increase typing speed and reduce errors, administrators can use the **Tab key to auto-complete** cmdlets and parameters as they type.

**Standardized Management:** Because many graphical management tools in Windows Server actually run PowerShell cmdlets in the background, mastering the command line provides a more direct and faster way to perform the same configuration work.

**Reusability and Speed Through Scripting**

**Task Automation:** PowerShell scripts are text files with a **.ps1 extension** that can perform a series of administrative tasks automatically.

**Reusable Logic:** Scripts can be written to accept **input arguments**, allowing the same block of code to be reused for different scenarios or server environments without modification.

**Logic and Loops:** Using constructs like the foreach loop, an administrator can efficiently **enumerate through large datasets**, such as a list of users or servers, to perform identical actions on every item.

**PowerShell Gallery:** Administrators can save significant time by downloading, repurposing, and modifying **existing scripts** from Microsoft and the IT community rather than starting from scratch.

**Powershell in action**

Creating a new user in Active Directory

Create a hash table for general user attributes

- $PW = 'Pa$$w0rd'
- $PSS = ConvertTo-SecureString -String $PW -AsPlainText -Force
- $NewUserHT = @{}
- $NewUserHT.AccountPassword = $PSS
- $NewUserHT.Enabled = $true
- $NewUserHT.PasswordNeverExpires = $true
- $NewUserHT.ChangePasswordAtLogon = $false

Create the new user

- $NewUserHT.SamAccountName = ‘TanCC'
- $NewUserHT.UserPrincipalName = ‘tancc@domainname'
- $NewUserHT.Name = ‘TanCC'
- $NewUserHT.DisplayName = ‘Tan Ching Chuan (IT)'
- New-ADUser @NewUserHT
