<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in Azure</h1>
This tutorial/lab outlines the implementation of on-premises Active Directory within Azure Virtual Machines. If you havent seen my prerequisite tutorials first, go ahead and check them first and come back here afterwards, as this lab builds on those ones.<br />

<h2>Prerequisites</h2>

- [Setup a Subscription and a Resource in Azure for Beginner Labs](https://github.com/jacksonmalms/setup-azure-sub-and-resource)
- [Create Virtual Machines Within Azure and Observe The Network Topology](https://github.com/jacksonmalms/create-azure-vm)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Azure
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain
- Setup Remote Desktop for non-administrative users on Client-1
- Create a bunch of additional users and attempt to log into client-1 with one of the users
- Clean-up/delete resources or stop both VMs

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/mBlYU0j.png"/>
</p>
<p>
First, create the resources, I just named the resource group AD-LAB, the virtual machine DC-1. Select Windows Server 2022 and set the size to be 2 CPUs, then create the VM.
</p>
<br />

<p>
<img src="https://i.imgur.com/iANg4bk.png"/>
</p>
<p>
Then create the Windows 10 VM (Client-1), make sure it's in the same resource group and region, and then select the size to use 2 CPUs as well. And finally make sure it is in the same vnet and subnet as DC-1. 
</p>
<br />

<p>
<img src="https://i.imgur.com/2On7GkF.png"/>
</p>
<p>
Now go into DC-1 and click into the NIC (network interface card) as shown above.
</p>
<br />

<p>
<img src="https://i.imgur.com/iBiPkNw.png"/>
</p>
<p>
Then go into IP configurations and click on DC-1's configuration.
</p>
<br />

<p>
<img src="https://i.imgur.com/5PGTLsH.png"/>
</p>
<p>
Now set the IP to static and click save in the upper left.
</p>
<br />

<p>
<img src="https://i.imgur.com/sCtm2u6.jpg"/>
</p>
<p>
Now login to Client-1 and ping DC-1's private IP with perpetual ping with ping -t, notice the request is being timed out, keep this open for now.
</p>
<br />

<p>
<img src="https://i.imgur.com/d0h4V0Q.jpg"/>
</p>
<p>
Now log into DC-1 and open Windows Defender Firewall with Advanced Security.
</p>
<br />

<p>
<img src="https://i.imgur.com/wJhlDGy.png"/>
</p>
<p>
Now go into inbound rules and enable all ICMPv4 traffic, this is thr protocol that the ping command uses.
</p>
<br />

<p>
<img src="https://i.imgur.com/Ig6FsqZ.jpg"/>
</p>
<p>
Go back into Client-1 and notice that we are now getting replies back from DC-1 with the ping -t command. (FYI if you hit Ctrl + C in command prompt it will stop whatever command that is currently being exceuted)
</p>
<br />

<p>
<img src="https://i.imgur.com/Mb91ghL.png"/>
</p>
<p>
Now we will install AD inside of DC-1, so go back into DC-1 and go into server manager -> Add roles and features -> Server Roles -> select Active Directory Domain Services -> Add Features -> and then click next until you can hit install.
</p>
<br />

<p>
<img src="https://i.imgur.com/wVqaKbn.png"/>
</p>
<p>
Now we need to promote DC-1 as a domain controller, so click on the flag in the top right and click "Promote this server to a domain controller".
</p>
<br />

<p>
<img src="https://i.imgur.com/XFj5Yrf.png"/>
</p>
<p>
Now add a new forest and name it (I just named this one "mydomain.com"), hit next.
</p>
<br />

<p>
<img src="https://i.imgur.com/We2cZjC.png"/>
</p>
<p>
Then set the DSRM password, and then just hit nest until you can hit install. It will restart the VM after installing.
</p>
<br />

<p>
<img src="https://i.imgur.com/5nfxWVn.png"/>
</p>
<p>
When you connect back into DC-1, you will need to use a different account and use mydomain.com\user (make sure it has a backslash and not a forward slash), use what ever username that you set when you created DC-1.
</p>
<br />

<p>
<img src="https://i.imgur.com/RfGRNWT.png"/>
</p>
<p>
Now go to Active Directory Users and Computers (ADUC).
</p>
<br />

<p>
<img src="https://i.imgur.com/Gl98T2W.png"/>
</p>
<p>
Now create two new Organizational Units (OU) one called “_EMPLOYEES” and the other “_ADMINS”. Do that by going to mydomain and right clicking in the empty space.
</p>
<br />

<p>
<img src="https://i.imgur.com/HDPxqTB.png"/>
</p>
<p>
Now create a new admin called "jane_admin" and uncheck "User must change password at next logon".
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
