## **ST2612 – Quick Module Overview**

- Lesson Plan
- 15 weeks
- Teaching Modes/Methods
- Online (A Sync) Lecture (1 hour)
- Flipped Learning
- Assessment
- Assignments, EST & General  Performance
- Onsite Tutorial (2 hours)
- Onsite Practical (2 hours)

**Teaching Modes**

Lectures material mostly referred on

**Reference**

- Jason W. Eckert, Cengage Learning, Hands-On Microsoft Windows Server 2019 (3ed) 2019
- Jason W. Eckert, Cengage Learning, Hands-On Microsoft Windows Server 2022 (4ed) 2026

**Lab Practical**

- Maintain your own set of VMware based systems :

  - Microsoft Server 2022 / Server 2019 and Microsoft Windows 10

**Teaching Modes**

**Self-Directed Learning**\
Lecture slides are designed for independent study, supported by online resources and self-assessment questions.

- Readings and online research (e.g., YouTube, Google)
- MCQs from the slides and Brightspace
- Case studies and analysis

**Flipped Learning**

- All topics will be delivered in flipped learning mode.

**Tutorials**

- Onsite sessions can have practical-tutorial
- Q&A on lectures and practical learning issues

## **Assessment Components**

- **CA1: Assignment (Term 1)**

  - 30% Group Assignment (Report and Demo)

- **CA2: Assignment (Term 2)**

  - 20% Group Assignment (Report)

- **EST: (EST Week)**

  - 30% (Open-book Theory & Practical Test)

- **CA9: General Performance**

  - 10% General Performance
  - 5% Online Quiz 1 (Term 1)
  - 5% Online Quiz 2 (Term 2)

Periodic table of elements

## **ST2612 – Secure Microsoft Windows**

***Lecture 1***

*Introduction to Windows Server 2022 AND configurations*

*OVERVIEW OF THE WINDOWS ACTIVE DIRECTORY CONCEPT AND GROUP POLICY MANAGEMENT*

Programming data on computer monitor

**Learning OBJECTIVE – PART 1**

1. **Explain the basics of Window Server Operating System**

2. **Describe the various Platforms, Core Technologies, and Features of Windows Server Operating System**

3. **Describe the configuration procedures of Windows Server Operating System**

4. **Describe how to use Server Manager to install and remove Server Roles**

**Basics of Window Server Operating System**

**Windows Server 2022** is Microsoft's flagship server operating system, built on the reliable Windows 10 code base.

It serves as a **foundational platform** for organizations of all sizes to house, protect, and serve data to client.

**Over 70%** of organizational servers worldwide run a version of the Windows Server operating system.

**Key Value**: It provides a comprehensive set of services designed to increase user productivity and data security across various environments.

**Basics of Window Server Operating System**

**Server Roles**: Defines the primary purpose of a server, such as a Domain Controller, Web Server, or DHCP Server.

**Client**: A computer that accesses resources on another computer or server via a network

**Features**: Supporting software components that complement roles, such as Failover Clustering, Group Policy Management, or BitLocker.

**Core Roles commonly used in infrastructure**:

- **Active Directory Domain Services (ADDS)**: Centralizes authentication and identifies all network objects.
- **Domain Name Services (DNS)**: Resolves hostnames to IP addresses.
- **Dynamic Host Configuration Protocol (DHCP)**: Provides IP configuration to clients.

**Various Platforms and Features**

**Standard Edition**: Designed for physical or minimally virtualized environments, supporting up to two virtual machines (VMs).

**Datacenter Edition**: Ideal for highly virtualized data centers and mission-critical applications, supporting unlimited VMs.

**Essentials Edition**: A cost-effective solution tailored for small businesses with up to 25 users and 50 devices.

**Datacenter: Azure Edition**: A specialized cloud-based version that enables exclusive features like "hot-patching".

**Licensing Model**: For most editions, Microsoft utilizes per-processor core licensing, often requiring Client Access Licenses (CALs) for user or device connectivity.

> For more reading link:
>
> https://learn.microsoft.com/en-us/windows-server/get-started/editions-comparison?pivots=windows-server-2022
>
> https://learn.microsoft.com/en-us/windows-server/get-started/editions-comparison?pivots=windows-server-2019
>
> https://learn.microsoft.com/en-us/windows-server/get-started/locks-limits?tabs=full-comparison&pivots=windows-server-2022
>
> https://learn.microsoft.com/en-us/windows-server/get-started/locks-limits?tabs=full-comparison&pivots=windows-server-2019

