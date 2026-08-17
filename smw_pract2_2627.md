**Secure Microsoft Windows**

**Practical 2**

|  |
| --- |
| ****Objectives:****<br> After completing this lab, you should be able to:<br>(i) Verifying and completing the setup of the Domain DNS Server with root hints update and Reverse Lookup support <br>(ii) Joining a Windows 2019 server to a Domain<br>(iii) Configuring Folder Permissions<br>(iv) Configuring Shared Folder Permissions<br>(v) Understand the operations of security groups.<br>(vi) Setting up Encrypting Files and the basic usages of the Cipher command<br>(vii) Try simple PowerShell commandlets |

Lab Prerequisites:

Completion of the Lab exercises on Practical 1.

Domain User account: Mgr1 has been created successfully.

The Base image of Windows 2019 (w2019ms.zip)

**Lab 2-1: DNS Server**

**2-1a Update the Root Hints List (Estimated 10 minutes)**

The 13 root name servers are operated by at least 12 independent organizations. Sometimes the IPv4 and/or IPv6 addresses are changed or are added to the list. Thus, good practice is to update your DNS Server Root Hints occasionally.

To update the list, you need to find out the IP address of an active root hints server.

You can find the information at [https://root-servers.org/](https://root-servers.org/)

A screenshot of a computer  Description automatically generated

For instance, we find out 198.41.0.4 is the ip address of Server A.

1. On the **Domain Controller**, start the DNS Manager and access the properties menu.
2. Select the Root Hints Tab.
3. Click on the Copy from Sever button.

A screenshot of a computer  Description automatically generated

A screenshot of a computer screen  Description automatically generated

4. Enter the IP address of the root hints server (e.g., 198.41.0.4) to the popup.
5. Press OK.
6. And close the properties menu by pressing the OK button. That's all.

**Caveat: Depending on your ISP policy, you may not be supposed to query the root hints directly. You are only allowed to use the forwarder scheme to access DNS services.**

**2-1b Reverse Lookup Records (Estimated 15 minutes)**

With DNS, fully qualified domain names can be resolved to their IP addresses (forward lookup) and IP addresses can be resolved to their fully qualified domain names (reverse lookup)

1. On the **Domain Controller**, run Administrative Tools, DNS Manager.
2. Click on your Domain Controller Computer Name.
3. In the right-hand pane, double-click on Root Hints. A list of the root nameservers used in the DNS is displayed.

A screenshot of a computer  Description automatically generated

4. Right click on the Forwarders to verify your forwarder settings.
5. Under your Domain Controller Computer Name, expand Forward Lookup Zones. Click on your domain name.
6. In the right-hand pane, you should see the computer names and IP addresses of your three images.
7. In the left-hand pane, click on Reverse Lookup Zone. No reverse lookup zones have been created yet.
8. On your **Win 10**, start a Command Prompt.
9. Run the DNS Client program: “nslookup”.
10. Type “client” and press Enter. You should see the IP address of client displayed.
11. Type the computer names of your Domain Controller. The IP address should be returned too.
12. Type the IP address of client and press enter. There will be an error message as no reverse lookup zones have been created.

A screenshot of a computer  Description automatically generated

13. Using the DNS manager On the Domain Controller, create a new Reverse Lookup Zone. Right click on the Reverse Lookup Zone and select New Zone. Take all the default value and choose the create a IPv4 Reverse Lookup Zone.

A screenshot of a computer  Description automatically generated

14. You need to key in the correct Network ID (i.e., The First 3 bytes of your network segment). The wizard will use these values to define the proper reverse lookup zone name.

A screenshot of a computer  Description automatically generated

15. Once the Reverse Lookup zone is created, you can add PTR records of your domain controller and client into the zone as described in the following step.
16. On the **Domain Controller**, in DNS, under Forward Lookup Zone, click on your domain name. In the right-hand pane, double-click your Domain Controller computer name. (See following diagram)

dns.gif

Double-click on your Domain Controller computer name.

17. Check the box “Update associated pointer (PTR) record” and click Apply. Click OK.
18. Repeat this for your Windows 10.
19. Redo the step 8-11/8-12, you should see some difference. Now your DNS server can perform reverse lookup for the Domain Controller and client.

A computer screen shot of a black screen  Description automatically generated

**Lab 2-2: Adding a new member server (Windows 2019) to your domain. (Estimated 10 minutes)**

1. Start your **Domain Controller**

   - You need to start your Domain Controller as it provides the essential DNS and DHCP services to the VM within your NAT segment.
   - You do not need to start your Windows 10 client. (To reduce the resources)
   - **Caveat: If you have forgotten to disable the vmware local dhcp services, you may encounter many mysterious issues due to the inconsistency of DNS and DHCP services.**

2. After the Domain Controller has started (with the Server Manager loaded). You may start your Windows 2019 VM.

   - The default login / password of Windows 2019

     - locadmin / 1qwer$#@!

At the first run of your new VM, you may see the Network Connection and the vmware tools popup. It is normal. After clearing the vmware tools popup, your VM may be forced to sign out to let the new display resolution settings kick in. Login to locadmin again.

Proceed to carry out the necessary steps to configure an additional Windows 2019 server to join your domain.

In initially, your server 2019 is based on Dynamic IP. Decide a set of static IP values for this server.

- Ensure you are assigning a **reasonable static IP address** to this server. You can name it **Server1** for ease of naming.

  - List down the following of your new server:

    1. Computer Name: Server1
    2. Static IP address: 192.168.30.56
    3. Network Mask: 255.255.255.0
    4. Gateway: 192.168.30.2
    5. Preferred DNS: 127.0.0.1

- Verify your new server can,

  - access to the Internet
  - ping your FQDN (e.g., smw.soc.org)

before joining the domain.

- Proceed to change your server’s name and join the domain. When being prompt for the Authentication before joining the domain, try to use the Mgr1 credential. (By default, any authenticated domain account can bring in new domain member machine!)

- Using the BPA to check this new member server. (optional / you can defer it)
- Verify that your new server can access the Internet after joining the domain.

3. Verify your setup by logging onto the new member server with the following Domain user accounts (you have created in the previous practical exercises):

   - Mgr1, Staff1, and User1

Click on the Other user link, then you can type in the user id for your login.

- In fact, you should avoid using locadmin account to login to this server. As locadmin remains as the local administrator of the server. It is not sufficient if you want to carry out domain level administrative tasks. (locadmin becomes a backup / fallback account)

Reflection prompt: Based on your observations, comment on the difference between the two BPA scanning results of the Domain Controller and this new member server.

**Lab 2-3**

Login as different Domain user accounts at **Server1** to carry out the following 2-3 (a-d) exercises.

**Lab 2-3a: Encrypting Files (Estimated 10 minutes)**

**2-3a.1**

Login in as User1 to create and encrypt files in a folder.

At step 1: Navigate to C:\\Users\\User1\\Documents to start this exercise.

1. Right-click an open area, click New, click Folder, and enter a folder name that is a combination of your first initial and last name, such as BTan, and press Enter. Find a file to copy into the folder, such as a text-based document or another file already in the root of drive C. Right-click it and click Copy. Open the new folder you created, right-click in an open area, and click Paste to copy the file into the new folder.
2. Right-click your new folder, such as BTan, and click Properties. Make sure that the General tab is displayed, and if it is not, then click it.
3. Click the Advanced button.
4. Check ‘Encrypt contents to secure data’, and then click OK.
5. Click Apply.
6. Be certain that Apply changes to this folder, subfolders and files are selected and click OK.
7. Click OK again to exit the Properties dialog box. Move the pointer to a blank area and click so that your folder is no longer highlighted. Now notice that the folder had a ‘lock’ on it and if you see the copied file inside, it too has a ‘lock’ appears when it is encrypted.
8. Decrypt the folder so that you can use it for another activity.
9. You can try to ‘Compress contents to save disk space’ and see what appears on the folder and file. Remember to uncompress it for another activity
10. Close the folder’s Properties dialog box and leave File Explorer open for the next activity.

**2-3a.2 Using command, try to use cipher command to:**

**Display EFS status at folder and file level**

**Encrypt files**

**Decrypt files**

Reflection Prompt: Can you verify how to apply -W option with the Cipher command, and explain what is the use of it?

**Lab 2-3b: Configuring Folder Permissions (Estimated 10 minutes)**

Login in as User1 to create a folder and grant modify permission of this folder to other user via security group.

1. Navigate to C:\\Users\\Public to start this exercise. Create a folder called Test plus your initials, such as TestXX. Inside the folder you just created, create a subfolder called Utilities plus your initials, such as UtilitiesXX.

2. Right-click the new Utilities folder, click Properties, and then click the Security tab.
3. Click the Edit button. Click each group and user again and notice that some boxes are checked and deactivated because they represent inherited permissions.
4. Click the Add button.
5. Click the Advanced button in the Select Users, Computers, Service Accounts, or Groups dialog box. Click Find Now. Go to find the ‘DomainMgrs’ group. Click OK.
6. In the Permissions dialog box, ensure that ‘DomainMgrs’ is selected.

What permission do the ‘DomainMgrs’ have by default?

7. Click the Allow box for Modify.
8. Click OK in the folder’s Permissions dialog box and click OK in the Properties dialog box.
9. Leave open File Explorer (the window with the folder containing the Utilities folder you have been working on) for the next activity unless you need to stop working now.

**Lab 2-3c: Inherited Permissions (Estimated 10 minutes)**

Before proceeding to work on this exercise, try to login to Server1 with Mgr1 and Staff1 accounts. Find out if these two accounts can both navigate to the C:\\Users\\Public\\TestXX\\UtilititiesXX folder. Record down your findings and logoff.

1. Login as User1.
2. Use File Explorer to display the Utilities folder that you created in the last activity. Right-click the folder and click Properties.
3. Click the Security tab.
4. Click the Advanced button.
5. Review the groups that have permissions for the folder. Click the Disable inheritance button
6. Notice that you can select to convert inherited permissions to specific permissions for the folder or you can select to remove all inherited permissions. Click Remove all inherited permissions from this object.
7. In the Advanced Security Settings window, you’ll now see that all other groups and user accounts with permissions have been removed, except for the ‘DomainMgrs’ group that you configured manually and that does not use inherited permissions.
8. In the folder Properties dialog box, notice that because you confirm the operation in Step 6, the default inherited groups are not listed.
9. Click OK in the folder’s Properties dialog box.
10. Leave File Explorer open for the next activity.

Now login as Mgr1 and Staff1 again to try to navigate to the C:\\Users\\Public\\TestXX\\UtilititiesXX folder. Compare your results with the previous experiment. Do you see any difference? Can you explain?

**Lab 2-3d: Configuring Advanced Permissions (Estimated 10 minutes)**

1. Login as User1
2. Create a new folder called Documentation plus your initials, such as DocumentationXX.
3. Right-click the new folder and click Properties.
4. Click the Security tab.
5. Click Edit.
6. Click Add.
7. Click Advanced in the Select Users, Computers, Service Accounts, or Groups dialog box.
8. Click Find Now
9. Double-click the Domain Users group under Search results.
10. Click OK in the Select Users, Computers, Service Accounts, or Groups dialog box.
11. Click OK in the Permissions dialog box.
12. Ensure that Domain Users are selected under Group or usernames in the Properties dialog box.
13. Click the Advanced button.
14. Select Domain Users under Permission entries in the Advanced Security Settings window and click the Edit button.
15. Click Show advanced permissions in the Permission Entry window.
16. Click the Allow box for each of the following advanced permissions entries: Create files/write data, Create folders/append data, Delete subfolders and files, and Delete.
17. Click the box for Only apply these permissions to objects and/or containers within this container.

**What does “Apply these permissions to objects and/or containers within this container only” mean?**

18. Click OK in the Permission Entry window.
19. Click OK in the Advanced Security Settings window.
20. Click OK in the Properties dialog box.
21. Leave File Explorer open for the next activity (unless you need to stop working).

**Lab 2-3e: Verifying the effect of Deny Permissions. (Estimated 10 minutes) start here (left off may 10)**

1. Login in as a **Domain Admin** to your **Domain Controller**.
2. Ensure you have the following 2 global groups in your domain: GlobalMgrs, GlobalStaff

(Create them accordingly).

3. Create a new Domain user account: newManager.
4. Add newManager to both GlobalMgrs and GlobalStaff global groups.
5. Ensure you have the following 2 domain local groups in your domain: DLMgrs, DLStaff.

(Create them accordingly)

6. Add GlobalMgrs group to DLMgrs.
7. Add GlobalStaff group to DLStaff.
8. Switch to the member server, Login as Domain Admin.
9. Create a file, testDeny.txt, at the C:\\Users\\Public\\TestXX\\ folder.
10. Assign the full control permission of the testDeny.txt to DLMgrs.
11. Assign the read control permission of the testDeny.txt to DLStaff.
12. Switch the login to newManager at the member server.
13. Test if newManager can update the testDeny.txt file.
14. As newManger, try to add Deny Read permission to DLStaff group for the file, testDeny.txt

(If you fail to do so, you may need to switch back to Domain Admin to add the permission)

15. Test if newManger can update the testDeny.txt file after step 14.

Note: At times, it is always easier to view the Account - Groups - Permission relationship with a diagram.

For example, we can summarize the 2-3e group assignment configuration in the following diagram.

Reflection prompt: Do you understand the rules for effective permissions resolution when an account is receiving different set of permissions from more than one group against the same resource.

**Lab 2-4: Configuring a Shared Folder (Estimated 30 minutes)**

**PROBLEM BASE (SELF EXPLORE):** Shared Folder features require you to turn on the File and Printer Sharing and Network Discovery features at the server, you need to do that by getting into the Network and Sharing Center. One way is to try to do it manually (update the firewall rules).

1. Login as the User1 to the **Server1**.
2. Navigate to This PC -> Documents and create a new folder with the name DocumentXX (XX is your initial).

3. Right-click the folder, point to Share with, and click Specific people to see the File Sharing window.
4. Click the down arrow next to the Add button and click Find people.
5. Click the Advanced button in the Select Users or Groups dialog box.
6. Click Find Now.
7. Double-click the DomainMgrs group. Click OK.
8. Click the down arrow for the Permission Level for the DomainMgrs group and click Read/Write.
9. Click the Share button at the bottom of the File Sharing window.
10. The File Sharing window indicates the folder is shared and enables you to email the link for the shared folder or to copy the link into a program. Click Done.

(Repeat Step 4 – 11 and assign **Authenticated Users** with Read Permission)

11. In File Explorer, right-click the folder you just shared and click Properties.
12. Click the Sharing tab.
13. Click the Share button.

(You will be prompted by the Administrator credential to complete this sharing operation. Only administrators can create shares in a domain server.)

14. Click Cancel.
15. Click the Advanced Sharing button in the Properties dialog box.
16. In the Advanced Sharing dialog box, click the box for Share this folder.
17. Click the Permissions button in the Advanced Sharing dialog box. Notice that the permissions are now displayed as Full control, Change (same as Contribute), and Read. Also, you can select the Allow or Deny boxes for any of the permissions. Further, you can add or remove users and groups. Click Cancel.
18. In the Advanced Sharing dialog box, click Caching.
19. To configure full offline use, click All files and programs that users open from the shared folder are automatically available offline. Notice that optimized performance is enabled by default. Click OK.
20. Click OK in the Advanced Sharing dialog box.
21. Click Close in the Properties for the folder dialog box. Close File Explorer.

After you have shared out the folder, test your settings.

1. On **Win 10**, login as Mgr1.
2. Access the Shared folder. \\\\*YourServer1\\*Users\\User1\\Documents\\DocumentXX test that Mgr1 can view, create, and modify files in the shared folder (Mgr1 have Co-owner Share permission)

3. On **Win 10**, login as Staff1.
4. Access the Shared folder.
5. Test that Staff1 can view, but cannot make changes in the shared folder (Authenticated User has Reader Share permission only)

You can also view shared folders through Network Node.

6. Go to Network (make sure Network Discovery is enabled on your Windows 10) and double-click on your **Server 2019** computer name.

**Lab 2-5: Publishing a Shared Folder (Estimated 15 minutes)**

The goal is to publish the shared folder(s) you have created in the previous exercises via the Active Directory.

1. On the **Domain Controller**, login as **Domain Administrator**.
2. Open Server Manager, if it is not open.
3. Click Tools and click Active Directory Users and Computers.
4. If necessary, click the right arrow in front of the domain name in the left pane to see the items under the domain. Right-click the Users folder in the tree (or you could right-click an OU at this point to control administration of the published folder from an OU by delegating authority over the OU). Point to New.
5. Click Shared Folder.
6. Enter the name for the published shared folder, such as DocumentationXX (the folder you created in Lab 2-4). Enter the network path to the share, such as \\\\servername\\Users\\Administrator\\Documents\\ DocumentationXX. Click OK. Notice that the shared folder is now one of the objects listed in the right pane within the Users (or OU) folder.
7. Close the Active Directory Users and Computers window.
8. In the Win 10, under the Network Node of the File Explorer, try to locate the shared folder via the ‘Search Active Directory’

Reflection Prompt: Can you identify the Pros and Cons of the Publishing Shared Folder feature of the Active Directory?

**Lab 2-6: PowerShell for log tracing (Estimated 15 minutes)**

(This exercise should be done in your **Domain Controller**)

1. Complete the Activity below before proceeding to Step 2:

   - Click Start and click the Windows PowerShell tile; or click Start, click the Windows PowerShell folder to open it and click Windows PowerShell.
   - To view the files in the current folder, such as \\Users\\Administrator, one page at a time type the traditional command, dir | more and press Enter. Press the Spacebar, if necessary, to advance to additional screens.
   - To change to the \\Users directory, enter cd \\users and press Enter.
   - View a list of cmdlets. Type Get-Command | more and press Enter. You see the commands one screen at a time. Press the Spacebar to advance to the next screen and repeat this step until you’ve viewed all the screens. (Note that you can also press q to exit back to the command line, if you decide not to view all the screens of commands.)
   - Press the up arrow and notice that the last command you entered is placed on the command line so that you can repeat the command. Press the up arrow again and you’ll see the second to-last command you entered. Press Enter to run that command.
   - Type Get-Process and press Enter to view the processes running on the server.
   - Next, type Get-Process | Where { $_.WS -gt 100MB } and press Enter. (If no process is displayed change the command to use 20MB.)
   - To stop a process, you would type Stop-Process -Name “processname” (where processname is the name of a process you found in the previous step) and press Enter.
   - Type Get-Service and press Enter to view services that are running or are stopped. (Or you can enter Get-Service | more to display the services one screen at a time.)
   - You can view more about the main or core Windows PowerShell cmdlets. Type Get-Help About_Core_Command and press Enter. (If you are asked if you want to run Update-Help, type Y for yes and press Enter)
   - Also, you can view the online help about a specific cmdlet by typing Get-Help plus the cmdlet. For example, type Get-Help Join-Path and press Enter.

2. In PowerShell, type "get-help get-eventlog" to see a brief description for the get-eventlog cmdlet.
3. Type "get-help get-eventlog -full" to see a more detailed help screen.
4. Type "get-eventlog -list" to view the available event logs.
5. Type the following to list the events in the Application log which come from the source “VMTools”.

get-eventlog –logname application | where-object {$_.source –eq "vmtools" }

6. Figure out how to use PowerShell to list the oldest 5 events in the Application log which come from the source of “VMTools”. (hints: you may use the '|' pipe ,sort-object and select-object, to achieve that)

Reflection prompt: You may record down the commands that you have tried (and working).

**Lab 2-7: PowerShell for Domain Enumeration (Estimated 10 minutes)**

(This exercise is preferred to be done in your **Server1 or Windows 10**, recommend using PowerShell ISH).

We will explore how to collect Domain information using a domain user account. (No special administrative privilege) via two approaches.

- Load in and Use .NET module at the PowerShell.
- Use cmdlets.

1. In **Windows 10**, login with as Mgr1
2. Search and open a PowerShell ISH session.
3. Create a Domain object with the following command at the ISH:

$myObj = \[System.DirectoryServices.ActiveDirectory.Domain]

4. Execute the GetCurrentDomain() method to retrieve the domain information:

$myObj::GetCurrentDomain()

5. Execute Get-ADUser -Filter * to retrieve a list of all the domain user accounts.

6. Execute Get-ADComputer   -Filter * to retrieve a list of all the domain computer objects.

**Lab 2-8: Create a new OU and move Windows Server 2019 to OU. (Estimated 10 minutes)**

1. On your **Domain Controller or Windows 10**, run Active Directory Users and Computers. Create a new OU called MemberServerOU. Move Server1 from the 'Computers' folder to this MemberServerOU (see following diagram).

Server1 is in the MemberServerOU

active.gif

Reflection prompt: Can you tell the difference between 'Folder' and 'Organizational Unit Container' that shown at the left-hand pane of the Active Directory Users and Computers management console. Other than their appearance, what is their main functional difference?

~ End of Practical 2 ~
