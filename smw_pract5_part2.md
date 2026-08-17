**Secure Microsoft Windows**

**Practical 5 – Part 2**

|  |
| --- |
| ****Objectives:****<br> After completing this lab, you should be able to:<br>(i) Deploy VPN service enabled with IKEv2.<br>(ii) Configure Network Policy Service. |

Deploying VPN Server on Windows Server 2019 – enabling with IKEv2

Main Reference:

[https://www.transip.eu/knowledgebase/3352-installing-vpn-server-windows-server-2019](https://www.transip.eu/knowledgebase/3352-installing-vpn-server-windows-server-2019)

Prerequisites: **4 VM** will be required here

Domain Controller /w DHCP (Your current **Domain Controller**)

Certificates Issuers (Your current **SMWCA**)

Client (Spin-up **new** Windows 10 image)

VPN server with two NICs (Spin-up new Windows 2019 or Windows 2022 image)

1. **Setting up a new member server w/ two NICs (Estimated 15 minutes)**

   - a. FQDN – ST2612VPN.soc.smw.com

   - b. NIC 1 – static address (192.168.137.86/24) at domain network range.

   - c. NIC 2 – static address (192.168.186.86/24) at a simulated public facing network range.

        - i. 192.168.186.86/24 (using Host-only VMNET)

A computer screen shot of a computer  AI-generated content may be incorrect.  A computer screen shot of a computer  AI-generated content may be incorrect.

- d. Apply sysprep if needed.
- e. Join the server to domain

2. **Installing Certificates to VPN Server and VPN client (Estimated 30 minutes)**

Define new customized certificate template at **ADCS server**:

In Certification Authority (CA), from mmc console, add the Certificate Templates Snap-in.

Duplicate the IPsec Template, with the following property adjustment:

On Request Handling tab, click Allow private key to be exported.

At Extension tab, select Application Policies and click Edit.

Remove the default "IP security IKE intermediate" Application policies and add "Server Authentication" Application policies.

Edit the Key Usage Extension, ensure that Digital signature is selected. If it is, click Cancel. If it is not, select it, and then click OK.

In the security tab, ensure Domain Computers are included in the Group or usernames section.

Make sure Read, Enroll and Auto-Enroll are selected.

In General tab give template, a name, e.g., VPN.

Finally, bring up the Certification Authority management console and issue the newly created template.

Now, the CA is ready to issue out certificates based on the VPN template.

Enrolling VPN certificate on VPN server.

Login as domain administrator at the VPN server, bring up the MMC with Certificate (Computer Account) Snap-in.

At the console, right click Personal and select All Tasks to Request New Certificate ….

A screenshot of a computer screen  AI-generated content may be incorrect.

At the Certificate Enrollment page, select the VPN Enrollment Policy entry.

Click the Details column to reveal the Properties button.

Click on the Properties button to modify some of the properties:

At the subject tab, add in the Common name and Alternative name properties your vpn server FQDN, you may then click Apply and OK follow by Enroll to complete the enrollment.

Export the VPN certificate and install it to the designated VPN client machine.

Continue from the previous step. You will see the newly enrolled VPN certificate on the main pane. Right click on it and select the Export action.

You need to select the option of exporting the certificate with its private key, to protect the key from unauthorized access, you need to provide a 'strong' password in the later step.

See setting below on the Export File Format page.

Provide a 'strong' password in the Security page.

A screenshot of a login box  AI-generated content may be incorrect.

Finally, save the exported certificate to a specific file.

Now you can copy the file to the client workstation, login as administrative account and right click at the file to install the certificate into the client workstation as a "Trusted Root Certificate Authority".

At Client's MMC with certificate snap-in:

A screenshot of a computer  AI-generated content may be incorrect.

3. **Add required roles to the VPN server (Estimated 15 minutes)**

On VPN server, add Network Policy Server and Remote Access roles.

At select role services – check the 'DirectAccess and VPN(RA)' and 'Routing'.

Click next(s) until the installation begins. It will take a while to complete. You may also have observed that the IIS (Web server) role is also included. It is part of the DirectAccess role. We do not use it for VPN server only mode though.

**Post-deployment Configuration**

You will be prompted to complete the deployment of the new roles.

In our case, select the Deploy VPN Only option.

The Wizard will bring up the Routing and Remote Access console; you can right click the VPN server node and use Configure and … to proceed. (It is another wizard-oriented setting tool)

Choose Remote access (dial up or VPN) configuration.

Opt for ‘VPN’ access.

Select the interface that faces the simulated internet side.

A screenshot of a computer  AI-generated content may be incorrect.

The next page is to prompt for using existing DHCPD or using static range of IP assignment for remote clients' DHCP requests. In our case we select 'Automatically'.

Select ‘No’ on the following page:

Verify and click finish to proceed.

4. **Confirmation of the DHCP relay setting (Estimated 15 minutes)**

At the routing and remote access console, right click at the VPN server node and select properties:

At the IPV4 tab, ensure the Adapter for the name resolution is selected correctly, i.e. The adapter in your local domain range.

In my case, it is Ethernet 0:

**Network Policy Service Configuration**.

At this point, the Remote access service is running, however, the Network Policy Service has not been configured yet, thus, no remote access request will be allowed.

At the routing and remote access console, right click at Remote Access Logging and Polices, select launch NPS:

On the Network Policy Server console, click on Network Policies. You can see the two default policies at the main pane are both 'disabled' (red crosses).

We need to configure and enable the first policy, "Connection to Microsoft ……". Right click on it and select Properties.

At Overview tab, select Grant Access, ignore user account dial in properties and set the server type to VPN-Dialup.

At condition tab, add in the User Groups condition to allow all Domain User to connect.

At constraint tab, select the authentication setting based on the following:

Click Apply and OK will bring you back to the main page:

You can see the first policy is now enabled; it will grant access to the remote clients if they can pass the authentication requirement.

5. **For the Client Configuration (Estimated 30 minutes)**

Client must be in the subnet and assign with IP address where the VPN server is externally facing

A screenshot of a computer  AI-generated content may be incorrect.

How many certificates does it need?

- a. The VPN Server certificate, install to the Trusted Root Authority

**This probably applies to the clients that is not part of the domain. But the user has a valid domain account.**

Or

- b. The Root CA cert of the issuer that issue the VPN Server certificate.

**This probably apply to the clients that already joined in the same domain.**

Host name setting at the Client.

The client needs to access the VPN Server using the 'common name' that the VPN server has been using to enroll for its certificate. We may need to update the client's hosts file to specify this name and ip address.

At the Client PC, start a new session of notepad.exe (run as administrator), open the hosts file at the Windows-System32-driver-etc folder.

Add in the required entry:

A screenshot of a computer  AI-generated content may be incorrect.

Create a VPN connection on the Client PC:

Take note that your client must have an IP that is in the same subnet of the VPN Server external facing

Login as the administrator at the client PC, bring up the Network and Sharing Center, select the 'Set up a new connection or network' action:

Choose 'Connect to a workplace' and click the 'Next' button.

In the next page, choose 'Use my Internet connection (VPN)

Choose 'I'll set up an Internet connection later'

In the final page, specify the correct vpn domain name, destination name (for display) and the connection options. Click ‘Create’ to confirm.

A screenshot of a computer  AI-generated content may be incorrect.

The vpn connection should be created, and you will be prompted:

By default, this new connection will try with all the VPN protocols (PPTP, L2TP, SSTP and IKEv2) with the default security setting associated to make connection to the VPN server.

To ensure the client 'only' uses the IKEv2 protocol, you need to update the 'properties' of the 'connection'. To do this, go to the Network Connection Panel, right click at the smw VPN Connection to bring out its properties menu.

At the Security tab, specify the VPN type to IKEv2, and the Authentication Protocol based on the following screen shot setting.

6. **Connecting to the VPN server using IKEv2 protocol (Estimated 15 minutes)**

At the Network connection panel, right click at the swm VPN connection and select Connect/Disconnect:

It will bring up the Network connection menu, click on the smw VPN Connection to display the Connect button, the click on the button.

A screenshot of a computer  AI-generated content may be incorrect.

At the sign in panel, you can sign in with any valid domain user account:

A screenshot of a computer  AI-generated content may be incorrect.

If the connection is successful, you will see the following:

A computer screen shot of a computer  AI-generated content may be incorrect.

That's it, you may now try to see if you are in the Domain network (i.e., check your ip address, check if you can surf the internet etc.)

A screenshot of a computer  AI-generated content may be incorrect.

7. **Final remote client status check at the server end (Estimated 10 minutes)**

While your client is connected to the VPN server, you can check for its status on the remote access management console:

~ The end ~