**Various Platforms and Features**

**Desktop Experience**: The standard graphical user interface (GUI) familiar to users of Windows 10; it is the primary choice for those new to server administration.

**Server Core**: A headless, command-line-driven installation option that removes most graphical frameworks. It provides enhanced security, a smaller footprint, and requires fewer system resources for performance.

**Nano Server**: An ultra-small footprint image designed exclusively as a base for containerized web applications.

**Installation Note**: In Windows Server 2022, administrators cannot switch between the Desktop Experience and Server Core modes once the operating system is installed.

**Core Technologies**

**Active Directory Domain Services (ADDS)**: A central repository that identifies all network objects and provides centralized authentication and single sign-on capabilities.

**Hyper-V Virtualization**: A Type 1 hypervisor that allows organizations to consolidate multiple guest operating systems onto a single physical server to improve resource efficiency.

**Advanced Storage**: Includes features like Storage Spaces Direct, which combines disks across multiple servers into one resilient volume, and Storage Replica for data protection.

**Secured-core Server**: Leverages specialized hardware and firmware protections, including TPM (Trusted Platform Module) 2.0 and UEFI (Unified Extensible Firmware Interface) Secure Boot, to guard against boot-time attacks.

**Advanced Encryption**: Features TLS 1.3 enabled by default and support for DNS-over-HTTPS (DoH) for more secure network activity.

**SMB Improvements**: Supports SMB over QUIC (Quick UDP Internet Connections) for secure mobile access and AES-256 encryption for the highest file transfer security.

> For more reading link:
>
> https://learn.microsoft.com/en-us/windows-server/get-started/whats-new-in-windows-server-2022

**Management Interfaces**

**Server Manager**: The primary GUI dashboard for managing local and remote servers and their respective roles.

On **Server Core** installations, the **sconfig** utility provides a menu-driven interface for common tasks such as configuring IP settings, changing the computer name, and managing updates.

**Essential Administration Tools**

The **Windows Admin Center (WAC)** offers a modern, web-based management platform that allows administrators to configure both local and remote servers through a single browser-based console.

**Remote Server Administration Tools (RSAT)**: A set of tools that allows administrators to manage servers from their personal Windows 10/11 workstations.

**Windows PowerShell**: A powerful command-line shell and scripting language used for advanced administration, automation and remote management.

> For more reading link:
>
> https://learn.microsoft.com/en-us/windows-server/manage/windows-admin-center/overview

**Summary of enhancement**

**Active Directory**: Windows Server 2022 enhances integration with **Azure AD**, supports **Domain Controller Cloning**, and adds better **Kerberos authentication**. These features, along with the **Secured-Core Server** initiative, offer higher levels of security and ease of management.

**DNS**: Windows Server 2022 builds on DNS security, including enhanced **DNS-over-HTTPS** and **DNS Cache Locking**. It also improves logging, scalability, and performance.

**DHCP**: Windows Server 2022 enhances **DHCP failover**, load balancing, and **IPAM integration**, making it a more reliable and scalable solution for managing IP addresses.

Windows Server 2022 offer significant improvements in **Active Directory**, **DNS**, and **DHCP**, with a stronger stance on security, hybrid cloud integrations, and scalability to better support enterprise environments.

**Configuration procedures**

Following the installation of Windows Server 2022, several essential post-installation tasks must be performed to prepare the system for its organizational role.

- Administrators must configure the **correct time and time zone** to prevent issues with network authentication and logging.

- The default computer name, which is randomly generated during installation, **should be changed** to follow the organization’s naming convention, a procedure that typically requires a system restart.

**Configuration procedures**

Configuring network interfaces is a critical procedure, as servers usually **require a static IP address** to ensure that client requests and network services remain reliable.

Through the **Server Manager Properties pane or PowerShell cmdlets**, administrators assign parameters including the subnet mask, default gateway, and DNS server addresses.

The **Windows Firewall** is enabled by default to block unauthorized access, so administrators must manually allow specific ports and protocols required by the server's roles.

**Use Server Manager**

