Computer script on a screen

## **ST2612 – Secure Microsoft Windows**

***Lecture 4***

*PLANNING AND DEPLOYING PATCH MANAGEMENT*

*PLANNING AND DEPLOYING PUBLIC KEY INFRASTRUCTURE*

**Learning objectives – part 1**

1. **Explain the issues, tools, and planning for Patch Management**

2. **Plan the deployment of service packs and hotfixes**

3. **Evaluate the applicability of service packs and hotfixes**

4. **Implement Microsoft Software Update Services (WSUS) architecture**

5. **Understand the Post deployment review and rollback strategy planning**

**The Critical Challenges of Patch Management**

**The Vulnerability Landscape:** Operating systems like Windows Server 2022 are subject to ongoing **security weaknesses and flaws** that require regular remediation to maintain stability.

**Unpatched Exploit Risk:** Failures to apply patches promptly lead to **widespread concerns**, as seen with vulnerabilities like **PrintNightmare** or kernel information disclosures.

**Zero-Day Threats:** Administrators must account for **zero-day vulnerabilities**, which are threats identified before a vendor has provided a formal remediation or mitigation.

**Impact on Production:** A primary issue is the **risk that a patch might break a production system**, leading many organizations to delay updates despite the security gap this creates.

**Bandwidth Saturation:** Allowing all computers in a domain to update directly from Microsoft simultaneously can **saturate an organization’s Internet connection**, preventing access to other vital resources.

**Legacy Systems:** Older, out-of-date systems that cannot be easily patched create persistent issues, often requiring **forced upgrades** of both the OS and the applications running on them.

**Deployment and Management of Updates**

Need to keep technology environment secure and reliable. Requires identifying security vulnerabilities and responding quickly

Patch management:

- Method for keeping computers up to date with new software releases

Security patch management:

- Patch management with a concentration on reducing security vulnerabilities; essential for secure IT management and operations

**Microsoft Update and Automatic Updates**

- For consumers and small businesses (fewer than 50 computers)
- Updates can be installed with minimal or no user interaction
- Uses Internet connection to search for downloads from Microsoft Update website
- No need to understand the technical details of the security update.
- Microsoft cautions that users should ensure they do not have applications that could be affected by the updates
- Price : Free for licensed users

**Deployment and Management of Updates**

**Windows Server Update Services (WSUS)**

- For medium or large businesses
- Administrators can manage update settings and control the distribution of updates
- Administrators can test updates on selected computers before deploying to the rest of the network
- Updates can be downloaded once from Microsoft Update Website and stored on local server (free up Internet bandwidth)
- Does not support deployment of non-Microsoft updates
- Price : Free for licensed users

**Advantages of WSUS**:

- The system administrator can control the updates to be applied
- Clients can be configured to get updates from a local WSUS server instead of downloading them from Microsoft’s site, reducing network traffic
- WSUS is a means to provide updates to computers that don’t have Internet access

**Deployment and Management of Updates**

