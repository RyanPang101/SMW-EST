**Secure Microsoft Windows**

**Practical 1**

|  |
| --- |
| ****Objectives:****<br> After completing this lab, you should be able to:<br>(i) Maintain and personalize your own Windows Server 2022 and Windows 10 VMWare Images in the Lab setup.<br>(ii) Use Server Manager to perform “Add Features” and “Add Roles” functions.<br>(iii) Set up a simple Domain Controller Server with one client workstation.<br>(iv) Set up the DNS Service and DHCP Service in the Primary Domain Controller<br>(v) Using the Best Practices Analyzer to check the configuration.<br>(vi) Create and configure Organization Units within a Domain.<br>(vii) Carry out basic domain user account management.<br>(viii) Understand the basic operations of security groups operations.<br>(ix) Create Fine-Grained Password Policy with Active Directory Administrative Center |

Lab Prerequisites:

- Take note that due to VMWare access right issue, you cannot use the Lab PC to do your practical.
- You need to download the VM images to your laptop to proceed.
- **Keep a copy of your original download, as you will need to use them to clone more VMs in future labs.**
- For the VMware software, you are recommended to use the latest VMware workstation.
- When you are prompted while powering on a newly copied windows VM.

Please answer with 'I Copied It'.

- **Be reminded to record down all the important configuration information of your VMs.**

  - **Passwords, IP configuration, Domain Name, Machine Name, etc**.

**Lab 1-1: Understanding VM Virtual Network Configuration (Estimated 15 minutes)**

1. Start VMware Workstation. Go to Edit, Virtual Network Editor.
2. Select VMnet8, which is the NAT virtual network adapter. Please **screenshot**
3. Paste **screenshot** below

4. Take note of the Subnet IP. When you start up a VMware image with NAT, VMware will assign an IP address from this range to the image.

**- Check the 'Use local DHCP service option to distribute IP address to VMs.**

**- Once the initial testing is completed, and you are having your own DHCP Service running, 'MUST' unchecked this option. (All your SMW lab client VMs should use DHCP service offered by your Primary Domain Controller (PDC), all your SMW lab server VMs should use appropriate static IP settings)**

Domain: smw.soc.com

**Windows 10 (Client)**

**Windows Server 2022**

**(DC) with ADDS, DHCP and DNS.**

You will be setting up two images. Both images will be part of a domain.

Windows Server 2022 – the Primary Domain Controller (PDC) and it runs the DNS Service and DHCP Service.

Windows 10 client

5. Check that the two images in Lab 1 folder (w2022ms.zip and w10.zip) are using NAT.

6. Check the processor settings of the two images in Lab 1 folder (win2k22std and win10) to have '2' virtual processors.

7. Power on both images.

