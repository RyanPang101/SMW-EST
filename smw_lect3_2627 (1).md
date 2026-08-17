Computer script on a screen

## **ST2612 – Secure Microsoft Windows**

***Lecture 3***

*INTRODUCTION TO WINDOWS SERVER SECURITY*

*PLANNING, CONFIGURING AND DEPLOYING SECURITY POLICIES*

**Recap**

User Account Management

- Local vs domain

Security Group Management

- One of the best ways to manage accounts is by grouping accounts that have similar characteristics
- For resources access and permission management

Managing Folder and File Security

- Discretionary ACL (DACL) – File Permission
- Shared Folder Security (supports Non-NTFS Volume)
- EFS (Encrypting File System)

**Learning Objectives – Part 1**

1. **Explain the need and security features of Windows Server Operating System**

2. **Explain Security Enhancements of Windows Server Operating System**

3. **Secure Windows Server Operating System using Security Policies**

4. **Establish Account Policy and Security Options**

**Identifying the Need for a Server Operating System**

**Resource Sharing**: Specialized server operating systems are required to allow organizations to efficiently **share resources** such as files, databases, and printers across a network.

**Centralised Management**: Windows Server provides **centralised management** for users, applications, and network resources, which is essential for maintaining consistency in large organisations.

**Identity and Access Security Features**

**Active Directory (AD)**: AD is a core security feature that provides **Single Sign-On (SSO)**, allowing users to authenticate once and access all authorized resources in a domain.

**Group Policy**: This feature allows administrators to **automatically deploy security settings**, software, and operating system configurations to thousands of computers from a central location.

**Principle of Least Privilege**: Security is enhanced through **Role-Based Access Control (RBAC)** and **Just Enough Administration (JEA)**, which ensure users have only the permissions necessary to perform their jobs.

**Discretionary Access Control Lists (DACLs)**: Windows Server uses DACLs to grant or deny specific permissions for files and folders based on user identity.

**Event Auditing**: System Access Control Lists (SACLs) allow administrators to **track and log access** to sensitive resources for security monitoring.

**Advanced Threat and Data Protection**

**Microsoft Defender**: This built-in service provides **real-time protection** against malware and automatically stops dangerous processes.

**Credential Guard**: Using **virtualization-based security**, Windows Server isolates secrets like Kerberos tickets in a protected process to prevent credential theft attacks.

**BitLocker Drive Encryption**: This feature protects data at rest by **encrypting entire volumes**, ensuring data remains unreadable if physical disks are stolen.

**Hardware and Network-Level Security**

**Secured-core Server**: This feature utilizes specialized hardware to protect against **firmware-level attacks** and ensure the system boots with legitimate code.

**Default Security Protocols**: Modern versions like Windows Server 2022 enable **HTTPS and TLS 1.3 by default**, ensuring that all network connections use strong encryption.

**DNS-over-HTTPS (DoH)**: This protocol encrypts DNS lookups, protecting them from eavesdropping and **man-in-the-middle attacks**.

**SMB Encryption**: The Server Message Block protocol now supports **AES-256 encryption**, providing maximum security for file transfers across a network.

**Secure Remote Access**: Tools like **Always On VPN and DirectAccess** provide seamless, encrypted tunnels for remote workers to connect to corporate resources safely.

**Overview of Security Features in Windows Server 2022**

**Security features commonly used:**

- Expanded and evolving Group Policies
- Windows Firewall
- Windows Defender
- User Account Control
- BitLocker Drive Encryption

**Group policy** is a way to bring consistent security and other management to Windows Server 2022 and to clients connecting to a server

In **Windows Firewall and IPsec, s**ettings are merged for consistency

**Windows Defender** is a software that scans for viruses, spyware, and malware

- Windows Server 2016 is the first Windows Server OS to include Windows Defender

**Overview of Security Features in Windows Server 2022**

User Account Control (UAC)

- Designed to keep the user running in the standard user mode to:

  - More fully insulate the kernel

  - Keep operating system and desktop files stabilized

  - Another element of UAC is the Administrator Approval Mode

    - Prevents malware or an intruder from acquiring control through a back door without the administrator knowing

BitLocker Drive Encryption

- Prevents an intruder from bypassing ACL file and folder protections incase hard disk is stolen

**Fundamentals of Windows Server Security Policies**

Security policies in Windows Server are primarily managed through **Group Policy**, a component of Active Directory that provides centralised management of Windows computers within a domain.

