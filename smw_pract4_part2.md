**Secure Microsoft Windows**

**Practical 4 – Part 2**

|  |
| --- |
| ****Objectives:****<br> After completing this lab, you should be able to:  <br>(i) Install Certificate Service at a Member Server that runs on Windows Server to provide Enterprise Root CA services.<br>(ii) Download and Install Root CA Chain to local computer via Web interface.<br>(iii) Submit Certificate requests and obtain the Signed Certificate from Root CA via Web interface.  <br>(iv) Create and Configure Certificate Template for Enterprise Root CA.<br>(v) Submit Certificate request via Microsoft Management Console interface. |

Lab Prerequisites:

In this Lab, you will need up to **four virtual machines**: your Domain Controller, a new Windows 2019 **OR** Windows 2022 image (hosting cert service), the Server 1 image (hosting a web site) and the Windows 10 Workstation in your domain.

You may take note that you do not need to have the certificate server running all the time. It is only required when you need to enroll for new certificates. You can bring it down to reduce the required resources from your host notebook.

**Lab 4-6: Add role to your Server 2019 or 2022 to facilitate Enterprise Root CA (Estimated 30 minutes)**

AD CS*

PDC

http://www.clker.com/cliparts/4/a/o/k/E/Q/server-hi.pnghttp://www.clker.com/cliparts/4/a/o/k/E/Q/server-hi.png

Window Server

Enterprise Root CA with CA private key and CA certificate

\*AD CS – Active Directory Certificate Services

You will deploy an Enterprise Certificate Authority (CA) that runs on a server 2019 or 2022 to your domain. When installing the Certificate Authority role, you are required to install the IIS too.

You will generate a CA private key and have its corresponding public key to the CA certificate. Your CA will be able to issue certificates to other systems. Other systems can make requests for a new certificate by connecting to the IIS on your CA.

user 1 2019 password: Xurui123

1. Power up your Primary **Domain Controller** first.
2. Prepare and add the server 2019 or server 2022 VM into your domain.

- a. You can use the Window 2019 or Windows 2022 default image, just remember to ‘sysprep’ it and use it to configure a new VM to become your Cert Server.

     - Just make sure you choose the ‘I copy it’ option when you start the image.
     - The default local admin account is locadmin with the standard SMW password.

- b. Record down the computer name, IP address, Network Mask, Network Gateway, and DNS settings of your Server 2019 or Server 2022 before joining the domain.

     - Recommended name to be used: **SMWCA**
     - **refer to photo below**
     - Static IP: \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\__
     - Network Mask: \_\_\_\_\_\_\_\_\_\_\_\_\_\__
     - Gateway: \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\__
     - Preferred DNS: \_\_\_\_\_\_\_\_\_\_\_\_\_\__

3. After joining the domain successfully, logged into **SMWCA** with an account that belongs to **Domain Admins and Enterprise Admins** Groups.

4. Start the Server Manager and select 'Add roles and features'.

5. Select the Role-based or feature-based installation and proceed.

6. - a. When you are at the Select sever roles page, select the 'Active Directory Certificate Services', and click Next. Take the default options for the subsequent pages by clicking Next.

- b. When you are prompted to select the Role Services, choose the '**Certification Authority**’, ‘**Certification Authority Web Enrollment**’, and **'Online Responder’.** (The wizard will prompt you that other services like IIS will also need to be installed)

- c. On the Web Server (IIS) Role services page, you may enable the Basic Authentication and Centralized SSL Certificate Support Services, on top of all the default options. (If they are not automatically selected.)

- d. After reviewing all the roles/features settings, check the Restart the destination server automatically option and then proceed by clicking the 'Install' button.

7. After the role(s) have been added to the server, you still need to take care of a series of post-deployment configuration operations.

At Sever Manager > AD CS Menu:

- a. Click on the Notification Message ('more')

Navigate to the AD CS Configuration menu for the post-deployment configuration of the Certificate Service. Make sure you are using a Credential that belongs to **Domain Admins** and Enterprise **Admins** Groups to install Enterprise CA. You may need to add the account to the targeted group now (at your Domain Controller.)

Account with Enterprise Admin and Domain Admin Right

- b. For Role Services Type page, you may choose one of the **Role services** at a time**\*.** Then click 'Next' to proceed:

(* To configure one role at a time can help you better visualize the configuration requirements of each role. Some roles do not require any further input!)

- c. First with Certification Authority:

- d. At the setup Type page, choose the **‘Enterprise CA'** type.

- e. On the CA type page, choose **'Root CA'**.

- f. On the Private Key page, choose ‘**Create a new private key**’ option.

- g. Use the following setting at the Cryptography page.

- h. At the CA Name page, take all the default values.

- i. For the rest of the pages, review and take the default settings.

- j. You will then click the Configure button on the confirmation page.

.

- k. Enter Yes when the pop-up dialog prompting you to configure the remaining Role Services: You have two more role services to go: the Web Enrollment and the online Responder.

