**Secure Microsoft Windows**

**Practical 4 – Part 1**

|  |
| --- |
| ****Objectives:****<br> After completing this lab, you should be able to:<br>(i) Understand the basic installation and configuration procedures of WSUS.<br>(ii) Configure Group Policy for Windows Update<br>(iii) Approve / test specific updates for your domain workstations. <br>(iv) Use WSUS Report Wizard to check for installed Updates. |

Lab Prerequisites:

In this Lab, you will need 3 virtual machines: A Primary Domain Controller, a Windows 2019 (serve as a WSUS client) you can use **Server1**, and a Win Server 2022 member Server with the WSUS role. Your virtual machines need to have a reliable connection to the Internet to complete the lab.

As a domain administrator, instead of letting all the windows workstations / servers download the latest Windows updates/patches from the Internet, you shall install a centralized software update server such as Windows Server Update Services (WSUS) on a member server and let it take care the download of all the updates needed for your entire network. You need to configure the targeted windows machines in your domain to check for updates from the local WSUS server. In this practical exercise, we only set the windows server 2019 to use the local WSUS services.

**Lab 4-1: Joining your WSUS Server to your domain. (Estimated 10 minutes)**

1. You can use the Windows Server 2022 baseline VM image (from Lab1) and use it to configure it as your **Server2**. (Just make sure you choose the ‘I copy it’ option when you start the image)

2. Power on your Baseline VM, configure and check that you have network access to the Domain Controller and have Internet access. (i.e., Use your PDC as the preferred DNS and set the correct gateway IP.)

   - a. The default local admin user is locadmin, password is '1qwer$#@!'.

   - b. You may check for and get the latest windows updates for this VM before proceeding to the next step.

   - c. Based on your previous exercises, accomplish the following basic configurations:

   - d. The basic configuration includes: (but not limited to)

        - i. Change your virtual machine name (at VM settings) to wsus_server.
        - ii. Computer Name change to **Server2**
        - iii. Time Zone

3. 

4. Before you joined the domain successfully, you should set and confirm that network Connection Settings of the WSUS Server accordingly.

- Static IP address
- Gateway
- DNS (Ensure the DNS is referring to your domain DNS)

5. Now you can let your standalone server join your domain.

Firstly, you must assign an appropriate static IP address to your standalone WSUS server.

Secondly, you can apply the same approach you have learnt from the previous labs to letting this server join your own domain.

However, you may encounter some issues as it is not allowed to have machines with the same SID co-exist in an Active Directory.

Graphical user interface, text, application  Description automatically generated

In case you have encountered an error message like the one as shown above: You need to regenerate a unique SID for the new server image:

At the standalone VM, right click on the Window button, open a Run Box, and enter ‘sysprep’ and press enter.

The folder which contains the sysprep application will be opened.

Run the sysprep application using administrator privilege:

A screenshot of a computer  AI-generated content may be incorrect.

Choose the default option with a tick on Generalize. (See the above), press OK to proceed.

Reflection prompt: While waiting for the operation to be completed, you may go to search what this sysprep is for.

6. Once the system is rebooted and reset, you should notice that the computer name has been assigned with a random value. Set it back to ‘Server2’ (or your preferred value) before proceeding to the next step.

7. Rename the local admin user account from Administrator to locadmin.

8. Before you try to join the domain again, you should set and confirm that network Connection Settings of the WSUS Server accordingly.

- Static IP address
- Gateway
- DNS (Ensure the DNS is referring to your domain DNS)

Now, join your server to the domain.

http://www.clker.com/cliparts/o/x/W/v/t/i/internet-cloud-hi.png

http://www.clker.com/cliparts/k/7/D/E/O/B/computer-hi.pnghttp://www.clker.com/cliparts/4/a/o/k/E/Q/server-hi.pnghttp://www.clker.com/cliparts/4/a/o/k/E/Q/server-hi.png

Windows Server 2019 – gets updates from Server2.

Server2 – WSUS Server installed and getting updates from Microsoft.

Domain Controller

checks for update from the Internet.

**Lab 4-2: Setup a server with the Windows Server Updates Services (WSUS) role (Estimated 30 minutes)**