**Server Manager** is the primary tool that launches by default upon administrator login, serving as a central hub for configuring and managing server roles and features.

Administrators can initiate the **Add Roles and Features Wizard** through several paths: On the Dashboard's Quick Start tile, selecting the Manage menu in the top bar, or using the Tasks menu within specific server panes.

A single server can host multiple roles simultaneously, such as acting as a domain controller, file server, and DNS server at the same time.

**The Installation Wizard Process**

The first decision in the wizard is selecting the **Installation Type**: most standard setups use "Role-based or feature-based installation”.

On the **Server Selection** page, you must choose a destination; this can be the local server, a managed remote server in your pool, or even an offline virtual hard disk (.vhdx).

When navigating the **Server Roles** list, checking a role may trigger a pop-up window informing you that additional required features must also be installed to ensure proper functionality.

Highlighting any role or feature provides a brief description of its purpose to help ensure the correct components are being selected.

**Finalizing Installation and Post-Deployment**

The **Confirmation** page allows you to review your selections and choose to "Restart the destination server automatically if required," which is necessary for certain roles to complete their setup.

After clicking **Install**, you can close the wizard and monitor the progress via the task details or the notification flag in the Server Manager header.

Once configured, new roles will appear in the Server Manager navigation pane, allowing for direct management and health monitoring.

**Removing Server Roles and Features**

To uninstall components, administrators must access the **Remove Roles and Features Wizard**, which is found under the Manage menu.

**The process mirrors installation**: you select the target server and then deselect the checkbox for the role or feature you wish to remove.

Following removal, the server typically requires a restart to finalize the changes, after which it will function as a basic member server without that specific role's capabilities.

**Learning objective – PART 2**

5. **Explain the basics of Active Directory and Group Policy management**

6. **Describe the Role of a Directory Service**

7. **Explain the Security concepts of Active Directory Design**

8. **Explain the procedures of configuring and maintain the Active Directory Infrastructure**

9. **Explain the concept and configuration procedures of Group Policy**

**Introduction to Active Directory (AD)**