Watch Video: [Microsoft Endpoint Manager](https://sp.video.yuja.com/V/Video?v=17954&node=120040&a=29247379)

- Supports management and distribution of Microsoft and non-Microsoft software updates and applications
- Supports various types of endpoints : windows / non-windows platforms.
- WSUS  is part of it.
- Advanced administrator control features
- Price : Charges apply

**Deployment and Management of Updates**

**Microsoft Security Update Guide**

- Formally known as Microsoft Security Bulletin.
- Contains detailed guidance and information about the security update and the vulnerability.

**Microsoft Technical Security Notifications**

- A few free of charge Notification services for sign-up users
- Security Update Email Alerts - Provide new/ major revision Microsoft product security content.
- Security Advisories Alerts - Helps administrators to plan for the coming security updates.

Get updates from Microsoft website only

- Microsoft may send email notifications about security updates
- Users should download updates from Microsoft Update website

**Overview and Architectural Hierarchy**

**Centralized Update Management:** Windows Server Update Services (WSUS) is a server role that allows administrators to **centrally manage, download, and distribute** updates for Windows and other Microsoft products.

**Bandwidth Conservation:** By downloading updates from Microsoft Update only once to the WSUS server and then distributing them locally, organizations prevent **saturating their Internet connection** when multiple computers require the same patches.

**Simple Architecture:** In a basic deployment, a single WSUS server acts as a **middleman** between the client computers and the Microsoft Update content delivery network (CDN).

**Hierarchical Deployment:** Large organizations can implement a hierarchy consisting of an **upstream server** that synchronizes with Microsoft and multiple **downstream servers** that synchronize from the upstream server.

**Scalability for Large Environments:** A single WSUS instance can support up to **100,000 clients**. For environments exceeding this, multiple servers can be deployed using a **load-balanced front end**.

**Overview and Architectural Hierarchy**

WSUS Server requires the following:

- Assume that WSUS clients are synchronizing with the server every eight hours for a rollup of 5,000 clients.
- Processor: 1.4 gigahertz (GHz) x64 processor (2 Ghz or faster is recommended)
- Memory: 2 GB of RAM
- Available disk space: 40 GB
- Applies To: Windows Server 2022, Windows Server 2019, Windows Server 2016

Microsoft Update Servers

http://www.clker.com/cliparts/4/a/o/k/E/Q/server-hi.png

**WSUS Server : download approved updates**

http://www.clker.com/cliparts/k/7/D/E/O/B/computer-hi.png

http://www.clker.com/cliparts/4/a/o/k/E/Q/server-hi.png

http://www.clker.com/cliparts/k/7/D/E/O/B/computer-hi.png

**Get updates from WSUS Server**

**Status report of installed updates sent to WSUS Server**

**Database and Communication Requirements**

**Database Options:** WSUS requires a database to store update metadata and deployment information. Options include:

- **Windows Internal Database (WID):** A built-in, relational database suitable for most small-to-medium environments.
- **Microsoft SQL Server:** Used for greater scalability or when multiple WSUS servers must share a **single centralized database** for load balancing.

**Client Communication Ports:** By default, WSUS uses **TCP port 8530 for HTTP** communication and **TCP port 8531 for HTTPS**.

**Securing Traffic:** It is a best practice to secure WSUS communications using **Secure Sockets Layer (SSL)**, which requires the installation of a custom certificate and configuring the server to use port 8531.

**Storage Requirements:** Administrators must define a local folder (e.g., C:\\WSUS) to store **synchronized update content**. This volume should be fault-tolerant and have significant free space, especially for multilingual updates.

**Synchronization and Product Selection**

**The Configuration Wizard:** Upon first launch, the **WSUS Configuration Wizard** guides the administrator through selecting an upstream source and specifying proxy server settings if required.

**Selecting Products:** To save storage space, administrators should only synchronize updates for **specific product categories** (e.g., Windows 11, SQL Server 2016) actually deployed in their environment.

**Update Classifications:** WSUS should be configured to download only necessary classifications, such as **Critical Updates** and **Security Updates**, at a minimum.

**Defining the Schedule:** Synchronizations can be manual but are typically **scheduled to run automatically** (e.g., once daily at 1:00 AM) to ensure the server stays current with the latest releases.

**The Initial Sync:** The first full synchronization can take **several hours** as the server downloads the extensive Windows Update catalog from Microsoft.

**WSUS Common Administration Tasks**

During synchronization, new security updates can be handled in two ways:

- Automatically approve new versions of previously approved updates
- Do not automatically approve new versions of approved updates

In a testing environment, second option is better

Otherwise, the testers may overlook and  have skipped the testing of the new updates.

WSUS has two logs for tracking events:

- Synchronization log: keeps following information

  - Time of the last and next scheduled synchronization
  - Success and Failure notification
  - Update packages that have been downloaded and/or updated since the last synchronization, or that failed synchronization
  - Whether synchronization was a Manual or Automatic
  - Approval log: keeps track of the content that has been approved or not approved

**Client Redirection and Group Management**

**Group Policy Redirection:** Domain-joined computers are redirected to the WSUS server by a **Group Policy Object (GPO)**. The setting **"Specify intranet Microsoft update service location"** must be enabled and point to the WSUS URL (e.g., http://WSUS1:8530).

**Client-Side Targeting:** When enabled via GPO, computers can **automatically assign themselves** to a specific WSUS group (e.g., "Domain Servers") upon their next check-in.

**The Approval Workflow:** Updates are **not installed** until they are approved by an administrator. Updates can be **Approved for Install**, **Declined**, or **Approved for Removal**. After updating, WSUS clients will notify WSUS Server and WSUS Server can maintain update status of all clients

**Client Redirection and Group Management**

**Automatic Approval Rules:** To reduce overhead, rules can be created to **automatically approve specific updates** (like antivirus definitions or critical security patches) for certain groups.

**WSUS clients** can be placed into Computer Groups

Sample Usage : some clients can be put into a “Test” computer group

- Administrator approve new security updates for the “Test” group
- Computers in the “Test” group apply the updates
- Administrators test the results before allowing other computers to apply the updates

Sample Usage:  Servers with the same roles can be put into the same group

- They can receive relevant update at the same time.

**Patch Management processes**

**Five-Stage** approach:

- Stage 1: Receive Microsoft Security - Release Communications
- Stage 2: Evaluate Risk
- Stage 3: Evaluate Mitigation
- Stage 4: Deploy Updates - 6 steps
- Stage 5: Monitor Systems

~https://www.dnsstuff.com/wp-content/uploads/2020/02/patch-management-process-best-practices.png

**Stage 1: Receive Microsoft Security Release Communications**

Microsoft sends out a notification if there is issue affecting customers’ security

If security changes are required, a security update is released.

Patch Tuesday

- Security updates and the corresponding security bulletin normally released on the second Tuesday and sometimes forth of the month.
- "Exploit Wednesday“ - This term was coined as many exploitation events were seen shortly after the release of a patch
- Since 2015, Microsoft release security updates to home PCs, tablets and phones as soon as they are ready, while enterprise customers will stay on the monthly update cycle.

Urgent updates will be released immediately.

Microsoft provides several ways of receiving information about updates :

- Email: Security Notification Service Comprehensive Edition (remember, update installers are never attached to the email!)
- RSS: Comprehensive Alerts - [https://msrc-blog.microsoft.com/feed/](https://msrc-blog.microsoft.com/feed/)
- Twitter - [https://twitter.com/msftsecresponse](https://twitter.com/msftsecresponse)
- Security Update Portal at  [https://portal.msrc.microsoft.com/en-us/security-guidance](https://portal.msrc.microsoft.com/en-us/security-guidance)

**Stage 2: Evaluate Risk**

Administrators should ask :

- Does the vulnerability apply to the organization?

  - To answer to this question, system admin should have kept an up-to-date inventory list of all IT assets of the organization.

- Does the vulnerability represent a risk high to the organization?

The deployment of a security update has a cost :

- Costs of testing the updates
- Costs of ‘deploying’ the updates
- Support costs in case of any negative result after the update (Example : important application does not work properly after update)

**Stage 2: Evaluate Risk**

Microsoft has four severity ratings :

- **Critical** : A vulnerability whose exploitation could enable the propagation of an Internet worm with little or no user action.
- **Important** : A vulnerability whose exploitation could result in compromise of the confidentiality, integrity, or availability of users’ data, or of the integrity or availability of processing resources.
- **Moderate** : A vulnerability whose exploitation is mitigated to a significant degree by factors such as default configuration, auditing, or difficulty of exploitation.
- **Low** : A vulnerability whose exploitation is extremely difficult, or whose impact is minimal.

**Stage 3: Evaluate Mitigation**

While administrators are evaluating security updates, some short-term security control may be applied.

For example, adjust firewall policies, or restrict a port only to a specific subnet instead of the whole network.

Microsoft may provide some suggested mitigation or workarounds in their security advisories if the security update cannot be applied immediately

Such mitigations and workarounds are meant for short-term use, they do not replace security updates

**Stage 4: Deploy Updates**

**SIX (6) steps** to deploy an update:

1. Plan the deployment.

- Determine which updates need to be rolled out quickly and which ones can be subjected to a more thorough testing process
- Deployment Schedule - do some computer groups need the updates more urgently? By which date or time, must all computers be updated?

2\. Determine whether the security update is available for download.

- If there is no security update available, Microsoft would advise the appropriate actions to take to protect the computer systems

3\. Obtain the required update files. Security update files can be obtained from several sources like :

- Microsoft security guide.
- Microsoft deployment tools, such as Microsoft Update, Windows Update, WSUS, or Endpoint Manager.
- The Microsoft Download Center. See [www.microsoft.com/download/](http://www.microsoft.com/download/)
- The Microsoft Update Catalog service. See catalog.update.microsoft.com

**Stage 4: Deploy Updates**

4\. Create the update package. (If security updates need to be customised)

5\. Test the package.

- To ensure that business-critical systems will continue to run successfully after the security update has been deployed.

- To ensure the package can be ‘uninstalled’ or there is a way to roll back.

- To ensure the system can be restarted properly.

- To ensure the update is effective.

- An update can be tested in 2 ways:

  - Test Environment:

    - Test lab with computers mirroring the actual environment
    - Extra overhead incurred

  - Pilot Environment:

    - Test on selected production computers
    - Authentic
    - Can test the deployment plan too
    - Extra risk incurred

6\. Rolling out the deployment

- Carry out the deployment of the update to the computers that need it
- Compliant to the Standard Patch Deployment Timeline

**Stage 5: Monitor Systems**

Determine which systems successfully deployed the update and which systems did not.

Possible reasons why update was not successfully deployed :

- The computer is offline.
- The computer is being rebuilt or reimaged.
- The computer has insufficient disk space.
- The computer is not communicating with the update server.
- The required update client software is not running on the computer.
- The computer is missing some dependent software.

Need to resolve the problem and get update applied.

Windows Update Catalog

- Download the specific update package from windows update catalog web site. To install the update locally.

Windows Update Troubleshooter

- A free tools from Microsoft which can fix most of the common windows update errors

**Post-deployment Review**

Conducted after the deployment

The review includes these steps:

- Review organization’s performance during the incident
- Discuss changes to your service windows
- Assess the total incident damage and cost (if any)
- Update the existing baseline for your environment

**Learning objectives – part 2**

6. **Plan, build and manage certification authority hierarchies**

7. **Understand the concept of PKI and explain its usefulness for resource access authentication and authorization.**

8. **Configure and deploy certificate authorities**

9. **Describe certificate enrollment and renewal method**

**What is a digital certificate?**

Before we proceed to PKI, you may need to answer the following questions:

- Why do we need it?

- How can we tell if a digital certificate is genuine ?

  - Watch Video: [What are certificates ?](https://sp.video.yuja.com/V/Video?v=17980&node=120124&a=4943201)
  - Watch out the content at 14:21 and onwards

Microsoft Windows Server enables secure data access based on the use of digital certificates

Certificates allow users or systems to prove to others that they are who they say they are

Before you can use digital certificates, you need to design a Public Key Infrastructure (PKI)

Reference : [Understanding Active Directory - Active Directory Certificate Services](https://sp.video.yuja.com/V/Video?v=17974&node=120111&a=71130180)[https://sp.video.yuja.com/V/Video?v=17974&node=120111&a=71130180](https://sp.video.yuja.com/V/Video?v=17974&node=120111&a=71130180)[https://sp.video.yuja.com/V/Video?v=17974&node=120111&a=71130180](https://sp.video.yuja.com/V/Video?v=17974&node=120111&a=71130180)

**Defining Certificate Requirements**

To protect exchanging messages from eavesdropping attack

- A key-pair is associated with an individual party. Each individual party will keep its private key as a secret but share out its public key to everyone. Messages being encrypted with the public key can only be decrypted by the corresponding private key.  A digital signed message with the individual private key can only be verified by its corresponding public key.

There is one problem

- How can we  obtain a genuine public key of a particular party before we can establish a secure communication channel with the party ?

Public Key Infrastructure (PKI)

- Refers to a technology that includes a series of features relating to authentication and encryption. Based on a system of certificates, each certificate contains a public key and the name of the subject. Allows an organization to publish, use, renew, and/or revoke certificates and enroll clients.

Certificates may be required if you are using any of the following applications:

- Digital signatures
- Secured e-mail
- Secured HTTP communication (HTTPS/SSL)
- IP security
- Encrypted file system

**Certificate Authority Hierarchies and Roles**

A **Certificate Authority** (CA) is an entity that issues digital certificates to other parties

The **Root** CA can issue certificates to Subordinate, Intermediate or Issuing CAs

**Issuing** CAs can issue certificates to users or clients

Types of hierarchies that can be used for CAs:

- Rooted trust model
- Cross-certification trust model
- Hybrid trust model

Roles that can be chosen for CAs include:

- For Standalone CA

  - Rudimentary CA
  - Intermediate CA

- For Enterprise CA

  - Basic-security CA
  - Medium-security CA
  - High-security CA

**Rooted Trust Model**

A model in which the root CA has a self-signed certificate, and the CA issues a certificate to all direct subordinate CAs

- CAs in this model can be online or offline; allows flexibility in deploying and managing PKI
- Each CA serves a single role within the hierarchy and is not dependent on other CAs; allows rooted trust hierarchies to be more scalable and easier to administer than other hierarchies
- Possible to add a new CA to a hierarchy

Rooted trust hierarchies fall into two subcategories: two-tier CA hierarchy and three-tier CA hierarchy

- Any CA in a rooted trust hierarchy is either a root or a subordinate but never both
- Each CA is responsible for processing requests, issuing certificates, revoking certificates, and publishing CRLs
- Each CA can be managed independently

**Cross-Certificates Model**

A model in which all CAs are self-signed and trust relationships between CAs are based on cross-certificates

- Certificates issued by one CA will be trusted by computers which belong to other CA hierarchy (cross hierarchy trust)
- Cross-certificates does not need to be bidirectional
- Advantages: Low cost and greater flexibility
- Disadvantage: Greater administrative overhead and increase risk of an unauthorized access to internal resources

**Using Third-Party CAs**

Useful when an organization conducts most of its business with external customers and clients

Examples of third-party CAs: DigiCert, GlobalSign or GoDaddy

Advantages:

- Customers have greater degree of confidence when conducting secure transactions with organisations because they trust the third-party CA
- Allows an organization to take advantage of third party CA’s understanding of technical, legal, and business issues with certificate use

Disadvantages:

- High per-certificate cost
- Allow less flexibility in managing certificates
- Autoenrollment is not possible

Windows System already configured with a list of trusted commercial third-party CAs

- Can be easily viewed and configured via your web browser.

**Installing Certificates Authority Roles**

Root CA is installed first, followed by installing intermediate CAs, and finally the issuing CAs

Root CA: CA that is at the top of a certification hierarchy and must be trusted unconditionally by clients in an organization

- An enterprise root CA stores its information in Active Directory and the server that hosts the service must  be joined to a domain.
- Storing the root CA offline is much more secure

Intermediate CA: CA subordinate to a root CA

Issuing CA: issues certificates to users and computers

**Selecting A Certificate Enrollment And Renewal Method**

Depends on:

- Types of certificates that you intend to use
- Number and type of clients that you enroll

Two types:

- Manual : if administrative approval is required; most useful for issuing and renewing computer and IPSec certificates
- Automatic: if no approval is necessary; can improve administrative control over certificates

Methods for automatic enrollment include:

- Certificate autoenrollment and renewal: based on a combination of Group Policy settings and certificate templates
- Certificate Request Wizard and Certificate Renewal Wizard
- Web enrollment support pages
- Smart card enrollment station

**Configuring Certificate Templates**

Certificate Services provides certificate templates to simplify the process of requesting and issuing certificates

- Each template contains the rules and settings; can serve a single purpose or multiple purposes
- Certificate templates are available only on Enterprise Root and Subordinate CAs
- Stored in Active Directory
- Available to every enterprise CA in the forest

Certificate Templates MMC snap-in provides administrators with the capability to:

- Create additional templates by duplicating and modifying existing templates
- Modify template properties
- Configure policies applied to certificate enrollment, issuance, and application
- Allow the autoenrollment of certificates
- Configure ACLs on certificate templates

**Configuring Certificate Templates**

Issuance of certificate requests can be controlled in three ways:

- Configuring permissions on the template from the Security tab
- Preventing the CA from issuing that certificate type by deleting the template
- Configuring the permissions on the CA

Restrict permissions on your CAs to prevent unauthorized access

Configure the discretionary access control list (DACL) for each template

**Deploying and Revoking Cert. for Users, Computers, and CAs**

Possible to automate the deployment of certificates by configuring Group Policy

Manual approval should be required for:

- All certificates that are needed to perform network administration or software development
- All certificates issued to joint venture partners

Conditions to auto-enrol certificates:

- Client computer must be integrated into Active Directory
- Users require the Read, Enroll, and Auto enroll permissions

Reasons for revoking a certificate:

- Certificate is compromised
- Termination of the account to whom the certificate was issued

**Deploying and Revoking Certificates**

All certificates have specified lifetimes

In some situations, there is need to invalidate or revoke a certificate before it has reached the end of its lifetime. Revoked certificates are published in the certificate revocation list (CRL).

CRLs are valid only for a limited time, PKI clients need to retrieve a new CRL periodically

CRL location must be define along with the access path before deploying certificates

OCSP - Online Certificate Status Protocol described in RFC 6960 – Proposed in 2013. An Internet protocol used for obtaining the revocation status of an X.509 digital certificate. It complements (not replacement)  the operations of CRLs.

OCSP Responder - A server typically run by the certificate issuer which returns a signed response on the status of a particular issued certificate.

Possible response:

- Good (Valid), Revoked (Invalid) or Unknown
- No response, as the issuer does not implement the responder service

Comparison to CRLs

- Less burden for clients to maintain CRLs (network and storage).
- Easier to implement compares to CRLs for up-to-date checking.
- OCSP use non-encrypted message, it may be subjected to interception.
- OCSP is an optional component for the cert server.

**Deploying and Managing SSL Certificates**

How to authenticate the identity of the remote Server

- E.g.  Hackers may setup phishing site to mislead clients to submit confidential information / even pay for a non-existing service.
- How to ensure the transferred content is not tampered or intercepted by unauthorized parties.

One possible solution

- Using SSL Certificates

Secure Sockets Layer (SSL): public key–based security protocol

SSL process uses:

- Certificates for authentication
- Encryption for message integrity and confidentiality
- Requires installation of valid server certificate to establish the encrypted communications

Ways to obtain certificates:

- Can be created using Certificate Services
- Can be obtained from trusted third-party CA

**Configuration of the Web Server for SSL Certificates**

HTTPS (HTTP over Secure Sockets Layer):

- A technology that encrypts individual messages in Web communications rather than establishing a secure channel
- Popular e-commerce technology and is used for secure online shopping
- Communicates on port 443
- SSL-secured URLs begin with https:// prefix

| Method | Security Level |
| --- | --- |
| Anonymous authentication | None |
| Basic authentication | Low |
| Forms authentication (IIS7 or later) | Low or Medium (with SSL) |
| Digest authentication | Medium |
| Advanced Digest authentication | Medium |
| Certificate authentication | High |
| Integrated Windows authentication | High |
| .NET Passport (IIS5/IIS6) | High |
| Windows Live ID (IIS7 or later) | High |

**Configuration of the Web Server for SSL Certificates**

Use SSL encryption only for sensitive information; encrypted transmissions can significantly reduce transmission rates and server performance

Server certificates provide a way for users to confirm the identity of the Web site

A server certificate contains

- Organization name affiliated with the server content
- Name of the organization that issued the certificate
- A public key that is used to establish an encrypted connection

**Issued Certificates**

Considerations when deciding to issue (**Self-Issued**) certificates:

- Microsoft Certificate Services can accommodate different certificate formats and provide for auditing and logging of certificate-related activity
- Evaluate the cost of each certificate.
- Keep the learning curve in mind
- Evaluate the willingness of external vendors and clients to trust your organization as a certificate supplier

Considerations when deciding to issue (**Publicly Issued**) certificates:

- Certificate can be obtained from a mutually trusted, third-party CA, e.g., VeriSign, GlobalSign or Thawte

- Wait time: Several days to several months

- Must be renewed on a regular basis

- General rules about any type of Web certificates:

  - Each Web site can have only one server certificate assigned to it
  - One certificate can be assigned to multiple Web sites
  - You can assign multiple IP addresses per Web site
  - You can assign multiple SSL ports per Web site

**Configuration of the Client for SSL Certificates**

Typical client certificate contains following items of information:

- Identity of the user
- Identity of the certification authority
- A public key used for establishing encrypted communications
- Validation information, such as an expiration date and serial number

To protect your Web content from unauthorized access you must do one of the following:

- Use Basic, Digest, or Integrated Windows authentication, in addition to requiring a client certificate
- Create a Windows account mapping for client certificates

**Certificate Renewal**

Security and renewal requirements for certificates should be based on following factors:

- Value of the network resources protected by the CA trust chain
- Degree to which you trust your certificate users
- Amount of administrative effort that you are willing to devote to certificate renewal and CA renewal
- Business value of the certificate

To lookout for certificate renewal:

1. All certificates have an expiry date.
2. We need to renew a certificate before its expiry date.
3. Since each certificate contains a public key for a specific application. One type of renewal is just to create and sign on a new certificate which contains the same public key (Certificate Renewal Operation).
4. The public key owner may generate a new key-pair and replace the old public key with the newly generated public key while renewing the certificate (Key Renewal Operation).
5. Finally, the signer itself, should also update its key-pair. Once the signer’s key-pair is changed, a new rootCA certificate should be generated too.
6. To keep the PKI operation runs smoothly, you may see multiple rootCA certificates for the same CA organization are co-existing in the Trusted Root Certificate Authorization store.