8. For the Windows Server 2022 image, to login => (user id: locadmin password: 1qwer$#@!)

9. For the Windows 10 image, the local user account locadmin is the Administrator and the password is also 1qwer$#@!

10. On both images, verify and set the date, time and timezone correctly.

11. On both images, **initially**, set the IP address and DNS Server address to be obtained automatically (using DHCP).

12. On both images, check that you can access the Internet.

13. **At this point, for the Windows 2022 Server images, modify the IP address, Gateway address and DNS Server to a static value.**

    - You decide the static IP address values (should be in same NAT subnet).
    - Hints, you can use the existing IP settings, except you need to change IP address value.

14. **Verify again that both images can access the Internet.**

15. Record down your Windows Server 2022 IP settings now:

IP Address: \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\__

Network Mask: \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\__

Gateway: \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\__

Preferred DNS Server: \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\__

Alternate DNS Server: (can leave it blank)

16. On both images, try to disable the Automatic Windows Update feature (this should be enabled for production systems, but we are disabling it just for a better performance during practical time). However, it may be relatively difficult to figure out how to disable the Server 2022/Windows 10 automatic update feature. If it is so, proceed without changing the setting first.

Caveat: Although we do not want to receive a sudden windows update notification during practical time it is good to keep your lab VM images up to date. Thus, you should manually check for updates during the weekends. Failing to keep your VM image up to date may cause issues when working on future lab exercises.

17. On both images, change the computer name. Right-click on the Windows button and select System. Under the System menu click Change settings. Click on Change and set a new computer name to your preference (do some research if unable to find change computer name option). You will have to restart the image after changing the computer name.

Caveat: As we will clone the same VM to create more windows machines in your domain, ensuring every VM has its unique and meaningful computer name is one of the best practices in this module.

Reflection Prompt: Other than the ipconfig /all command, what are the other ways to check the network settings in a Windows system?

**Lab 1-2: Setting up your own Domain (Estimated 30 minutes)**

You will now install **Active Directory Domain Services** on your Windows Server 2022 and make it a Domain Controller. As the IP address of the Domain Controller should not be changing, please ensure that you have changed the IP address setting of the Windows Server 2022 to static.

**For this step, you do not need your Windows 10 VM to be active, you may shut it down to reduce the loading of your notebook.**

1. On **Windows Server 2022**, check out your network status details and take note of the current IP, Subnet Mask, Gateway IP, and DNS Server IP.
2. Ensure you have disable the VM Net NAT Local DHCP service.
3. Now verify your Windows Server 2022 to a set of workable static values:

IP address: \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\__

Net mask: \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\__

Gateway: \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\__

Preferred DNS: \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\__

4. Check that you can still access the Internet.

5. To promote your Windows server to become a domain controller, add the Active Directory Domain Services role to your server.

   - To add the role: At Server Manager -> Local Server -> Manage -> Add Roles and Features
   - Click ‘Next’ accept all default selection until reach Server Roles
   - Select Active Directory Domain Services -> Click Add Features -> Click Next accept all default selection until reach Confirmation -> Click Install

6. After the installation process is completed, remember to click on the "Promote this server to a domain controller" to proceed to the additional steps

7. Refer to the following to set up your own domain:

- (a) Deployment Configuration:

      - a. Add a new forest.
      - b. Root domain name (fully qualified domain name FQDN), you can use “smw.soc.com” or any other domain name you like, e.g., “mydomain.org”. **Please avoid using a single word.**

- (b) Domain Controller Option:

      - a. Use **Windows Server 2012** for both Forest Functional level, and the Domain Functional level. You will change them to a higher level in Lab 2.
      - b. Specify the DNS and the Global Catalog (GC) capabilities.
      - c. For the Directory Services Restore password, you can use: !QWER4321

- (c) DNS Options

      - a. You do not need to create DNS delegation as you are setting up a Primary Domain Controller.

- (d) Additional Options

      - a. Take the default for the NetBIOS domain name.

- (e) Paths: Take the default values.

- (f) Review Options – Examine and verify the options. To proceed, click the next button.

      - a. You may try the view script option to learn how to use PowerShell script to install a domain controller.

- (g) Prerequisites Check: (You may proceed if the check is successful, press the install button)

8. When the installation is complete, you will need to restart your server, or it will automatically restart if you have opted for the 'auto-restart if needed' option at the very beginning.

**Lab 1-3: Exploring your Domain Controller (Estimated 15 minutes)**

1. You can login to the server using the same user id: locadmin and the same password: 1qwer$#@!

   - Take note that locadmin is now promoted to a Domain User account with Domain administrative right.
   - **(Very important)** If you are prompted to change your password, please record down the new password!!!

2. At the Server Manager -> Local Server, explore the properties page:

Click on the Blue color link and explore their functions.

Reflection prompt: Can you identify at least 5 clickable elements in the Server Manager Local Server Properties page that are related to the Windows Security measures? I.e., any misconfigurations / misuse of these will cause higher risk of security implications.

3. At the Server Manager -> Local Server -> Properties

Click on your Ethernet0 IP address to bring up the Network Connections Window.

From there, bring up your current IPV4 network settings Properties Window. Take note that your preferred DNS is now set to 127.0.01. It is because your server itself is providing DNS service now.

4. Add your own Primary Domain Controller static IP address as the Preferred DNS server now. This will help to clear the false alarm of 'Not Connected' notification showing at the network connection icon.
5. At this point, try to access the web again to verify your current network settings are correct.
6. At the Server Manager -> Local Server -> Tools, explore the following tools:

- Active Directory Users and Computers

  - Do quick research on renaming Active Directory default domain account.
  - **(Very Important)** Renamed locadmin to **dcadmin** when accessing this tool and ensure that you can login using **dcadmin**.

- Services

- System Information

- Windows Firewall with Advanced Security

Reflection Prompt: Try to run the Group Policy Management Console. From the console, can you find out what is the forest name and domain name of your system?

**Lab 1-4 Adding DHCP service to your Domain Controller (Estimated 20 minutes)**

1. Since you are going to run your own DHCP Server for the NAT segment and you have disabled the VMWARE Network Local DHCP service (Refer to Lab 1-2, Step 2), there is no DHCP service running in your LAN segment.
2. You need to login to your Domain Controller with an account that is a member of domain administrator group.
3. At the Server Manager -> Local Server, use Add Role feature to add in the DHCP Server role:
4. You may accept all the default settings and click the next button until the installation is completed.
5. After the role and features are added, you still need to complete the required **post-deployment** configurations.
6. Again, you can accept all the defaults to complete the post-deployment configuration.
7. The DHCP Scope configuration: The scope definition determines the information to be sent to all connected DHCP clients. You need to start the DHCP Manager to configure the scope.

Access to the DHCP Manager via Server Manager:

- - a. Adding an active IPv4 scope to provide dynamic address allocation services.

- Right click on the IPv4 node and select 'New Scope'.
- Refer to the following to complete the configuration guided the New Scope Wizard.

(The IP address range needs to be adjusted to match your VMNet NAT segment settings)

At Add Exclusions and Delay – leave it blank and Click Next to proceed.

Also takes the default settings until the Router (Default Gateway) screen: You must enter your VMNet NAT gateway address: in this example, it is 192.168.137.2.

In the Domain Name and DNS Servers screen. Double check if the domain name and DNS server address (It should be your PDC address.) then click the Next button.

You can skip the next WINS Servers screen and reach the following Activate Scope screen: You may select Yes and click the Next button to complete the scope configuration.

- b. You may now verify your scope configuration at the DHCP Manager Console:

- c. You may now close the DHCP Manager console and proceed to the next exercise.

**Lab 1-5 Joining a client to your domain (Estimated 15 minutes)**

You will now configure your **Windows 10 VM** to join your domain.

1. Ensure your Window Server 2022 (Primary Domain Controller) is up and running, the local DHCP service of the VMNet has been disable.
2. Start your **Windows 10** VM (also connect to NAT segment)
3. Verify the IP settings of the Windows 10 VM remains dynamic.
4. Check that you can still access the Internet.
5. Verify and record down the IP address, Gateway, and DNS settings of Windows 10 by using the GUI approach.
6. At Network Connections->Right Click on the Ethernet0->Click Status->Click Details.
7. Close the Network Connections and its popups. Right-click on Windows Menu and select 'System'.
8. At the system settings page click on 'Rename this PC (advanced)' to bring up the System Propertied Popup. Then click on the ‘Change’ button to bring up the system properties popup.
9. Click on the Change button to bring up the Computer Name/Domain Changes popup.
10. Update your computer name as **client**. Select Domain. Enter your domain name, e.g., smw.soc.com. Click OK

A screenshot of a computer  AI-generated content may be incorrect.

11. You will be prompted to enter the Administrator and password for your Windows 10 to join the domain. You may provide the dcadmin credential to proceed.
12. Your windows 10 will be restarted at this point to complete the join domain operation.

**Lab 1-6 Logging onto Windows 10 with local and domain administrator (Estimated 10 minutes)**

1. Login to **Windows 10** with the local administrator account by using “.\\locadmin” as the username. A local user account exists only for that system locally. You cannot log in with this account on to your Windows Server 2022.
2. Login to **Windows 10** with the domain administrator account by using “dcadmin@\<your domain name>” (e.g., st2612@smw.soc.com) as the username. A domain account exists on all computers in the domain. You can use the domain Administrator account to login to both Windows 10 and Windows Server 2022 as they are both in the domain.

Reflection Prompts:

What is the main advantage of allowing a user account to login to all the computers in the domain?

**Lab 1-7: Using Best Practices Analyzer (BPA) to check your Domain Controller (Estimated 15 minutes)**

1. Login to your **Domain Controller**

2. The steps for running BPA are:

   1. Open Server Manager.
   2. Click Local Server in the left pane.
   3. In the right pane, scroll to Best Practices Analyzer.
   4. Click the down arrow for TASKS.
   5. Click Start BPA Scan and wait for the scan to complete.

3. Review the result of the scanning and try your best to fix the sited issues.

- Please put some effort into this exercise. To fix at least one or two issues.

After you resolve the first warning issue and rerun the process, the report shows:

Reflection Prompt: How many issues has the BPA reported? How many of them have been fixed by you? Don’t need to be too worried if there are many issues that cannot be fixed.

When the BPA finds problems, you will see a level of severity as well as a category within the results. There are three levels of severity:

- **Information** - The role complies, but a change is recommended. For example, a server’s network interface might have a valid IPv4 address temporarily leased by DHCP, but a static address is recommended for the role.
- **Warning** - The role complies under current operating conditions, but this may change if the operating conditions change. For example, the Hyper-V role might become noncompliant if another virtual machine is added for which there is no available virtual disk space.
- **Error** - The role does not meet best practices, and problems can be expected.

The different categories that you will see for BPA recommendations include:

- **Configuration** - Indicates whether role settings are configured for best performance and avoid conflicts with other services
- **Pre-deployment** - Indicates whether prerequisites for the role are properly installed or configured
- **Post-deployment** - Indicates whether services needed for the role are started and running
- **Performance** - Indicates whether the role can perform the tasks for which it is intended on time and adequately for the intended workload
- **BPA Prerequisites** - Indicates whether the role is set up in such a way that BPA can analyze the role. Failure here simply means that a component or setting prevented the BPA from properly analyzing the role.

Reference: [https://docs.microsoft.com/en-us/windows-server/administration/server-manager/run-best-practices-analyzer-scans-and-manage-scan-results](https://docs.microsoft.com/en-us/windows-server/administration/server-manager/run-best-practices-analyzer-scans-and-manage-scan-results)

**Lab 1-8: Viewing Firewall Rules (At your Domain Controller) (Estimated 10 minutes)**

1. Left click the Windows button, expand the Windows Administrative Tools, and click on the Windows Firewall with Advanced Security.
2. Look at the Overview pane. Which Firewall profile is currently active?

Reflection Prompt: Do quick self-research and figure out what is the purpose of these 3 Profiles for? And identify the unique different attributes of Domain Profile and Private Profile.

3. Click on Inbound Rules. Can you find the rules that allow clients to connect to your DNS Server? Which ports is the DNS Service running on? Is there any restriction on which client IP address can connect to your DNS Server?
4. Can you find the rule that allows other systems to ping your server?

**Lab 1-9: Raising the Forest and Domain Functional Level (Estimated 10 minutes)**

In the previous practical, you had set the Domain Functional Level. You will now raise it to Windows Server 2016. (You can raise the Functional Level, but you cannot lower it after that)

1. Raise Domain Functional Level to Windows 2016.

1. In Server Manager, click Tools and click Active Directory Domains and Trusts.
2. In the tree in the left pane, right-click the domain name (e.g., smw.soc.com).
3. Click Raise Domain Functional Level.

4. Select a functional level. (Select Windows Server 2016) (If currently in the Windows Server 2016 domain functional level and all you can do is click Cancel and go to Step 8)
5. Select the appropriate forest functional level and click Raise.
6. Read the message box and click OK.
7. Read the acknowledgement box and click OK.
8. Close the Active Directory Domains and Trusts window.

2. Verify the Forest Functional Level has also been raised to Windows Server 2016.

1. In Server Manager, click Tools and click Active Directory Domains and Trusts.
2. In the tree in the left pane, right-click Active Directory Domains and Trusts.
3. Click Raise Forest Functional Level.

4. Select a functional level. (Select Windows Server 2016) (If currently in the Windows Server 2016 forest functional level and all you can do is click Cancel and go to Step 8)
5. Select the appropriate forest functional level and click Raise.
6. Read the message box and click OK.
7. Read the acknowledgement box and click OK.
8. Close the Active Directory Domains and Trusts window.

Reflection Prompt: Search and discuss with your classmates to figure out why windows provide this Raising Domain Functional Level feature?

**Lab 1-10: Creating Domain User Accounts in Active Directory (Estimated 20 minutes)**

You will now proceed to create **THREE** domain user accounts on your Domain Controller. Domain users can login to any computer in the domain, except for the domain controller.

- Give your 3 users the following Full Names and User logon names: User1, Staff1 and Mgr1
- For passwords, you can use: 1qwer$#@!

1. Right-click Start, click Run, type mmc in the Open box, and click OK. Maximize the console window, if necessary. Click the File menu and click Add/Remove Snap-in. Under the Available snap-ins, click Active Directory Users and Computers and click Add. Click OK.
2. In the left pane, click the right pointing arrow in front of Active Directory Users and Computers, if necessary, to display the elements under it. Click the right pointing arrow in front of the domain name, such as st2612.com, to display the folders and OUs under it.
3. Click the Users folder in the left pane and then in the middle pane, view the user accounts (with one head icons) and groups (with two head icons) already created.
4. Click the Action menu or right-click Users in the left pane, point to New, and click User.
5. Follow the example as shown in the picture below.

A screenshot of a computer  AI-generated content may be incorrect.

6. Click Next.
7. Enter a password and enter the password confirmation. Checked the box **User must change password at next logon**. This will force users to enter a new password the first time they sign in. RESEARCH what the other options are for.
8. Click Next.
9. Verify the information you have entered and click Finish.
10. To continue configuring the account, in the middle pane, double-click the account you just created, such as User1.
11. Notice the tabs that are displayed for the account properties.
12. Click the General tab, if it is not already displayed, and enter a description of the account, such as Sample account.
13. Click the Account tab to view the information you can enter on it.
14. Click the tabs you have not yet viewed to find out what information can be configured through each one.
15. Click OK.
16. Start up your Windows 10 image. Click on Switch User. Login as one of the 3 domain user accounts you just created. When asked to change password, you can use !QWER4321.

Reflection Prompt: We usually consider it good practice to ask a user to change his password upon the first successful logon session. Why is it so?

**Lab 1-11: Group Policy controlling access to the Domain Controller (Estimated 10 minutes)**

As the Domain Controller is a very important server, physical access to it should be restricted. Moreover, direct logon to the Domain Controller should also be restricted. In fact, by default, there are only a handful of user accounts are allowed to do so.

1. On the **Domain Controller**, try to login as any one of the 3 domain user accounts just created. You should not be successful. (This is an example of security by default!)
2. Login as the Domain Administrator.
3. Under Administrative Tools, run Group Policy Management. Expand your domain. Under Domain Controllers, there is a **Default Domain Controller Policy**. This is the policy that controls all Domain Controllers in your domain. (Note: **DO NOT** select the Default Domain Policy instead)
4. Right-click on Default Domain Controller Policy and select Edit.
5. Expand Computer Configuration, Policies, Windows Settings, Security Settings, Local Policies. Click on User Rights Assignments.
6. In the right-hand pane, double-click on Allow log on locally. Are normal users among the groups who are allowed to log on locally on Domain Controllers?
7. Click on Add User or Group. Click Browse.
8. Type “Mgr1” and click Check Names. Click OK. Click OK. Click OK.
9. Try to login as Mgr1 on the Domain Controller. You should be successful now.

**Lab 1-12: Granting Administrative Rights to user account by security group member. (Estimated 10 minutes)**

1. Create another domain user account (give any name you like) and make him/her a member of the Domain Administrators group.

2. Try to use the newly created account to log on to the domain controller directly to prove it has administrative rights.

Reflection Prompt: Can you suggest any additional ways to prove the new account has the domain administrative right?

**Lab 1-13: Managing Organizational Units (OU) (Estimated 15 minutes)**

1. On your **Domain Controller**, do the exercise with the following additional requirements:

- Create a new OU (Organization Unit) called salesOU for your domain.
- Move the following Directory Objects into this salesOU

1. In Server Manager, click Tools, and click Active Directory Users and Computers.
2. Right-click the top domain in the tree in the left pane, such as smw.soc.com, point to New, and click Organizational Unit.
3. Enter salesOU and your initials, such as salesOU. Click OK.
4. Click the arrow in front of the domain in the left pane so that you can see the OU you created listed under the domain.
5. Move the **User1** and **Staff1** user account objects, and your **Windows 10 computer object** into salesOU. Click Yes if you see a warning message about moving objects in Active Directory

2. Try out the delegate control wizard at salesOU. Delegate the control of salesOU to the Mgr1 account (only with Reset user passwords and force password change at next logon control)

1. Right-click the OU, such as salesOU
2. Click Delegate Control.
3. Click Next when the Delegation of Control Wizard starts.
4. Click Add.
5. Click the Advanced button.
6. Click Find Now.
7. Click OK.
8. Click OK in the Select Users, Computers, or Groups dialog box.
9. Click Next in the Delegation of Control Wizard.
10. Click the box for Reset user passwords and force password change at next logon, as shown in figure below.

Lab2_delegate.JPG

11. Click Next.
12. Review the tasks that you have completed and then click Finish.
13. Close the Active Directory Users and Computers window.

The above should enable the Mgr1 to reset passwords for users who are in the Sales OU.

Please carry out a quick test to verify it.

**Lab 1-14 Create Domain Local and Global Security Groups (Estimated 15 minutes)**

1. Login as the **Domain Administrator** on

   - Your client Windows image.
   - Or your Domain Controller (if your RSAT tools are not working in your client Windows workstation.)

2. Run Active Directory Users and Computers consoles (or you may try to run mmc and add the Snap-in).

3. Create a Domain Local Group: DomainMgrs and a Global Security Group: GlobalMgrs

   - For Step 17, select only one user accounts Mgr1.
   - For Step 19, verify that only Mgr1 is in the Member tab of the GlobalMgrs group.

   - Note: You need to use the DomainMgrs and GlobalMgrs security groups created in this exercise in the next practical exercise.

1\. Access the MMC console window for Active Directory Users and Computers that you have been using or click its icon on the desktop if you have saved it to the desktop. Alternatively, you can use the Tools menu in Server Manager to open Active Directory Users and Computers.

2\. In the tree on the left pane, display the contents under the domain, such as st2612.com.

3\. Click Users in the tree.

4\. Click the Action menu, point to New, and click Group.

5\. In the Group name box, enter DomainMgrs plus your initials, for example, DomainMgrs.

6\. Click Domain local under Group scope, and click Security (if it is not already selected) under

Group type.

7\. Click OK and then look for the group you just created in the right pane within the Users folder.

8\. Click the Create a new group in the current container icon on the button bar (with two heads).

9\. In the Group name box, type GlobalMgrs plus your initials, for example, GlobalMgrs.

10\. Ensure Global is selected under Group scope and that Security is selected under Group type.

11\. Click OK and then look for the group you just created on the right pane.

12\. Double-click the global group you created.

13\. Click the Members tab. Notice that no members are currently associated with this group.

14\. Click the Add button.

15\. Click the Advanced button in the Select Users, Contacts, Computers, Service Accounts, or

Groups dialog box.

16\. Click Find Now.

17\. Click the first user provided by your instructor, press, and hold down the CTRL key and click the second user provided by your instructor. Click OK.

18\. Make sure that the users you selected are shown in Select Users, Contacts, Computers,

Service Accounts, or Groups dialog box. Click OK.

19\. Again, be sure that the accounts are shown in the global group’s Properties box on the Members tab. Click OK.

20\. Double-click the domain local group, such as DomainMgrs, and then click the Members tab.

21\. Click Add.

22\. Click Advanced in the Select Users, Contacts, Computers, Service Accounts, or Groups dialog box.

23\. Click Find Now.

24\. Locate the global group you created, such as GlobalMgrs. Click that global group and click OK.

25\. Verify that the global group is displayed in the Select Users, Contacts, Computers, Service Accounts, or Groups dialog box, and then click OK.

26\. Make sure the global group is listed under Members on the Members tab. Click OK.

27\. Close the MMC console window and click Yes to save the console settings if you have not done this previously. If you are saving the settings, click Desktop in the left portion of the dialog box. Enter a name for the console, such as Manage Accounts, and click Save.

**Lab 1-15: Fine-Grained Password Policies and Active Directory Administrative Center (Estimated 20 minutes)**

Fine-Grained Password Policies (FGPP) allow you to create multiple password policies for specific **users** or groups. Multiple password policies are available starting with the Windows Server 2008 version of Active Directory. In previous versions of AD, you could create only one password policy per domain (using the Default Domain Policy). Starting with Windows Server 2008, administrators can use multiple GPOs or FGPP to manage multiple password policies. In this exercise, we explore the FGPP approach. Administrators can create multiple FGPPs in Password Setting Objects (PSO). Individual PSO can be associated with a **global** security group to set the password policy requirement for the members of the group.

There are different ways to create the PSO. In this exercise, you will explore and use the Active Directory Administrative Center (ADAC) to create a testing PSO and associate it with GlobalMgrs security group.

For this testing PSO, you can set the password requirements (length=6, complexity=No, history=3) and account lockout option = None.

Please refer to the following video for the usage of ADAC to accomplish the task.

*"How To Manage Fine-Grained Password Policies in Active Directory"*

Verify your configuration with the Mgr1 account, that is: Log in as Mgr1 and try to change the password.

References:

1. Fine-Grained Password Policy in Active Directory - [https://thesleepyadmins.com/2024/08/30/active-directory-fine-grained-password-policy/](https://thesleepyadmins.com/2024/08/30/active-directory-fine-grained-password-policy/)
2. Introduction to Active Directory Administrative Center Enhancements (Level 100) - [https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-#bkmk_create_fgpp](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-)

~ End of Practical 1 ~
