Computer script on a screen

## **ST2612 – Secure Microsoft Windows**

***Lecture 5***

*PLANNING AND DEPLOYING SECURITY FOR NETWORK COMMUNICATIONS*

*PLANNING AND DEPLOYING AUTHENTICATION FOR REMOTE ACCESS USERS*

**Learning objectives – part 1**

1. **Configure Secure Network Communications with IPSec**

2. **Explain the concept and implications of deploying IPSec**

3. **Explain Network Authentication in Windows Server Operating System**

4. **Configure IPSec policies**

5. **Deploy and manage IPSec policies**

**Introduction to IP Security (IPSec)**

IPsec: a set of protocols, used to ensure private, secure communications over IP networks by using cryptographic security services; based on:

- Protecting the contents of IP packets
- Providing packet filtering
- Enforcing trusted communication
- Securing communication with encryption of the information traveling the network

Original TCP/IP Network protocol was not designed with security in mind

- No data integrity or confidentiality

Subject to

- DOS attacks, Source Spoofing, Replay attacks, Spying  and more …

IPsec utilizes three main security control elements:

- Internet key exchange (IKE): protocol for exchanging encryption keys
- Authentication Header (AH): provides an authenticity guarantee for packets
- Encapsulating security payload (ESP): provides confidentiality through encryption

IPsec also utilizes IP Compression (IPComp), which is used to compress raw IP data

