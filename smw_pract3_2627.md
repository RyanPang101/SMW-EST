**Secure Microsoft Windows**

**Practical 3**

|  |
| --- |
| ****Objectives:****<br> After completing this lab, you should be able to:<br>(i) Basic configurations of member server with the IIS role to publish web content. <br>(ii) Understand the basic usage of Microsoft ‘Security Compliance Manager’.<br>(iii) Import and apply standard GPO baseline for Security Policies management.<br>(iv) Modify GPO settings for selected computers and user configurations.<br>(v) Use gpupdate for immediate domain GPO deployment.<br>(vi) Use RSoP to verify Group Policy Application Order<br>(vii) Use CIS Benchmark hardening via GPO |

Lab Prerequisites:

Completion of the Lab exercises on Practical 2 and you have **Server1** joined to your domain.

You will have properly configured the **Windows Server 2019** (called **Server1**) created in Practical 2 to your domain. This server will be a Member Server (a Member Server is any server in the domain that is not a domain controller).

**Lab 3-1: Installation of Internet Information Services (IIS) – Web Server (Estimated 30 minutes)**

1. On **Server1**, login as the Domain Administrator, install the IIS (Web Server).

**Take all the default services but ensure you have selected Windows Authentication under the security settings.**

After you have the IIS added to Server1, proceed with the following exercises:

2. Go to C drive. You will see a new folder “inetpub” has been created. In the inetpub folder, a folder called “wwwroot” has been created. This is the default folder for the web pages.
3. Start Internet Explorer at Server1 and browse to http://127.0.0.1. You will see the default start page.