Watch Video: [Active Directory](https://sp.video.yuja.com/V/Video?v=17659&node=119282&a=52043841)

**Active Directory Domain Services (ADDS)** is a **directory service** that serves as a central repository for an organization's information, including user accounts, computer accounts, and security policies.

The primary role of a server hosting AD is a **Domain Controller (DC)**, which acts as a central "hub" for authentication and identification on the network. Contain writable copies of information in Active Directory

A key benefit of AD is **Single Sign-On (SSO)**, which allows users to authenticate once to a domain controller and automatically prove their identity to other servers and clients on the network.

In addition to authentication, AD provides components to centrally manage and secure all domain-joined computers, reducing redundant administrative effort.

**Logical Component and Objects of Active Directory**

Active Directory uses a hierarchical structure consisting of **forests**, **domains**, and **organizational units** (OUs).

A **Forest** is the top-level container consists of one or more Active Directory domain that are in a common relationship.

A **Domain** is a logical grouping of objects (users, computers, groups) that share a central database and common security policies.

**Organizational Units** (OUs) are container objects within a domain used to group like objects for ease of management and to delegate administrative permissions.

**Global Catalog** (GC): A service that stores a partial replica of objects from every domain in a forest to enable fast searching and successful logons across multiple domains.

Watch Video: [Global Catalog](https://sp.video.yuja.com/V/Video?v=17660&node=119283&a=44485162)

The Active Directory **Schema** acts as a dictionary, defining every object class and attribute that can be used within the database.

**Managing AD Objects: Users, Groups and Computers**

All items in the AD database, such as user accounts and printers are stored as **objects** that conform to specific standards.

**User** objects allow employees to log into any domain-joined computer with their specific credentials and access granted resources.

**Security Groups** (Universal, Global, and Domain Local) are used to collect users and computers into a single unit to simplify the assignment of rights and permissions.

**Computer** objects represent domain-joined hosts that establish a secure channel with a domain controller to authenticate user logins.

**Introduction to Group Policy Management**

**Group Policy** is a powerful engine used in AD environments to centrally manage and automate settings for users and workstations.

Administrators create **Group Policy Objects** (GPOs), which are packages containing settings like password requirements, desktop lockdowns, or software distribution.

Settings are issued automatically to any computer or user simply by being joined to the domain and located in the correct target area of AD.

The **Group Policy Management Console** (GPMC) is the primary tool used to create, edit, delete, and link GPOs.

**GPO Sections and Processing Order**

Each GPO contains two main sections: **Computer Configuration** (applied to the machine at boot) and **User Configuration** (applied to the user at login).

GPOs are processed in a specific hierarchy known as **LSDOU**: **Local**, **Site**, **Domain**, and then **Organizational Unit**.

A "last applied wins" rule exists where settings in a higher-level container (like an OU) override conflicting settings from a lower-level container (like a Domain).

By default, Group Policy background refreshes happen every **90 minutes** plus a random offset, ensuring new settings roll out without requiring a reboot.

**Advanced Management and Troubleshooting**

Administrators can use the **gpupdate /force** command to bypass the refresh cycle and apply new policy changes immediately on a client machine.

To troubleshoot which policies are reaching a machine, the **gpresult /r** or **Resultant Set of Policy** (RSoP) tool identifies exactly which GPOs were applied or filtered out.

**WMI Filtering** and **Item-level** targeting allow administrators to further narrow GPO application based on specific hardware attributes (like RAM) or security group memberships.

**Purpose of a Directory Service**

**Centralized Repository**: A directory service serves as a structured database that stores and manages information about network resources, such as user accounts, computer accounts, and security groups.

**The "Central Source of Truth":** It provides a single point of contact for authentication and identification on the network, replacing the decentralized management found in workgroup environments.

**Unified Identity**: When a user logs into a domain-joined computer, their credentials are validated against the central database on a special server called a domain controller.

**Single Sign-On (SSO):** This is a primary benefit where users authenticate only once to a domain controller to automatically prove their identity to all other domain-joined servers and clients on the network.

**Access Control**: The directory service works with Access Control Lists (ACLs) to grant or deny access to resources based on the permissions assigned to the user's specific identity.

**Streamlining Administrative Effort**

**Reduced Redundancy**: Directory services automate and centralize security controls, significantly reducing the redundant administrative effort required to manage individual computers.

**Group Policy Integration**: ADDS includes a powerful engine (Group Policy) used to automatically deploy software and centrally configure security, operating system, and application settings across thousands of computers.

**Delegated Administration**: Using Organizational Units (OUs), administrators can delegate specific management tasks (like resetting passwords) to junior staff or specific departments without granting full administrative access.

**Scalability**: A directory service can contain an unlimited number of objects, allowing it to support organizations ranging from small businesses to global enterprises with hundreds of thousands of users.

**Structural Security Boundaries (Forests and Domains)**

An Active Directory **forest** represents the top-most container and serves as the absolute security boundary for an entire organization.

A **domain** acts as a logical grouping of objects that share a common security database and centralized security policies.

**Trust relationships** allow users in one domain to access resources in another; these can be configured as one-way or two-way and may be transitive or nontransitive depending on design needs.

**Access Control and Identity Identification**

Every **user**, **group**, and **computer** in Active Directory is assigned a **Security Identifier** (SID), a unique value that persists even if the object is renamed.

When a user logs in, the system generates a **Security Access Token** (SAT) containing their SID and all group SIDs, which follows their session to determine resource access.

**Discretionary Access Control Lists** (DACLs) identify which security principals (users or groups) are allowed or denied specific actions on an object.

**Hardening Domain Controllers and Authentication**

**Domain Controllers (DCs)** are critical assets that should be physically secured and only run essential roles like ADDS and DNS to minimize their attack surface.

**Kerberos** is the default ticket-based authentication protocol for ADDS, providing mutual authentication and protection against eavesdropping and relay attacks.

**Read-Only Domain Controllers (RODCs)** are used in branch offices where physical security is limited; they store a read-only database and only cache passwords for specified users to mitigate the risk of server theft.

**Policy Enforcement and Modern Security Tools**

**Group Policy** is a powerful engine used to automatically enforce security settings, such as password requirements and workstation lockdowns, across all domain-joined devices.

The **Active Directory Recycle Bin** allows for the safe restoration of accidentally deleted objects without requiring a full system state restore, maintaining service availability.

**Windows Defender for Identity (MDI)** monitors network signals to detect advanced threats like Golden Ticket or Pass-the-Hash attacks in real time.

**Credential Guard** uses virtualization-based security to isolate secrets in memory, preventing hacking tools from extracting NTLM or Kerberos credentials.

**Installation and Infrastructure Deployment**

**Prerequisites**: Before configuring a server as a Domain Controller (DC), it is critical to assign a static IP address, configure DNS settings to point to a valid server (often itself), and ensure the hostname is finalized, as it cannot be easily changed after promotion.

**The ADDS Role**: Administrators use the Add Roles and Features Wizard to install the Active Directory Domain Services (ADDS) role and its associated management tools.

**Promotion Options**: Through the configuration wizard, a server can be promoted as the first DC in a new forest, a new domain within an existing forest, or an additional DC in an existing domain for redundancy.

**Critical Settings**: During deployment, you must define forest and domain functional levels and specify a Directory Services Restore Mode (DSRM) password, which is vital for database repair and recovery.

**Configuring Core Infrastructure Components**

**Raising Functional Levels**: Administrators can increase functional levels to enable newer AD features (such as the Recycle Bin); however, this is a one-way operation that cannot be reversed.

Optional Reading: [Active Directory Functional Level](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/active-directory-functional-levels)

**Sites and Subnets**: To control Domain Controller replication traffic across physical locations, administrators create Site objects in the Active Directory Sites and Services tool and associate them with specific IP subnets.

**Replication Management**: Site Links are configured to restrict replication between sites to specific times or frequencies, balancing data consistency with geographical bandwidth costs.

**Global Catalog (GC):** The GC is a partial replica of all objects in the forest, enabling forest-wide searches and logons; it is typically enabled on DCs during the promotion process.

**Ongoing Maintenance and Disaster Recovery**

**System Health Checks**: It is a best practice to run the **Best Practices Analyzer** (BPA) periodically to scan for configuration issues and ensure the infrastructure follows Microsoft’s recommended guidelines.

The **AD Recycle Bin**: Once enabled via the **Active Directory Administrative Center** (ADAC), this feature allows administrators to restore deleted objects (like user accounts) with their original configuration intact and without downtime.

**Backup Procedures**: Regular system state backups must be performed, as they contain the **Active Directory database** (ntds.dit) and the Windows Registry.

**Important feature in Active Directory**

Important features deserve mentioning:

**Restart capability**

- Provides the option to stop, start or restart

Active Directory Domain Services without

shutting down the server.

**Read-Only Domain Controller**

- Blocks local changes as it is "read-only"
- Safer for branch offices that don't have IT staff or

high-security server rooms

**Cloning domain controllers**

- Ability to clone domain controllers
- For easier deployment of multiple domain controllers in an environment with multiple virtual machines

**Fine-grained password policy**

- Different security groups can have different password policies
- Ability to use the Active Directory Administrative Center for managing password setting objects in the Active Directory schema

**Understanding Group Policy Concepts**

The **Group Policy Object (GPO):** A GPO is a single package or "named object" that contains a collection of configuration settings applied to domain computers and users.

**Automatic Enforcement**: Settings are issued automatically to any system simply by being joined to the domain, meaning administrators do not need to touch individual client systems to push configurations.

**Scope and Scalability**: A single GPO can be applied to thousands of objects simultaneously, significantly reducing the redundant administrative effort required to manage large networks.

**Configuration Procedures**

The Management Tools: Administrators primarily use the Group Policy Management Console (GPMC) to create and link GPOs, and the Group Policy Management Editor to modify specific settings.

**Step 1**: Creating a GPO: Within GPMC, right-click the "Group Policy Objects" folder and select "New" to create a shell that contains no initial settings.

**Step 2**: Configuring Settings: Right-click the new GPO and select "Edit" to open the Editor; from here, administrators can navigate through over 5,000 settings across Software, Windows, and Administrative Templates.

**Step 3**: Linking to Containers: To activate the settings, the GPO must be linked to an Active Directory container, such as a Site, Domain, or specific Organizational Unit.

**Advanced Scoping**

**Block Inheritance**: This setting is applied to an Organizational Unit (OU) or a domain to prevent it from applying GPOs linked to parent containers, such as sites or higher-level OUs

**Enforced**: If a GPO link is set to Enforced, it bypasses any "Block Inheritance" settings configured on child OUs. An enforced GPO is applied last, which ensures its settings override any conflicting configurations in other GPOs