Watch Video: [What is IPSec?](https://sp.video.yuja.com/V/Video?v=17996&node=120215&a=135829708)

**IP Security Implementation**

Windows Server 2022 supports the implementation of IP security (IPsec)

When an IPsec communication begins between two computers

- The computers first negotiate (using IKE module) and authenticate between the receiver and sender (Using Microsoft AuthIP extension)

Next, extra hashing scheme \[optional] will help to ensure data integrity at packet header.

Finally, data is encrypted with integrity check \[optional] at the NIC of the sending computer as it is formatted into an IP packet

IPsec can provide security for all TCP/IP-based applications and communications protocols

IPsec security policy

- Can be established through the Group Policy in an Active Directory
- Can also be configured through the IP Security Policies Management MMC snap-in
- IPsec security can also be configured via the Windows Firewall Advanced Security Rules.
- Provide more IKE authentication options and AES encryption support.

**Planning an IPsec Deployment**

Configuration of IPsec is based on:

- The way that you are using IPSec.
- Types of client operating systems in your network.
- Types of network devices in your network.

IPsec is not recommended for the following:

- To secure communication between domain members and their domain controllers

  - Reduces network performance
  - Configuration of the IPsec policy is complicated

- To secure all traffic in a network

  - Again, reduced network performance
  - IPsec can’t support multicast and broadcast traffic
  - Network tools that need to inspect packet headers may not work

**Planning an IPsec Deployment**

Proper uses of IPsec are as follows:

- Packet filtering such as using IPsec with the Routing and Remote Access service to permit or block inbound or outbound traffic
- Securing host-to-host traffic on specific paths
- Securing traffic to servers
- Combining L2TP and IPsec (L2TP/IPSec) for securing VPN scenarios
- Incorporating site-to-site or gateway-to-gateway tunnelling

Deploying IPsec can affect network performance and compatibility with other services & applications

Do not deploy IPsec if the security it provides is not required

Impact of IPsec :

- Time needed to establish IPsec connection
- Time needed to filter and encrypt packets
- Increased packet size
- Network inspection technologies used in routers, firewalls, Intrusion Detection System (IDS) may not work on IPsec packets
- Application compatibility with other platforms
- Effect on Active Directory and domain controller connections

**The 5 Elements that make up an IPSec rule**

Each IPsec Policy may consist of multiple rules

Each rule contain **5** configurable elements

**Connection Type** (LAN, Remote, ALL)

**Modes** (Transport or Tunnel)

IPsec security has two modes:

**Transport** mode (Within LAN):

- Used to secure communications within a network and it can be server-to-server or server-to-client
- IPSec provides end-to-end security

**Tunnel** mode (In WAN):

- Used to secure communications between networks          (E.g., Between two gateways)
- Used with gateways or end systems that do not support L2TP/IPSec or PPTP VPN site-to-site connections

## A : Tunnel established between router and Firewall so Alice can connect to HR Servers securely  (gateway to gateway)

**Which IPSec Mode to use?**

B : Tunnel established  between VPN client running on Alice’s PC and IPsec gateway

> In the scenario of case A, an IPsec tunnel is configured between the router (close to Alice's PC) and the Firewall of the company.
>
> In this case, the traffic between Alice’s PC and the HR server is protected by the IPsec tunnel.
>
> In this slide, for case B, an IPsec tunnel is configured between Alice's PC (Yes, Alice's PC is also capable to set up as a tunnel end point) and the router device (at the edge of  the company network).
>
> In this case, the traffic between Alice’s PC and the company network is protected by the IPsec tunnel configurations.
>
> One of the differences between case B and case A is that all the traffic from Alice’s PC to the closest router is  protected by IPsec.
>
> Can you spot another difference between Case B and Case A ?

## C : Tunnel established  between router and IPsec software running on HR Server

**Which IP Mode to use?**

D : Transport mode used between Alice’s PC logging in to the Firewall to configure it (client to server)

> In this slide, for case C,  the tunnel end-points are the router (closest to Alice's PC) and the HR server.
>
> Can you describe the main difference between this case and the case B configuration ?
>
> In case D scenario, Alice's PC is connecting to the office firewall with a VPN connection. Therefore, Alice's PC can use IPsec in transport mode to establish a secured communication channel between the PC and the HR server.
>
> However, if the VPN connection is already providing secured communication, the IPsec protection applied here may be redundant.  Do you think so?

**IPsec Filters**

**IPsec Filter** is a specification in the IPsec rule that is used to match IP packets.

- Filters are grouped together in a system wide Filter List.

In the IPsec rule properties setting

- The system wide filter list will be available for the choice of the filter.

Packets which match a filter

- Will be applied with the associated filter actions such as permit, block, or negotiate security

Caveat

- A system configured with IPsec may not apply the expected security scheme cause the filter is set wrongly.
- When the network traffic does not match the IPsec, it will not be blocked, but it will just pass through.

**IPsec Filters**

Filter list identifies traffic based on its source, destination, and protocol.

- All ICMP Traffic
- All IP Traffic

Filter action is set for each type of traffic as identified by a filter list.

- Permit
- Block
- Negotiate Security

Administrator may construct/customise specific filter actions

IPsec policy can be managed in one of two ways:

- Create a new policy and define the set of rules for the policy, adding filter lists and filter actions as required
- Create the set of filter lists and filter actions, then create the policies and add rules that combine the filter lists with filter actions

**Planning Authentication Methods for IPsec**

If the connection matches the filter, IKE (Phase 1) will be invoked for initial authentication

Available **Authentication** Methods:

1. Kerberos v5

2. Certificates

   - Requires PKI (Public Key Infrastructure)

3. Pre-shared Key

The authentication process is done using asymmetric key encryption (DH) for data exchange.

Methods for authentication include Kerberos v5

- Default authentication method for Windows 2000  server or later
- Based on mutual authentication
- Use when all your clients can authenticate using Kerberos v5
- A method that requires the least administrative effort.

**Planning Authentication Methods for IPsec**

Methods for authentication include:

Certificates:

- A method of granting access to users based on their unique identification
- Used in situations that include access to corporate resources, external business partner communications, or computers that do not run the Kerberos v5

Pre-shared key:

- Used when other methods are not available
- It is a shared secret key

Choosing IPsec Authentication Method

- More than one can be selected

- Depend on priority level

- IKE – Phase 1

  - Will sort out if two parties share one common Authentication method

**Configuring IPsec Policies Between Networks and Hosts**

IPsec Authentication specifies how the computers will prove their identities to each other when \
trying to establish a SA (Security Association)

**Encryption Levels**

Two basic categories of **Encryption**:

- Symmetric key encryption
- Public key encryption

Methods of hashing

- Secure Hash Algorithm (SHA): uses a 160-bit encryption key, very-high security method
- Message Digest 5 (MD5): uses a 128-bit encryption key, lower performance than SHA
- Hashing method is used to support AH \[Authentication Traffic]

Methods of encryption

- Data encryption standard (DES): uses a 56-bit key, not recommended for high-security
- Triple DES (3DES): key length is 168-bit, provides medium- and high-security networks
- Encryption method is used to support ESP \[Encapsulating Security Payload Traffic]

**IPsec Protocols**

Tunnel mode uses double encapsulation, suitable for protecting traffic between network systems, such as an un-trusted medium like the Internet

AH Tunnel Mode: encapsulates an IP packet by placing an AH header between internal IP header and external IP header

ESP Tunnel Mode: IP packet is first encapsulated with an ESP header, then the result is encapsulated with an additional IP header

**Deploying IPsec Policies**

IPSec policies can be deployed using:

- Local policy objects: a way to enable IPsec for computers that are not members of a domain
- Group Policy Objects: IPsec policy is propagated to any computer accounts that are affected by the GPO
- Command-line tools: netsh IPsec command in windows server
- IPsec configurations can be done via the Windows Defender Firewall Advanced Security Settings (WFAS).

Understanding Default IPsec Policies:

Client (Respond Only)

- Will secure communication only if the other computer requests for it

Server (Request Security)

- Accepts initial incoming communication that is unsecured
- Requests for communication to be secured
- Will carry on with unsecured connection if client does not support

Secure Server (Require Security):

- Accepts initial inbound communication that is unsecured
- Requires that all connections to be secured

**Understanding IPsec Policy Precedence**

Possible to configure IPsec for multiple containers that will affect a computer

Only one IPsec policy can be assigned to a computer at a time

If there are multiple IPsec policies assigned at different levels, the last one takes effect

Precedence sequence: From lowest to highest (Local GPO, Site, Domain, OU)

Default order can be overridden using Enforced, Block Policy Inheritance

When troubleshooting IPsec policies and their precedence, there are list of items to remember:

- Only a single IPsec policy can be assigned at a specific level in Active Directory
- An IPsec policy assigned to an OU takes precedence over a domain-level policy for members of that OU
- IPsec policies from different organizational units are never merged
- An OU inherits the policy of its parent OU unless the policy inheritance is explicitly blocked or a separate policy is explicitly assigned to that OU
- Before assigning an IPsec policy to a GPO, verify the group policy settings that are required for the IPsec policy
- Use the Enforced and Block Policy Inheritance features carefully

**Understanding IPsec Policy Precedence**

Procedure for deleting policy objects:

- Unassigned the IPSec policy in the GPO
- Wait 24 hours to ensure that change is propagated
- Delete the GPO

Persistent policy: provides maximum protection against attacks during computer startup

- Adds to or overrides local or Active Directory policy
- Remains in effect regardless of whether other policies are applied
- Provides backup security in case IPsec policy gets corrupted or if errors occur
- Can be set using command line tools – netsh at the local station.

**Learning objectives – part 2**

6. **Plan, build and manage Network Policy and Remote Access Services in Windows Server Operating System**

7. **Implement secured SSL remote access for IIS Server**

8. **Use Virtual Private Networks and Network Address Translation**

9. **Use RADIUS Server**

10. **Deploy Network Policy Server (NAP) and NAP Console**

**Introduction to Remote Access**

Remote Access Services (RAS) role

- Can be enable through three means: virtual private networking, DirectAccess,  and Web Application Proxy

Virtual private network (VPN)

- Like a tunnel to a larger network that is restricted to designated member clients only

Related Topics (you may research on):

DirectAccess

- Establishes two “tunnels” for connecting to a Direct Access server used in remote access
- Transparent to users and always on
- Web Application Proxy

So that users external to an organization can access those applications on the organization’s servers

**Implementing a Virtual Private Network**

VPNs can use an Internet connection or an internal network connection as a transport medium to establish a connection with a VPN server

A VPN uses LAN protocols as well as tunneling protocols

- To encapsulate the data as it is sent across a public network such as the Internet

Benefit of using a VPN

- Users can connect to a local ISP and connect through the ISP to the local network
- VPN is used to ensure that any data sent across a public network, such as the Internet, is secure
- VPN creates an encrypted tunnel between the client and the RAS server

> This diagram shows three different remote clients to Site VPN implementations:
>
> Top : Firewall Based VPN for remote client access to office Network.
>
> Middle : Router Based VPN for remote client access to office Network.
>
> Bottom : Microsoft RAS based VPN ( use basic router) for remote client access to office Network.
>
> In the context of SMW, we are covering the Microsoft RAS Based VPNs only.

**Implementing a Virtual Private Network**

A VPN server uses two or more NICs for communication

- One NIC is used to connect to the private network inside the organization that uses private IP addressing
- The other NIC connects to the external public network

To create this tunnel, the client first connects to the Internet by establishing a connection using a remote access protocol

Once connected to the Internet, the client establishes a second connection with the VPN server. The client and the VPN server agree on how the data will be encapsulated and encrypted across the virtual tunnel

Caveats :

- Do not use any of your Domain Controller to be the VPN server.

Figure 9-1, Page No. 379, Complexity level: 4 A figure shows “A VPN network structure.” The figure shows three tube structures labeled “Subnet 172.2.17.19 at the top, Subnet 172.17.7 at the left bottom, and Subnet 172.17.23 at the right bottom. The Subnet 172.17.19 is connected to two windows server 2016 servers and internal firewall (private IP addresses) at the top. The Subnet 172.17.7 is connected to two systems at the bottom and a CPU with IP address 172.17.7.44 at the top. The Router is connected between the Subnet 172.17.19 and Subnet 172.17.7. The Subnet 172.17.23 is connected to two CPU and one CPU with an IP address 172.17.23.10. A router is connected between the Subnet 172.17.19 and Subnet 172.17.23.10. In Subnet 172.17.19, the internal firewall (private IP addresses) is connected to a Windows Server 2016 with VPN/IIS in the DMZ and it is connected to DSL telephone lines that are connected to two internets on either sides. Each of the two internets is connected to DSL adapter card through the DSL telephone lines and finally to the system. The Windows Server 2016 with VPN/IIS in the DMZ is connected to an IP address 172.17.23.10, to the internet shown at the bottom and it is collectively labeled “VPN tunnels.”

> This diagram shows how a remote client connects to a site ( e.g., a staff remote access to his office network via VPN)
>
> The net effect after the VPN connection has established:
>
> 1. The remote client will be assigned with a set of local IP settings: such as (IP address, DNS, Gateway).
> 2. The remote client can access to all the services offered by the Site as if it is connected to the LAN directly.
> 3. By default, all the outgoing traffic from the remote client will first go to the VPN server via the internet.

**Using Remote Access Protocols**

Remote access protocol carries the network packets over a wide area network (WAN) link.

- IP is the most used transport protocol
- Several remote access protocols are used by Windows Server and its remote clients

An early basic remote access protocol in use is PPP

Point-to-Point Protocol (PPP)

- Used  in legacy remote communications involving modems
- Enables the authentication of connections and encryptions for the network communications, but is not considered to be as secure as more modern options
- Can automatically negotiate communications with several network communication layers at once

When you implement a Windows Server VPN server, you have the choice of three (or four) remote access tunneling protocols:

- Point-to-Point Tunneling Protocol (PPTP)
- Layer Two Tunneling Protocol (L2TP)
- Secure Socket Tunneling Protocol (SSTP)
- IKE v2 Protocol  (IPSec , tunnel mode , IKE version 2)

**Using Remote Access Protocols**

Point-to-Point Tunneling Protocol (PPTP)

- Offers PPP-based authentication techniques
- Encrypts data carried by PPTP through using Microsoft Point-to-Point Encryption (MPPE)

Layer Two Tunneling Protocol (L2TP)

- Works similarly to PPTP
- Uses Layer Two Forwarding that enables forwarding based on MAC addressing
- Uses IP Security (IPsec) for additional authentication and for data encryption

Secure Socket Tunneling Protocol (SSTP)

- Employs PPP authentication techniques

- Encapsulates the data packet in the Hypertext Transfer Protocol (HTTP) used through Web communications

- Additionally uses a Secure Sockets Layer (SSL) channel for secure communications

  - SSL is a data encryption technique employed between a server and a client
  - SSL has now evolved into Transport Layer Security (TLS)

- Viewed as more secure than PPTP or L2TP

**Using Remote Access Protocols**

Internet Key Exchange Version 2 Protocol (IKE v2)

- Employs IPsec in tunnel mode protocol over UDP port 500 and 4500.
- Encapsulates datagrams by using IPsec ESP or AH headers for transmission over the network

Encryption:

- AES-256 (Advanced Encryption Standard), AES-192, AES-128, and 3DES encryption algorithms.
- 3DES is not recommended.

Pros

- Believed to be faster and extremely secured protocol.
- IKEv2 supports mobility (MOBIKE), it is much more resilient to changing network connectivity.
- Switch between access points / wired to wireless connection.

Cons

- Proprietary (like SSTP) – but there are open-source implementation.
- Support by some Network Devices vendor.

**Configuring a VPN Server**

General steps:

- Install the Network Policy and Access Services role
- Configure a Microsoft Windows Server as a network’s VPN server, including configuring the right protocols to provide VPN access to clients
- Configure a VPN server as a DHCP Relay Agent for TCP/IP communications
- Configure the VPN server properties
- Configure a remote access policy for security

> We will base on the network infrastructure configuration shown in this slide to go through a simple RAS or VPN gateway setup in the following section.
>
> This simulation requires 4 Virtual Machines. 3 of them are windows systems and the other one is a Linux system that serve as a virtual router in this setup.

**Configuring a VPN Server**

The VPN server must be able to send communications through the network

- An early configuration step is to make sure its communications can go through a firewall set up at the server

If you are using **Windows Firewall** on your server

- TCP and UDP ports used by VPN are unblocked by default when you configure a VPN server
- However, it is important to make certain they are unblocked

After the VPN server is set up

- The **configuration wizard** will be invoked to guide you through the configuration.

After the initial setup. You can further configure it from the Routing and Remote Access tool

- By right-clicking the VPN server in the tree and clicking Properties

**Configuring a VPN Server**

DHCP Relay Agent

- Broadcasts IP configuration information between the DHCP server on a network and the client acquiring an address
- You can configure the VPN server as a DHCP Relay Agent using the Routing and Remote Access tool

In basic terms:

- The client contacts the VPN server to make a connection
- The VPN server, as a DHCP relay agent, contacts the DHCP server for an IP address for the client
- The DHCP server notifies the VPN server of the IP address
- The VPN server relays this IP address assignment to the client

You can set up VPN security through a remote access policy

- Greatly reduces administrative overhead and offers more flexibility and control for authorizing connection attempts

Elements of a Remote Access Policy

- Access permission
- Conditions
- Constraints
- Settings

**Configuring a VPN Server**

First step in evaluating access is to determine if access permission is enabled at the VPN server

- Default permission for this policy is set to Deny access
- Change the default permission to Grant access in the default remote access policy

The conditions of a remote access policy are a set of attributes that are compared with the attributes of the connection attempt

If the connection attempt matches the conditions of a remote access policy, the constraints are evaluated

Settings in the remote access policy are then examined

- Settings include elements such as IP filters, encryption, IP settings, and others

Establishing a Remote Access Policy

- You can use the Routing and Remote Access tool to create and configure a remote access policy

**Monitoring VPN Users**

After you have configured the VPN server and placed it in production

- You should periodically monitor the users who are connected

Use the Routing and Remote Access tool to monitor connected users

With the tool open:

- Expand the elements under the server's name in the left pane
- Click Remote Access Clients in the left pane
- The right pane shows the users who are connected
- If you want to disconnect a user, right-click the user and click Disconnect

**Troubleshooting VPN Installations**

Troubleshooting a VPN communications problem can be divided into hardware and software troubleshooting tips

**Hardware Solutions**

Use Device Manager to make sure network adapters and WAN adapters are working properly

- Also, in Device Manager to make sure that an adapter has no resource conflicts

For an external DSL adapter or a combined DSL adapter and router

- Make sure the device is properly configured and connected
- Check its monitor lights for problems
- Call your ISP to determine if problems are present on the ISP’s WAN

**Troubleshooting VPN Installations**

**Software Solutions**

Use the Services tool or Server Manager to make sure the following services are started:

- IP Helper, Remote Access Connection Manager, IKE and AuthIP IPsec Keying Modules, Routing and Remote Access, Netlogon, Remote Access Management service, Base Filtering Engine, Windows Firewall, and Secure Socket Tunneling Protocol Service

Ensure that the Windows Firewall is set up to allow remote access

Make sure that the VPN server is enabled

Check the remote access policy to be sure that access permission is granted

Be certain that the VPN server is started

In the Routing and Remote Access tool, check the network IP parameters are correctly configured

Check to be sure the remote access policy is consistent with the users’ access needs

If only certain clients but not all are having connection problems, try these solutions:

- Make sure the clients are using the same communications protocol as the server
- If you manage access to a VPN server by using security groups, make sure that each user account or computer account that needs access is in the appropriate group

**Remote Desktop Services**

Remote Desktop Services (RDS) server

- Enables clients to run sessions-based desktops, virtual desktops, and software applications on Windows Server instead of at the client

Using session-based desktops

- The client accesses a RDS server to run applications on that server during a connection session virtual desktops
- Multiple virtual desktops can be in a pool of desktops for different purposes

Windows Server Remote Desktop Services are used for three broad purposes:

- To run applications on a server instead of the client
- To support thin clients
- To centralize program access

Thin clients

- Downsized PCs that have minimal Windows-based OSs that access a Windows Server so that most CPU-intensive operations are performed on the server
- Also used for portable field or handheld remote devices

Centralize control of how programs are used

- Highly-classified or sensitive documents can be stored and modified only on the server
- Server can be configured to provide a high level of security

**Remote Desktop Services**

RDS not only supports thin clients, but also other types of OSs including:

- Windows 10/11
- Windows Server 2016/2019/2022
- UNIX and UNIX-based X-terminals
- Linux
- Mac OS
- Tablet operating systems

Four main components enable remote desktop server connectivity:

- Windows Server multiuser Remote Desktop Services
- Remote desktop services client
- Remote Desktop Protocol (RDP)
- Remote Desktop Services administration tools

When you install RDS, you can install different role services:

- Remote Desktop Session Host
- Remote Desktop Web Access
- Remote Desktop Virtualization Host
- Remote Desktop Licensing
- Remote Desktop Gateway
- Remote Desktop Connection Broker

**Remote Desktop Services**

An RDS server employs RemoteApp

- A feature that enables a client to run an application without loading a remote desktop on the client computer
- The program appears to be just another program in a window running on the local computer

A RemoteApp program is started by clients in the following ways:

- From an icon on the client’s desktop
- From the client’s Start menu
- As a link on a website via Remote Desktop Web Access
- As an .rdp (remote desktop protocol) file

**Installing Remote Desktop Services**

When install the Remote Desktop Services role, the RDS Licensing role is also needed

- To manage the number of terminal server user licenses you have obtained from Microsoft
- The RDS Licensing role server can be installed when you install the Remote Desktop Services role
- Licenses can be purchased either per user account or by client device

When you install the RDS role, you implement Network Level Authentication (NLA)

- NLA enables authentication to take place before the RDS connection is established
- Designed to eliminate man-in-the-middle attacks

An element to consider is who will be allowed to access the RDS server

- Create groups of user accounts in advance so that you can add these groups during the installation

If operating in an Active Directory environment

- Consider creating a domain local group, such as RDS Users
- Next, create different global groups of users, such as a global group for each department that will access the RDS server
- Add the appropriate user accounts for each department’s global group
- Finally, add the global groups to the single domain local group

**Installing Remote Desktop Services**

After you have installed the RDS role and associated role services

- Use Server Manager to complete the basic configuration

Figure 9-19, Page No. 406, Complexity level: 2 Screenshot shows the “Add Roles and Features Wizard” dialog box. On the left pane of the dialog box, the Roles Services option is selected. On the content pane of the dialog box, check boxes are selected for Remote Desktop Connection Broker, Remote Desktop Session Host, and Remote Desktop Web Access options. The Install button shown below is selected.

**Installing Remote Desktop Services**

For all Windows Servers (and some Windows 10/11 editions), there are build-in Remote Desktop services

- With limited number of connections.
- With simplified configuration options.
- Run on the same Remote Desktop Protocol.

Remote Desktop Services client computers can sign in using the Remote Desktop Connection (RDC) client

- RDC client are by default installed in Windows 10 and in Windows Server 2016 onwards

The general steps to start RDC client in Windows 10/11 or Windows Server are as follows:

- Click Start and click the Windows Accessories folder
- Click Remote Desktop Connection
- Enter the name of the computer to access and click Connect
- Provide the username and password and click OK