**(Warning! WSUS service requires more memory than any other servers in the SMW lab. (Try to adjust the memory allocation for all the involved VMs accordingly. Recommended minimum Memory – 3 GB.  If possible, set 4 GB memory to your WSUS server)**

You will install Windows Server Update Services (WSUS) on a new member server (Server 2022)

1. Power on the **Domain Controller**. (The domain controller is a vital part of your domain, so it should always be on!)

2. At this point, your new **Server2** VM is tapping onto the DHCP and DNS services of your domain, but it is still running as a standalone server. Other than that, it does not have other relations with your domain.

3. Before proceeding with the next step, please ensure your Server2 is compliant with the following two requirements:

   - Your Server2 has a good connection to the Internet.
   - Your Server2 should have 4 Gb of memory if possible
   - Your Server2 should have 15 to 20 Gb free disk storage at this point.

4. On **Server2**, login as Local Admin. Start Server Manager and try to add in a new Role – Windows Server Update Services

A screenshot of a computer  AI-generated content may be incorrect.

5. Take the default choices and press ‘Add Features’ at the Add Role and Features Wizard.

Take note of what are the required additional roles and features (including web server IIS) for adding in WSUS role.

A screenshot of a computer  AI-generated content may be incorrect.

6. The Wizard will refresh the Select Server Roles screen, and you may Click Next to accept and proceed to install all the required features. There will be a few more option screens, and you can accept all the default options/selection to proceed. One of them (showing below) is worth mentioning, as it gives you the choice of databases to store the windows updates content. In our case we pick the default option, as we do not have a MS SQL database in our domain. WID is a kind of standalone database that can be used to support the WSUS operations.

7. When the Content location selection appears (see following diagram). Carefully read the descriptions and the different choices. You may choose to store the updates at the local path, i.e., C:\\WSUS.  (* The wizard will create this local folder automatically).

8. The wizard will then proceed to first install the dependent roles and features followed by installing the WSUS role. Be patient as this may take a while.

A screenshot of a computer  AI-generated content may be incorrect.

9. Once the installation is completed, the system will display an 'alert' notification that post-deployment configuration is required. You may click on the ‘Launch Post-Installation tasks’ to proceed. In fact, this implies the installation process is getting into the next stage. Just wait for **a little while longer**, you will see the installation and configuration are proceeding accordingly.

10. Eventually, you will see the Windows Server Update Services appear at the left pane of the server manager.  You can select the WSUS Node and right click on the main pane and execute the Windows Server Update Services management console.

At the initial run of the WSUS management console, the Configuration Wizard will pop up automatically, it will guide you through the configuration process.

\*If you want to, you can run WSUS Server Configuration Wizard from the Options page of the WSUS 10.0 Administration console.

11. When asked to Choose Update Source (or Upstream Server), leave the default “Synchronize from Microsoft Update” checked. (See the following diagram).

12. For Proxy Server, we do not need to specify a proxy in our lab to access the Internet. Click OK to confirm your update source and proxy server settings at one go. Right after this, you will be prompted to proceed with the connection test (Initial synchronization), click ‘start connecting’ to proceed, and it will take a while until the wizard goes to the next step. \[This initial Connection Operation may take up to an hour to complete.]

13. For Language choose the 'English' only to conserve storage requirement.

14. For Products and Classifications, try to make a sensible choice to only include minimum set of updates targeted for **Windows Server 2019**, and take the default ‘classification’ choice.  *Special note! You may need to run at least once of the initial synchronization to obtain complete Products and Classifications listings. The initial synchronization only takes place when you are running the WSUS configuration wizard. To rerun Wizard, you can find the command link as the last entry in the options pane (See step 12).

15. For Synchronization Schedule, take the default setting of 'Synchronize manually', as we are not really trying to host a production WSUS which usually synchronizes with Microsoft Update on a regular basis.

16. After all the basic configuration is completed, you may trigger the first Synchronizations operation (manually), by selecting the Synchronizations option from the left-hand side menu, and click on the Synchronize Now under the Actions options at the far right:

Depending on the choices of your products and classifications settings, this synchronization operation may **take a very long while (say >= 24 hours)** to complete. Please consult your tutor before confirming your choice. In general, we recommend only including Windows Server 2019 as product and definitions files update as the classifications.

17. The middle pane of the Synchronization menu will show the task that is currently running. (See the following diagram).

18. The synchronization is downloading updates for Windows 2019. It may take about 5 to 10 minutes or longer depending on the connection speed and quality.

19. When the synchronization is complete, try to click on Synchronization Report in the right pane. You **MIGHT** get a message that you need Microsoft Report Viewer to view reports if it is not installed. If you get the message, **please do step 22 – 28** or else you are done and can process to the next lab.

20. As generally we do not use servers for browsing the Internet, we will use the Windows 10 image to search for and download Microsoft Report Viewer.

21. Using the Windows 10 image or simply your own notebook, open Internet Explorer and do a search for “Microsoft Report Viewer”

22. Download the Microsoft Report Viewer installation file.

23. Copy the downloaded msi file over to Server2 (you can use shared folders, or by other means, i.e., use drag and drop to copy the file from your notebook to the VM)

24. On Server2, close WSUS console if it is still running. Install Microsoft Report Viewer

25. While you are installing Microsoft Report Viewer, the installation wizard may prompt you to download some other Microsoft components. Just follow the instructions given and proceed.

26. In WSUS Console, click on Synchronizations in the left pane. In the middle pane, select the latest Synchronization that you just did. In the right pane, click on Synchronization Report to see the updates for Windows Server 2019 that were downloaded. Below is a screenshot of a sample report:

A screenshot of a computer  AI-generated content may be incorrect.

Congratulations to you, up to this point, you have successfully installed the WSUS service.

**Lab 4-3: Setting up WSUS Client Configuration (Estimated 15 minutes)**

You will now create a Group Policy for your Windows Server 2019 (Server 1 you have set up since lab 3). The Group Policy will make your Windows Server 2019 VM check for updates from the WSUS service running on Server2 instead of from the default Windows Update services at the Cloud.

http://www.clker.com/cliparts/4/a/o/k/E/Q/server-hi.pnghttp://www.clker.com/cliparts/k/7/D/E/O/B/computer-hi.pnghttp://www.clker.com/cliparts/4/a/o/k/E/Q/server-hi.png

Check for updates.

Domain Controller: Let Windows Server 2019 receive a group policy object which configures the client to check for updates from the WSUS Server

WSUS Server (Server2)

Windows 2019

(Server1)

1. On the **Domain Controller**, use Active Directory Users and Computers, verify if there is an OU called "MemberServerOU". Create one if it is needed. Move your Windows Server 2019 (Server1 or another name you have assigned it to be.) to this "MemberServerOU".

2. Run Group Policy Management Console (GPMC). Right-click on the MemberServerOU in the left-hand pane and choose Create a GPO...

3. For Name, enter “WSUS_Policy”. Click OK.

4. Expand the MemberServerOU. Right-click on WSUS_Policy and choose Edit.

5. 

6. Make the following changes to the policy:

Computer Configuration-> Policies -> Administrative Templates -> Windows Components -> Windows Update

7. Double-click “Configure Automatic Updates”.

Click Enabled. Select “4 - Auto download and schedule the install”. (See the following diagram)

Or you can select option 3 (The default).

Tick the Check box of Install during automatic maintenance.

8. Click OK.

Reflection Prompt: Study the difference between Option 2 to Option 5 settings.

9. Double-click “Specify Intranet Microsoft Update Service Location”.

10. Click Enabled. Type “http://Server2:8530” for both the “intranet update service for detecting updates” and the “intranet statistics server” (you can also replace “Server2” with the IP address of Server2) (see following diagram).

11. Click OK.
12. You may also check out the “Automatic Updates detection frequency” setting.  (Optional)
13. Close the Group Policy Management Editor.

To get the Group Policy applied to your Windows 2019 immediately:

14. Power on your Windows Server 2019 (if needed).
15. On the Windows Server 2019 as domain administrator, at command prompt or power shell (run as Administrator), run “gpupdate /force”.
16. To verify if the update WSUS settings have been applied to your Win server 2019 via the GPO, you can use the gpresult command:

GPRESULT /R /V

- To display the actual GPO setting, you need to run the command with Administrative Privilege.

- You may also need to ‘pipe’ the out to a text file, as the output is long.

  - cd c:/Users/Public
  - gpupdate /force
  - gpresult /R /V > gpresult.txt

Here is sample session of the getting a gpresult.txt by gpresult /r /v command:

After the Group Policy has been applied to the Windows Server 2019, it may take about 20 min for the Windows Server 2019 to contact the WSUS Server.

17. Check for updates interactively/explicitly.

- i. At the local server dashboard of the server manager
- ii. Click on the link next to the Windows Update
- iii. Click on the Check for updates button at the popup.

A screenshot of a computer  AI-generated content may be incorrect.

Note: For newer windows: e.g., To force a Windows 10 Client to contact the WSUS Server immediately, run the Windows Update Auto-Update Client by typing "usoclient.exe startscan".

18. What you should be getting will be like the following:

A screenshot of a computer  AI-generated content may be incorrect.

As the server1's update process is controlled by the WSUS server now. There is no approved update for server1 to use at this initial moment.

19. Trouble shooting common Windows Updates issue on windows 10 or later: **(Optional)**

In case you have encountered Error message after applying the “Check for Updates”:

You may try to resolve the problem by using the updates troubleshooter.

At the Setting >> Update and Security, click on the troubleshoot at the left pane. At the main pane, click on Windows Update and press the “Run the troubleshooter” button.

20. Check that the Windows Server 2019 appears in WSUS on Server2:

    - i. On Server2, login as Domain admin, run WSUS management console.
    - ii. In the left-hand pane, expand Computers, All Computers, Unassigned Computers.
    - iii. Change the Status to “Any” and click Refresh. (See following diagram) *

Your server 2019 computer name should appear

A screenshot of a computer  AI-generated content may be incorrect.

As you can see from the above, the client machine has not reported its status to the WSUS server yet.

\*In case you cannot see any computer listed, you may right click on the "Computer" label and choose 'search' option to force the Active Directory Search to load in the computer information.

**Lab 4-4: Set up a Test Computer Group to test updates from WSUS. (Estimated 10 minutes)**

We can create a computer group with test computers that receive the updates first. If the updates do not cause any problems on the test computers, then the updates can be approved for the rest of the computers.

http://www.clker.com/cliparts/4/a/o/k/E/Q/server-hi.png

Updates approved for the Test Group first for testing.

http://www.clker.com/cliparts/k/7/D/E/O/B/computer-hi.pnghttp://www.clker.com/cliparts/k/7/D/E/O/B/computer-hi.png

WSUS Server (Server2)

Test Group

1. On **Server2**, at WSUS management console, in the left-hand pane, expand Computers, Right-click on All Computers and choose Add Computer Group.
2. For Name, type “WinTestGroup”. Click Add.
3. In WSUS, on the left-hand pane, click on Unassigned Computers. Right-click on your Windows 2019 client and choose Change Membership.
4. Select WinTestGroup and click OK.

**Lab 4-5: Approve and Deploy updates to the Test Group (Estimated 15 minutes)**

Approved update downloaded to Test Group

http://www.clker.com/cliparts/4/a/o/k/E/Q/server-hi.png

Test Group

http://www.clker.com/cliparts/k/7/D/E/O/B/computer-hi.pnghttp://www.clker.com/cliparts/k/7/D/E/O/B/computer-hi.png

WSUS Server (Server2)

1. On **Server2**, in WSUS management console, in the left-hand pane, expand Updates. Click on Security Updates.
2. For Approval, choose Unapproved. For Status, choose Needed. Click Refresh. (See following diagram)
3. Select any one or two of the updates and Right-click on it/them and choose Approve. (See following diagram)

A screenshot of a computer  AI-generated content may be incorrect.

4. Click OK.

5. Change the Approval dropdown to Approved. Click Refresh. You will see the approved update listed. (See following diagram)

A screenshot of a computer  AI-generated content may be incorrect.

6. The update you have approved will be downloaded from Microsoft at the next synchronization process.

7. Do a manual synchronization Now (See the following diagram):

8. By default, clients will check the WSUS for updates every 22 hours. To make the Windows Client check for updates immediately, you need to do it manually.

9. **Eventually**, at your Windows 2019, you will see the update will be automatically downloaded and pending for installation without the user intervention.

10. Install the update. Restart the Windows Server 2019 if necessary.

You will now look at the Windows Update Status Report for your Windows Server 2019:

11. On Server2, in WSUS console, expand Computers, All Computers. Click on WinTestGroup. Right-click on your Windows server and choose Status Report. You should be able to observe that the report is now different from the one you have seen at Exercise 5-4 step 4. You can browse through the pages of the report to see the updates that have been installed for the Windows Server 2019.

A screenshot of a computer  AI-generated content may be incorrect.

~ End of Practical 4 – Part 1 ~