**Group Policy Objects (GPOs)**: These are named objects containing collections of rules that govern the operating system. They allow administrators to define configuration changes to multiple computers without editing each user's individual settings.

**Default Policies**: Every Active Directory domain contains default GPOs, such as the **Default Domain Policy**, which applies to all users and computers in the domain, and the **Default Domain Controllers Policy**, linked to the Domain Controllers OU.

**Enforcement Levels**: GPOs can be linked at different levels: **Local, Site, Domain, and Organizational Unit (OU)**. Settings at a higher level (like an OU) typically override lower-level settings.

**GPO Structure**: Policy settings are organised into two main sections: **Computer Configuration** (applied at boot time) and **User Configuration** (applied at login).

**Windows Server Security Policies**

Computer Configuration settings applies to computers

User Configuration settings applies to users

**Securing Identities and Access**

**Security Filtering**: This allows administrators to limit the scope of a GPO to specific users, computers, or security groups rather than everyone in a linked OU.

**User Rights Assignment**: These policies define how a user or service can interact with the server locally, such as who can **log on locally**, **shut down the system**, or **change the system time**.

**Restricted Groups**: This policy allows administrators to centrally govern membership in built-in local groups, such as the **Administrators** group, ensuring only authorised accounts have elevated rights.

**Security Options**: This section offers granular settings for security-related actions, such as requiring a **smart card** for login or preventing the display of the last signed-in username on the logon screen.

**Auditing and Tracking Security Events**

**The Audit Policy**: This defines which events Windows should record in its security logs. Key categories include **logon events**, **object access** (files and folders), and **policy changes**.

**System Access Control Lists (SACLs)**: To audit resource access, a SACL must be configured for a specific file or folder. It determines what type of event (Success, Failure, or both) should generate an entry in the **Security log**.

**Object Access Auditing**: This allows organisations to track activity on sensitive data, such as recording each time a file in a financial folder is modified.

**Event Logging Policies**: Administrators can use GPOs to define the maximum size and retention settings for event logs, ensuring critical security data isn't overwritten too quickly.

**Expression-Based Audit Policies**: In newer versions of Windows Server, administrators can create targeted audit policies using expressions based on user, computer, and resources (e.g., auditing access only for contractors).

**Implementing Security Baselines and Best Practices**

**Security Baselining**: This is the practice of implementing a minimum set of standard configurations to ensure systems are deployed in a consistently secure state.

**Microsoft Security Compliance Toolkit (SCT)**: This set of tools provides recommended GPOs and configuration baselines for different versions of Windows Server, which can be acquired and deployed to adhere to best practices.

**CIS Benchmarks**: Many organisations use third-party frameworks like the **Center for Internet Security (CIS)** benchmarks to complement Microsoft’s baselines with additional hardening recommendations.

**Policy Analyzer**: This tool within the SCT allows administrators to compare their current GPO settings against recommended baselines to identify security gaps or conflicts.

**Fundamentals of Windows Server Account Policies**

**Three Pillars of Account Policies**: These policies are organized into three specific subfolders within the Security Settings of a GPO:

- **Password Policy**: Dictates requirements for password strength and lifecycle.
- **Account Lockout Policy**: Determines how the system responds to multiple failed login attempts.
- **Kerberos Policy**: Sets limits for ticket lifetimes and clock synchronisation within the domain.

**The Default Domain Policy**: In a fresh AD environment, global account policies are typically established in the **Default Domain Policy**, which applies to all users and computers in the domain.

**Implementing Robust Password Policies**

**Enforcing Password Strength**: Administrators configure specific settings to ensure security, such as **Minimum password length** (recommended at 14 characters for modern environments) and **Password must meet complexity requirements**.

**Managing Password Lifecycle**:

- **Maximum password age**: Sets how many days a password is valid before a reset is required (e.g., 42 or 60 days).
- **Minimum password age**: Prevents users from immediately changing passwords back to an old one.

**Password History**: The **Enforce password history** setting determines how many unique passwords must be used before an old one can be reused (typically up to 24).

**Modern Guidance**: Recent research from **NIST and Microsoft** suggests moving toward **longer passphrases** and removing periodic reset requirements unless a compromise is suspected.

**Account Lockout and Brute-Force Protection**

**Lockout Threshold**: This defines the number of **failed login attempts** permitted before an account is disabled. A common best practice is five attempts.

**Lockout Duration**: Specifies how long the account remains locked. Setting this to **0** requires an administrator to **manually unlock** the account, while a numeric value (e.g., 15 or 30 minutes) allows for automatic unlocking to reduce help desk calls.