Graphical user interface, text, application, email  Description automatically generated

- l. Select the next role service and proceed:

- m. Take note that Web Enrolment Configuration does not prompt you for any options.

- n. Repeat the above to configure the next role service, 'Online Responder':

- o. Again, the Online Responder role does not require further input. That's all.

**Lab 4-7: Setting up the default Enterprise auto-enrollment policy (Estimated 10 minutes)**

Once you have your own Enterprise Root Certification Authority set up you may issue certificates to machines/services/users to support the various possible Enterprise applications that make use of PKI for the respective authentication processes.

We can set an autoenrollment policy at the domain level to automatically grant all domain machines with Enterprise issued certificates.

Let's enable auto-enrolment for all Domain machines:

1. Login to **Domain Controller** and edit your Default Domain Policy GPO.

Graphical user interface, text, application  Description automatically generated

2. Edit your Default Domain Policy GPO.

Graphical user interface, application, Word  Description automatically generated

Navigate to Computer Configuration – Policies - Windows Settings - Security Settings - Public Key Policies - Automatic Certificate Request Settings. (As shown above.)

Right click on the right-hand side panel and click on New - Automatic Certificate Request …

It will start a wizard to complete the setting.

Only select 'Computer' (a type of Policy Templates) and press Next.

Click Finish to confirm.

Graphical user interface, application, Word  Description automatically generated

That's all.

**Lab 4-8: Adding HTTPS protocol to your certsrv web site. (First Attempt) (Estimated 15 minutes)**

There is one more configuration to be carried out before proceeding to the next step:

First make a request to obtain a https certificate from your own enterprise root CA (step 1 to step 5). Then bind this cert to your CertSrv site.

(* You may have done a similar operation in Lab 4, the difference is, in Lab 4 the https is bound to a ‘self-signed certificate, here in Lab 6, the https is bind to a ‘self-issued certificate’.)

1. At your **SMWCA** server, Open the IIS Manager- Select the Server and double click on the '**Server Certificates**' to go about adding in a new server certificate:

(The above is only a sample screen shot for your reference; the detailed information will be different from your own system. For instance: the server’s name may be different)

2. On the Server Certificates page, you will see an existing self-sign certificate. This certificate is not usable for HTTPS.  You need to make use the right-hand side 'Action' menu to issue a 'Create Domain Certificate …' action:

3. At the Create Certificate page, refer to the following to fill out the properties. **The most important property** is the Common name; it **must exactly match** with your web server's Fully Qualified Domain Name (FQDN). For example: 'smwca.kitty.org'.

(The above is only a sample screen shot for your reference; the detailed information will be different from your own system. For instance: the Common name and the other input field data should be different from yours.)

4. In the next page: Online Certification Authority. You should be able to select your own server as the CA if you have successfully added the AD CS role on to it. For the friendly name, it is used to identify this certificate creation request. In a production CA, it may receive many requests, it requires a friendly name to help identify different requests and the corresponding signed certificates.   (Suggested Friendly Name to use: certSrvSSL1)

Ensure this match with your   Certificate Authority Name and with the correct ADCS Server Name. Click on the Select... button should work already.

5. Once you have pressed the Finish button. Your request should be automatically endorsed by your own ADCS, and you will see the new certificate listed on the Server Certificates page:

(The above is only a sample screen shot for your reference; the detailed information will be different from your own system.)

The two entries shown represent a 'self-signed' RootCA certificate, and a 'self-issued' https (SSL) certificate.

6. Now you can bind this newly created domain certificate to your Default Web Site by following the steps shown below.

Click Bindings.

Click the OK button to complete the new binding. Your **SMWCA** web site is now serving https connection requests.

**Lab 4-9: Installation of CA Certificate Chain (Estimated 15 minutes)**

Download and install CA certificate.

http://www.clker.com/cliparts/k/7/D/E/O/B/computer-hi.png

http://www.clker.com/cliparts/4/a/o/k/E/Q/server-hi.png

http://www.clker.com/cliparts/4/a/o/k/E/Q/server-hi.png

Windows 10

SMWCA

(Root CA)

Server1

You will now verify and re-download and re-install the CA certificate chain on to both Windows 10 and Server1 images. (* Why re-install? It is because, by default, the certificate chain of the Enterprise CA should be automatically deployed to all the domain members via the Active Directory, this exercise is for your familiarization of the certificate chain concept via going through the process manually.)

(In case you are working alone without a partner, you can turn on the two client machines (Win 10 and Server1) one at a time to reduce the system resources requirement. When all the certificates are set up properly, you can turn off the cert CA server to save the system resource)

1. Logon to the **Win 10** workstation using the domain administrator account (as you will need to install additional Trusted CA into this workstation later, and it requires admin right).
2. Search for your internet options and start.

3. Examine your current Trusted Root Certification Repository via the Internet Options Menu:

Internet Properties -> Content -> Certificates -> Trusted Root Certification Authorities:

You will see your newly created Enterprise Root CA certificate has already appeared in the Trusted Root CA repository. It is because your Win 10 is a Domain workstation, and it will automatically obtain the Enterprise Root CA certificate from the Active Directory. (If you do not see that, it is even better, as the next step is to manually download the Root CA certificate and install it onto your client system.)

4. The following steps are only serving as an exercise to illustrate how to download and install a Root CA certificate to a non-domain workstation manually.

5. Bring up a web browser at your **Windows 10** client to access the CA server with the following URL:

https://*\<FQNA your Root CA Server>*/certsrv

If you are using a modern-day browser (i.e., Edge or Chrome), you will see the warning message like the above. It is expected, as the way we created the domain certificate is not up to date. The https certificate only contains the 'Common Name' entry, but the modern browsers require you to check the 'DNS' entry, therefore, your browser will issue the warning. For now, you just click the advanced button and proceed to access the web site. We will cover how to create a proper https certificate in the later Exercise 6-6**.**

While accessing the SMWCA web interface, you may be prompted for the login credential. Depending on the type of services you need, you may need to login as a Domain Administrator, or any individual domain user, e.g., User1 or Staff1, etc. For downloading a Root CA certificate, it does not require special privilege. However, you need to be the administrator of the client system that you are going to install the downloaded Root CA certificate.

6. Click on the “Download a CA certificate, certificate chain or CRL” link. (See below)

Graphical user interface, text, application, email  Description automatically generated

7. Check that Current \[\<Your Root CA Record>] is selected and click Download CA certificate chain**.** Save the certificate to the Download Directory.

8. Do not open the downloaded file directly but go to the download folder and right click the file and choose “Install Certificate” to install the Certificate Chain to your Windows 10. Click Next.

Yes, go to the downloaded folder.

No, do not open the file directly.

9. Click the Next button to start the Certificate Import Wizard. When asked about Certificate Store, select “Place all certificates in the following store” and choose **Trusted Root Certification Authorities**.

10. Click OK, Click Next and Click Finish to complete the operation.

11. Just verify that \<Your Root CA> is still in your Certificate repository.
12. Repeat the above steps for **Server1** to verify that your **Server1** has installed \<Your Root CA> certification chain in its "Trusted Root Certification Authorities" repository.

stopped here

**Lab 4-10**: **Adding in Certificate Template to the Issuing CA\* (Estimated 15 minutes)**

*\*(In our lab setup we only use one single CA server, thus it is our root CA, and it is also our Issuing CA.)*

Set up as an Issuing CA to issue IPSec certificates to other systems.

http://www.clker.com/cliparts/4/a/o/k/E/Q/server-hi.png

SMWCA

(Root CA)

1. Logon to your **Root CA Server** with the Domain Admin Account.
2. Go to Start, then in the ‘*search*’ box type **MMC.** Press **Enter.**
3. When the MMC opens, choose **File, Add/Remove Snap-in.**
4. Locate and add in ‘Certificate Templates’. Close the Add/Remove Snap-in screen.

5. Click OK to add in the snap-in. Double-click Certificate Templates on the left side of the screen. Your screen should display all the available templates along with additional parameters, as shown in the figure below.

6. Highlight the ‘IPSec’. Right-click the template and choose Duplicate Template.

When being prompted for the Compatibility options, choose appropriate settings for the Certificate Authority and the Certificate Recipient.

For the current ST2612 Lab setup, **we should select Server 2016** for both settings.

A screenshot of a computer  AI-generated content may be incorrect.

7. On the **General Tab**, In the Template display name text box, type ‘SMW IPSec', in the Template name, type 'SMW IPSec Template'.
8. On the Issuance Requirement tab, ensure the **CA certificate manager approval** box is **NOT** checked. (This is to ensure auto-enrolment.)
9. On the Request Handling Tab, check **Allow private key to be exported** checkbox. (This is to ensure the requester can get the private key from the Issuing Server.)

10. Click **Apply**, then click **OK** to confirm the Duplicate Template operation.
11. Close the MMC window. You do not need to save the MMC settings.

12. The above steps are to create a new Certificate Template. To verify the above operations are completed correctly and to ‘enable’ the new template in your Root CA, do the following:

- a. At your **Root CA** server, at the server manager, under the Tools menu, select Certification Authority. (Should be the first or second choice in the Tools menu.)
- b. Expand your Root CA node in the left pane.
- c. At the left pane, click on the Certificate Templates to view all the current enabled templates at the main pane. Each template refers to a type of certificate the CA can issue.   User, Computer, and Web Server are the types that have been relevant to our Lab exercises.

Graphical user interface, text, application  Description automatically generated

- d. Right click on the Certification Template and choose New  Certificate Template to Issue

- e. At the next screen, select SMW IPSec template (The one you have created it previously), and click OK to enable it. Now, your certificate service will allow users/machines to enroll for an IPSec specific certificate based on this newly enabled template. You will use this template in the next Activity.

~ End of Practical 4 – Part 2 ~