- From your **Win 10** to browse the same page via [http://server1](http://server1)

  - If your Window Server 2019 computer name is server1

4. You will now create a virtual directory in c:\\MyWebsite.
5. Create a new folder C:\\MyWebsite.
6. Create a file called “index.htm” in C:\\MyWebsite. Edit the file to have the following content:

**(Note: do not mistakenly create the file index.htm.txt instead. You need to show file extension at your file explorer.)**

This is my website!

7. Start the IIS Manage via the Server Manager (You may be prompted to install the optional web platform component. You do not need it for this practical.).

Add a virtual directory that points to the Physical path C:\\MyWebsite to your Default Web Site Node:

Use webdoc as the alias and take the default pass-through authentication settings.

8. Start Internet Explorer and browse to http://127.0.0.1/webdoc. You will see your index.htm.

9. Create a new folder C:\\inetpub\\wwwroot\\userlist.
10. Create a file called “users.htm” in C:\\inetpub\\wwwroot\\userlist. Edit the file to have the following content:

List of users

11. Try to browse this new htm file using the full path:  http://\<your server1 name>/userlist/users.htm

12. Try to only access to http://\<server>/userlist : You may get the following error message.

13. Explore on your own to enable Directory Browsing feature for the userlist folder let the user browse the folder via the web page:

14. It is generally **NOT** a good security practice to allow users to view a directory listing of your web server, so disable Directory Browsing.

15. Try accessing your Webdoc page from both your Domain Controller (By now you should have learned how to disable the ‘Enhanced IE Security configuration’ at the DC) and Windows 10 image. You can browse to http://Server1/Webdoc or http://*server1\_ipaddr*/Webdoc.

Question:  How can you restrict this website to only your Windows 10 system? That means your Domain Controller is not allowed to access the website.

Hint:  Using the Windows Firewall on Server1 is one solution.

Question: How can you restrict this website to only clients from the smw.com domain? This means the client from pc.smw.com can access the website but the client from pc.soc.org cannot access it. (You can verify your answer by using your host computer to surf server1)

Hint: On **Server1**, go to Server Manager. Under Roles Summary, click on Web Server->Security, check IP and Domain Restrictions.

(You may need to restart the Server1).

In Internet Information Services Manager, click on your website. In the middle pane, double-click on IPv4 Address and Domain Restrictions. Explore this feature. You can use this instead of Windows Firewall to restrict access to your website based on IP addresses or domain names.

**Lab 3-2: IIS Basic Authentication and Windows Authentication (Estimated 15 minutes)**

1. On **Server1**, create a new folder C:\\MyWebsite\\admin.
2. Create a file called “index.htm” in C:\\MyWebsite\\admin. Edit the file to have the following content:

This is my admin page.

Only admin can view it.

3. Use Advanced File Permission setting to enable Mgr1 to read this file and do not allow another domain user to read it.
4. Try to access this page via: http://server1/webdoc/admin/ from Win 10.

You should not be able to do so even if you login to Win 10 as Mgr1.

5. Go to Server Manager. Under Roles Summary, click on Web Server. In the right-hand side pane, click Add Role Services.

6. Under Security, enable the Basic Authentication and Windows Authentication options. Click Next. Click Install. (You may have checked it already)

7. In IIS Manager, expand Webdoc. Select the admin folder. In the middle pane, double click on Authentication. Right-click on Anonymous Authentication and choose Disable. Right-click on Basic Authentication and choose Enable.

8. On **Windows 10**, browse to http://Server1/WebDoc/admin/. You will be asked to enter a username and password. You need to enter the username and password of a valid domain user (e.g. SMW\\Mgr1) to view the admin page.

Note: In Basic Authentication, the username and password are sent over the network using Base64 encoding, which can be easily reversed.

9. Delete your browsing history and close the web browser.

10. On **Server1**, in IIS Manager, for the admin folder, disable Basic Authentication and enable Windows Authentication.

11. On **Windows 10**, login as Mgr1 and browse to http://Server1/WebDoc/admin. In Windows Authentication, you do not need to enter the username or password, as your existing login credentials are used.

Windows authentication is the best authentication scheme in an intranet environment where users have Windows domain accounts, especially when using Kerberos. Windows authentication does not pass the user's password across the network.

**Lab 3-3: Customization and Deployment of Group Policy (Estimated 15 minutes)**

1. Create a new group policy object called MemberServerPolicy and linked to MemberServerOU. On the Domain Controller, in Group Policy Management, right-click on MemberServerPolicy and Edit.
2. Make the following changes to the policy:

Computer Configuration-> Policies -> Windows Settings-> Security Settings-> Local Polices-> Security Options->Interactive logon:

Logon Message Text: set to:

*“Unauthorised access is strictly prohibited and will be investigated. Your activities may be monitored and logged.”*

Logon Message Title: set to:

*“Legal Notice”*

Number of previous logons to cache: set to 5 (see following diagram)

A screenshot of a computer  AI-generated content may be incorrect.

3. Close the Group Policy Management Editor.

4. Right-click on the MemberServerOU and choose Link an Existing GPO. (See following diagram)

5. Select the MemberServerPolicy. Click OK.

All computers in the MemberServerOU will have the MemberServerPolicy applied to them.

Test your settings!

6. On **Server1**, log off. Press Control-Alt-Del. Does the logon title and message appear?

Note: If the test message does not appear, this could be because the Group Policy takes a while to take effect on domain workstations and member servers.

Question: When the Group Policy security settings are changed, how long does it take for the settings to be refreshed on a workstation or member server? You may try to search for the answer form the web.

7. If the logon message does not appear, you can either restart Server1 or run “gpupdate /force” on Server1 to get the latest Group Policies.

Test the “number of previous logons to cache” setting:

8. Login to **Server1** as Mgr1. Go to the VMware settings, select Network Adapter, uncheck "Connected". This will bring down the virtual network adapter on Server1. (See following diagram)

Now your Member Server has no network connection to Domain Controller.

9. Log off from Server1. Try to log in as Staff1 or User1. You will get an error message “no logon servers available” as there is no network connection to the Domain Controller.
10. Try to login as Mgr1. You can login because Mgr1’s previous login was cached.
11. Go to VMware settings and enable the virtual network adapter.

**Lab 3-4: Studying Group Policy Application Order (Estimated 15 minutes)**

1. In the **Domain Controller**, run Group Policy Management.
2. Under your domain, right-click on Default Domain Policy and Edit.
3. Go to Computer Configuration-> Policies -> Windows Settings-> Security Settings-> Local Polices-> Security Options.
4. Set the “Interactive Logon: Display user information when the session is locked” property to ‘do not display user information’. (See following diagram)

5. Close the Group Policy Management Editor.

The Default Domain Policy applies to all computers in the domain.

6. On Server1, login to any user account, then lock the session.
7. At the Lock screen, do you see the user information has been removed? (See following diagram)

8. You will need to run “gpupdate” before you can see the above effect. Login to Server1 as st2612 and run “gpupdate /force”.

You will now configure MemberServerOU to Block Inheritance of the Default Domain Policy so that the current user information will be displayed at the session locked screen.

9. In the **Domain Controller**, run Group Policy Management.
10. Right-click on MemberServerOU and choose Block Inheritance. (You will see the OU icon change to a new one.)

11. Login to **Server1** as **Domain Admin** and run “gpupdate /force” so that the Group Policy settings are refreshed immediately.
12. Lock the session and check out the effect.

The Domain Administrator can Enforce the Default Domain Policy so that Administrators of OUs cannot block it.

13. In the **Domain Controller**, run Group Policy Management.
14. Right-click on Default Domain Policy and choose Enforced. (See following diagram)

15. Now verify if the system works as expected.

**Lab 3-5: Hands-on with GPO Scope and WMI Filter (Estimated 15 minutes)**

By now, you should be familiar with Group Policy management and using container to GPO linkage to control the delivery of GPOs to the targeted systems. In this exercise you will learn an additional management scheme to control the application of the GPOs.

You may wonder why we have named the GPO as DisableUSBWin10GPO in the previous exercise even though it works with Windows 2019. In this exercise, we will use WMI Filter and GPO scope setting to ensure only the system running Windows 10 will apply the DisableUSBWin10GPO.

1. First, we need to define a WMI Filter that refers to Window 10 system. At the Domain Controller, run GPMC, and create a new (and it should be your first) WMI Filter, 'Win10WMI Filter' with the following specifications:

Name: **Win10WMIFilter**

Description: **Only match with Windows 10. \* systems.**

Queries:

Namespace: **rootCIMv2** (It is the default namespace)

Query: **select \* from Win32\_OperatingSystem where Version like "10. %"**

(Note: 10. % matches Windows 10, Windows 11, Windows Server 2019 and Windows Server 2022.)

2. Using the GPMC, apply the newly created WMI Filter to the DisableUSBWin10GPO (in the Scope tab):

Graphical user interface, text, application, email  Description automatically generated

3. Now, run GPUPATE on both Win2019 and Win10 VMs to get the latest GPOs. You will verify that the Win2019 can access to the USB device and the Win10 remains in the disabled state.

4. On **Win 2019**, start a command prompt or PowerShell with Administrator right, run the GPRESULT /R command to verify the current Group Policy configuration. You will see that the system has filtered out DisableUSBWin10GPO:

**Lab 3-6: Using User Configuration to apply GPO settings to specific users. (Estimated 10 minutes)**

In the previous exercises, we have been using Computer Configuration settings to disable USB access. It implies all users logging on to the system will not be able to access the USB removable storage. In this exercise, we explore an alternative way to disable access to the USB removable storage.

1. Edit the DisableUSBWin10GPO, clear the Deny read access and Deny write access settings under the Computer Configuration section:

- a. Policies->Administrative Templates->System->Removable Storage Access->Deny read access (Not Configured).
- b. Policies->Administrative Templates->System->Removable Storage Access->Deny write access (Not Configured).

2. Edit the DisableUSBWin10GPO, Apply the following 2 User Configuration settings to this newly created GPO:

   - a. Policies->Administrative Templates->System->Removable Storage Access->Deny read access (Enabled).
   - b. Policies->Administrative Templates->System->Removable Storage Access->Deny write access (Enabled).

Graphical user interface, text, application  Description automatically generated

(Note: The Removable Storage Access Settings are available in both Computer Configuration and User Configuration.)

3. At Win10 VM, run the gpudpate command to ensure it receives the updated GPOs. Try to login as Mgr1, Staff1 and User1 to access USB storage. According to the exercise setup, Staff1 and User1 will not be able to access to the USB but Mgr1 can.

Reflection Prompt: Can you explain the outcome of exercise 3-6?

**Lab 3-7: Report the Resulting Set of Policy currently deployed on Server1. (Estimated 10 minutes)**

If the Administrator is confused about the final policy settings that will be in effect, the Resultant Set of Policy feature can be used.

1. In the **Domain Controller**, run Group Policy Management.
2. Right-click on Group Policy Results and choose Group Policy Results Wizard.
3. Choose “Another Computer” and enter “Server1”.
4. When you are prompted for the specific user. Select the SMW\\Mgr1 login account.
5. The Group Policy Results for user Mgr1 on Server1 will look something like this.

6. You can also run Resultant Set of Policy on Windows 10 or the domain controller itself to check the effective policy settings.

Reference:

[https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-firewall/create-wmi-filters-for-the-gpo](https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-firewall/create-wmi-filters-for-the-gpo)

**Lab 3-8: Deploying CIS Benchmark using GPO. (Estimated 30 minutes)**

We will dive into a practical, hands-on exercise for applying CIS Benchmark password policies in a Windows environment using Active Directory Group Policy. This guide will include more detailed steps, screenshots descriptions, and additional tips for verification.

1. In the **Domain Controller**, run Group Policy Management.
2. In the Group Policy Management Console, expand your domain to see Organizational Units (OUs) or the domain root.
3. Right-click the OU where the Window 10 client is located.
4. Select “Create a GPO in this domain, and Link it here…”
5. Name the GPO something descriptive like “CIS Password Policy.”
6. Right-click the newly created GPO and select “Edit…”.
7. In the Group Policy Management Editor navigate to Password Policy

A screenshot of a computer  Description automatically generated

8. Configure each policy setting:

   - a. Maximum password age:

        - i. Double-click “Maximum password age”.
        - ii. Set the value to 60 days.
        - iii. Click “OK”.

   - b. Minimum password age:

        - i. Double-click “Minimum password age”.
        - ii. Set the value to 1 day.
        - iii. Click “OK”.

   - c. Minimum password length:

        - i. Double-click “Minimum password length”.
        - ii. Set the value to 14 characters.
        - iii. Click “OK”.

   - d. Password must meet complexity requirements:

        - i. Double-click “Password must meet complexity requirements”.
        - ii. Set to “Enabled”.
        - iii. Click “OK”.

   - e. Enforce password history:

        - i. Double-click “Enforce password history”.
        - ii. Set the value to 24 passwords remembered.
        - iii. Click “OK”.

   - f. Store passwords using reversible encryption:

        - i. Ensure “Store passwords using reversible encryption” is set to “Disabled”.
        - ii. If it is enabled, double-click to open and select “Disabled”.

9. Close the Group Policy Management Editor.

10. Login into the **Window 10** client

11. Open Command Prompt as an administrator on the computer

12. Run the command ‘gpupdate /force’ to force a policy update

13. Verify GPO Application:

    - a. Run Resultant Set of Policy (RSOP):

         - i. Open Command Prompt and type: rsop.msc
         - ii. Review the applied settings under Computer Configuration → Policies → Windows Settings → Security Settings → Account Policies → Password Policy.

    - b. Generate Group Policy Result Report:

         - i. Open Command Prompt and type: gpresult /h report.html
         - ii. Open the report.html file in a web browser to check the applied password policies.
         - iii. 

14. Verify Compliance

    - a. Check Password Policy Locally:

         - i. Open Local Security Policy on a client machine: Press Win + R, type secpol.msc, and press Enter.
         - ii. Navigate to Account Policies → Password Policy and verify the settings.

15. Test Password Changes:

    - a. Attempt to change user passwords to ensure that they meet the new policy requirements. Try using passwords that are too short, too simple, or that don’t meet complexity requirements to confirm that policy enforcement is active.

16. Review Event Logs:

    - a. Open Event Viewer (Press Win + R, type eventvwr.msc, and press Enter).
    - b. Check under Windows Logs → Security for any warnings or errors related to password policy enforcement.

Reflection Prompt: Can you use secpol.msc to harden a standalone operating system? Try it out

**Now you can try exploring and apply the Account Lockout Policy from the CIS Benchmark using the same approach as how you have done for Password Policy.**

~ End of Practical 3~