**Reset Counter**: The **Reset account lockout counter after** setting determines the timeframe in which the failed attempts must occur to trigger a lockout; this should generally align with the lockout duration.

**Mitigating Denial of Service (DoS)**: Administrators must balance strict thresholds with the risk of an attacker intentionally locking out legitimate users across the domain.

**Account Lockout and Brute-Force Protection**

Kerberos security

- Involves the use of tickets that are exchanged between the client who requests logon and network services access and the server or Active Directory that grants access
- Once a user is authenticated, the Kerberos ticket-granting service grants a permanent ticket (called a service ticket) to that computer
- A service ticket is good for the duration of the logon session
- Use of Advanced Encryption Standard (AES) encryption
- When Active Directory is installed, the account policies enable Kerberos
- When Active Directory is not installed, the default authentication is through Windows NT LAN Manager version 2 (NTLMv2)

**Fine-Grained Password Policies (FGPP)**

**Beyond the Default Policy**: While the Default Domain Policy applies one set of rules to everyone, **Fine-Grained Password Policies** allow for different requirements for different groups of users.

**Use Cases**: FGPP is ideal for enforcing **stricter requirements for privileged accounts** (like Domain Admins) while allowing more lenient settings for service accounts or shared "shop floor" computers.

**Password Settings Objects (PSOs)**: These policies are created as PSOs within the **Active Directory Administrative Center (ADAC)** rather than through traditional GPOs.

**Precedence**: Since a user might be a member of multiple groups with different PSOs, each policy is assigned a **precedence number**; the policy with the lowest number (highest precedence) is applied.

**Learning Objectives – part 2**

5. **Analyze current security settings by using various methods**

6. **Apply Security Policies in Windows Systems**

7. **Configure Client Security Using Policies**

8. **Manage Security using Security Templates**

9. **Deploy security templates using various methods**

**Foundations of Security Profiling and Baselines**

**Defining the Baseline**: Analyzing security begins with creating a **baseline**, which is a collection of configuration settings representing a known "good" or secure state.

**The Profiling Process**: Profiling involves comparing the current configuration of a computer against these established secure settings to identify differences and vulnerabilities.

**Snapshots Over Time**: Profiling allows administrators to compare snapshots of a system at different times, helping to detect unauthorized changes that may indicate a breach.

**Microsoft Security Compliance Toolkit (SCT)**: This toolkit provides recommended security baselines for various Windows versions, allowing administrators to download and test their systems against Microsoft’s best practices.

**Microsoft Security Compliance Toolkit (SCT)**

The **Microsoft Security Compliance Toolkit (SCT)** is a comprehensive set of tools that allows enterprise security administrators to download, analyze, test, edit, and store Microsoft-recommended security configuration baselines for Windows and other products. It serves as a foundational resource for hardening systems and ensuring they are deployed in a consistent, desired state as part of a Zero Trust security strategy.

The toolkit includes several specialized utilities to assist in policy management:

- **Policy Analyzer**: A utility used to compare Group Policy Objects (GPOs) against other GPOs, local policy settings, and the local registry. It is particularly effective at identifying conflicts and differences between recommended baselines and current system settings.
- **Local Group Policy Object (LGPO) Tool**: A command-line utility used to manage local group policies, allowing administrators to apply computer-based settings even on systems that are not part of an Active Directory domain.
- **Security Baselines**: Microsoft-recommended configuration settings designed to reduce the attack surface and improve the security posture of various platforms

Core Use Cases

**System Hardening**: Implementing a minimum set of mandatory standards to ensure consistency and security across thousands of devices.

**Policy Comparison and Auditing**: Using Policy Analyzer to find 128+ combined differences or conflicts in a standard Windows installation compared to Microsoft's recommendations

**Configuring User Rights**

Two general categories of user rights:

- Privileges – generally relate to the ability to manage server or Active Directory functions
- Logon rights – are related to how accounts, computers, and services are accessed

Figure 10-9, Page No. 436, Complexity level: 2 Screenshot shows the “Group Policy Management Editor” dialog box. On the left pane of the dialog box, the User Rights Assignment option is selected within the Local Policies tree under the Computer Configuration. The cursor points to the scroll bar of Policy and Policy Settings in the content pane.

Examples of privileges:

- Add workstations to domain
- Back up files and directories
- Change the system time
- Shut down the system

Examples of logon rights:

- Access this computer from the network
- Allow logon locally
- Deny logon as a service
- Deny logon locally

**Configuring Security Options**

There are many specialized security options that are divided into the following categories:

- Accounts
- Audit
- Domain controller
- Interactive logon
- Network security
- Recovery console
- Shutdown
- System settings
- User Account Control

Figure 10-11, Page No. 439, Complexity level: 1 Screenshot shows the “Group Policy Management Editor” dialog box. On the left pane of the dialog box, the cursor points to the Security Options within the Local policies tree under the Computer Configuration.

**Policy Application Order**

Policies can be applied at Domain level, at OU level, etc. Any settings that do not conflict with other settings will be applied. What about policy settings that conflict?

Example

- Policy at Domain level states that only Administrators can shutdown computers.
- Policy at Sales OU level states that Administrators and Mgr1 can shutdown computers.

Important  to understand the order in which the settings in the group policy should be configured.

- Usual order in which Group Policies are applied is **LSDOU:** local, site, domain, and then OU.
- Settings that conflict will be applied based on the usual order of application; **Last setting** applied will become the effective setting.

Watch Video: [Recommended Video ](https://sp.video.yuja.com/V/Video?v=17853&node=119679&a=182175462)

Here is another [video](https://sp.video.yuja.com/V/Video?v=17946&node=119977&a=34381043)

**Policy Application Order**

Special Configuration Options:

Block Inheritance

- Container option: to prevent from inheriting GPO settings from top level

Enforced

- GPO option: to force down to all child containers and overwrites all the setting conflicts (take the highest precedence)

Read more about Blocking Policy Inheritance and Enforced Policies at these links

[https://blogs.technet.microsoft.com/musings_of_a_technical_tam/2012/02/15/group-policy-basics-part-2-understanding-which-gpos-to-apply/](https://blogs.technet.microsoft.com/musings_of_a_technical_tam/2012/02/15/group-policy-basics-part-2-understanding-which-gpos-to-apply/)

**Policy Application Order**

- ABC Company has a Policy at domain level that sets every user’s desktop wallpaper to their logo
- Domain policies will be inherited by all

ABC.com

SalesOU

AccountsOU

Policy to set wallpaper to logo

ABC logo

ABC logo

- If  there are 2 Policies at domain level to set different desktop wallpaper
- Policy with lowest Link Order number is applied last

ABC.com

SalesOU

AccountsOU

Policy (Link order 2) to set ABC logo as wallpaper

Policy (Link order 1) to set Sales figures as wallpaper

Sales figures

Sales figures

**Policy Application Order**

- SalesOU Administrator creates a Policy for SalesOU to put latest sales figures as desktop wallpaper
- **LSDOU** : OU policy is applied last

ABC.com

SalesOU

AccountsOU

Policy to set ABC logo as wallpaper

ABC logo

- SalesOU Administrator can also choose to Block Inheritance (policies will not be inherited)
- There is no SalesOU  group policy

ABC.com

SalesOU

AccountsOU

Policy to set ABC logo as wallpaper

Block Inheritance

Default Desktop

Policy to set Sales figures as wallpaper

Sales figures

ABC logo

**Policy Application Order**

- Domain Administrator can Enforce his Policy – lower-level administrators can not block the policy

ABC.com

SalesOU

AccountsOU

ABC logo

ABC logo

Block Inheritance

Policy to set ABC logo as wallpaper (Enforced)

To avoid complexity in Group Policies

- Limit number of Group Policies Objects (Can a single domain policy apply to all?)
- Minimize use of Block Inheritance and Enforced to avoid complexity

**What are CIS Benchmarks?**

**Definition**: CIS Benchmarks are a set of best practices for securing IT systems and data. They provide detailed guidelines for securely configuring systems and applications.

**Purpose**: To offer a standardized approach to hardening systems, reducing vulnerabilities, and improving overall security posture.

**Developed by**: The Center for Internet Security (CIS), a global nonprofit dedicated to enhancing cybersecurity.

Additional Reading: [CIS Benchmarks Overview](https://www.cisecurity.org/cis-benchmarks-overview)

**Importance of Configuration Standards**:

- **Enhanced Security Posture**: Following CIS Benchmarks helps protect Windows systems from known vulnerabilities and threats.
- **Compliance Requirements**: Many regulations and industry standards (e.g., GDPR, HIPAA) require adherence to security best practices which CIS Benchmarks can help fulfill.
- **Risk Management**: Implementing benchmarks reduces the risk of breaches by addressing common and known vulnerabilities.

**Key CIS Benchmarks for Windows OS**

**Benchmark Overview**:

- CIS Microsoft Windows 10 and 11 Benchmark: Provides recommendations for securing Windows 10 and 11 desktops.
- CIS Windows Server Benchmarks: Guidelines for securing Windows Server environments   (e.g., 2016, 2019, 2022).

**Can be deployed by:**

- Using Active Directory GPO to deploy chosen control from CIS Benchmark
- Locally deployed on Windows client and server using Local Security Policy (secpol.msc)

**Securing Active Directory**

Securing Active Directory is very important to safeguard against unauthorized access, maintain compliance, prevent data breaches, and ensure smooth and secure IT operations. This comprehensive protection is essential for maintaining the overall integrity and security of an organization’s IT infrastructure for the mentioned key reasons.

**Single Point of Authentication**

- **Central Control**: AD provides centralized authentication for users across the network. If AD is compromised, attackers could gain unauthorized access to all systems and applications that rely on AD for authentication.
- **Access to Multiple Systems**: Compromising AD potentially gives attackers access to all systems integrated with AD, including file servers, applications, and databases.

**Domain Controller Compromise**

- **Critical Infrastructure:** Domain Controllers (DCs) are the servers that manage the AD database and handle authentication and directory services. Compromising a DC can provide attackers with the ability to manipulate the AD database, create or modify user accounts, and potentially disrupt the entire network.

**Securing Active Directory**

**Authorization Management**

- **Role-Based Access Control (RBAC)**: AD manages permissions and access rights for users and groups. If AD is insecure, attackers could escalate privileges, gaining unauthorized access to sensitive data and resources.

**Directory Information**

- **Sensitive Data:** AD holds critical information such as user credentials, group memberships, and access rights. This data is vital for the normal functioning of the organization, and unauthorized access or manipulation can lead to security breaches and operational disruptions.

**Attack Surface**

- **Entry Point for Attacks:** AD is a prime target for various attack vectors, including Pass-the-Hash, Kerberoasting, and Golden Ticket attacks. Securing AD helps protect against these sophisticated attacks that exploit authentication and authorization mechanisms.

**Privileged Account Management**

- **Administrator Accounts:** AD includes highly privileged accounts like Domain Admins and Enterprise Admins. Compromise of these accounts can lead to full control over the AD environment and other critical infrastructure components.

**Securing Active Directory**

**Service Disruption**

- **Operational Impact:** Compromise or misconfiguration of AD can disrupt IT operations, affecting user access to systems and services. Ensuring AD security helps maintain smooth and reliable IT operations.

**Recovery and Restoration**

- **Disaster Recovery:** In the event of a security incident involving AD, recovery processes can be complex and time-consuming. A secure and well-maintained AD environment simplifies recovery and reduces downtime.

**Federated Systems**

- **Single Sign-On (SSO):** AD often integrates with other systems and services via Single Sign-On (SSO). Compromising AD can impact these integrations, potentially affecting access to multiple services and applications.

**Application Security**

- **Application Access:** Many applications rely on AD for user authentication and authorization. Securing AD helps ensure that application access remains controlled and that security policies are enforced consistently.

**Some common attacks on Active Directory**

**Pass-the-Hash (PtH) Attack**

- Attackers steal hashed passwords (NTLM hashes) from memory or other sources and use them to authenticate without needing the plaintext password.

**Pass-the-Ticket (PtT) Attack**

- In this attack, attackers use stolen Kerberos tickets to authenticate to services or systems as if they were the legitimate user.

**Golden Ticket Attack**

- Attackers forge a Kerberos Ticket Granting Ticket (TGT) to gain unauthorized access to any service within the domain.

**Silver Ticket Attack**

- This attack involves forging service tickets to gain unauthorized access to specific services within the domain, without needing to compromise the Domain Controller.

**DCOM (Distributed Component Object Model) Attack**

- Exploits vulnerabilities in DCOM to remotely execute commands and gain access to AD resources.

**Some common attacks on Active Directory**

**AD Certificate Services Attack**

- Attackers exploit vulnerabilities in AD Certificate Services to issue fraudulent certificates, potentially allowing unauthorized access or man-in-the-middle attacks.

**LDAP Injection Attack**

- Exploits weaknesses in LDAP queries to manipulate or extract information from the AD database.

**Kerberoasting Attack**

- Attackers request service tickets for service accounts and then attempt to crack these tickets offline to retrieve plaintext passwords for those accounts.

These attacks exploit various aspects of AD, including authentication mechanisms, permissions, and service accounts. Defending against these attacks requires a combination of strong security practices, regular monitoring, and timely patching of vulnerabilities.
