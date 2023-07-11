# Objectives

Plan deployment of service packs and hotfixes, highlight importance of software updates for a secure computing environment  

Intro to common Windows Update practices and tools  

Intro to Window Server Update Services (WSUS). Architecture, services and operations  

Basic Stages for Update/Patch management. Understand deployment considerations for various machines. Evaluate the applicability of service packs and hotfixes. Plan a rollback strategy  

Post deployment review, common fixes for Windows update problems  

<br>

# Navigation
* [Planning Deployment of Service Packs and Hotfixes](#planning-deployment-of-service-packs-and-hotfixes)
* [Windows Server Update Services (WSUS)](#windows-server-update-services-wsus)

<br>

## Planning Deployment of Service Packs and Hotfixes

Need to keep technology environment secure and reliable  

Requires identifying security vulnerabilities and responding quickly  

<b>Patch management</b>, method for keeping PCs up to date with new software releases  

<b>Security patch management</b>, patch management with a concentration on reducing security vulnerabilities; essential for secure IT management and operations  

<br>

### How to manage updates

Microsoft Update and Automatic Updates  

Windows Server Update Services (WSUS)  

<br>

Microsoft Endpoint Manager  
* Comprises of many configuration tools  
* Microsoft System Centre Configuration Manager (SCCM)  
* Including software deployment management

<br>

3 general approaches for managing Windows updates
* 1st is purely relying on Windows updates which automates the download and installation of the latest updates from Microsoft Windows Updates Services  
* 2nd is by setting up a centralized and local Update Services at the Enterprise level  
* 3rd is going beyond software update or patch management, to include software marketplace solution with a centralized services at the Enterprise level.

<br>

### Microsoft Update and Automatic Updates

For consumers and small businesses (fewer than 50 PCs)  

Updates can be installed with minimal or no user interaction  

Uses Internet connection to search for downloads from Microsoft Update website  

No need to understand the technical details of the security update  

Simplest approach that is applicable to client systems that do not run mission critical services  

For servers, automatic Microsoft Updates applied could disrupt many services during the update. Thus, enterprises usually do not adopt this approach for their servers  

Microsoft cautions that users should ensure they do not have apps that could be affected by the updates  

Price: Free for licensed users  

<br>

## Windows Server Update Services (WSUS)

For medium or large businesses  

Admins can manage update settings and control the distribution of updates  

Admins can test updates on selected PCs before deploying to the rest of the network  

WSUS approach is to install a local Windows update server at the Enterprise network  

The source of the patches is from the standard Windows update services. Unlike the first approach, the WSUS admins have the option to hold back the deployment of the available patches, and defer the actual updates to the clients based on a planned schedule. This approach is essential to servers that run mission critical services. 1 limitation of WSUS is it only supports Updates from Microsoft.  

Updates can be downloaded once from Microsoft Update Website and stored on a local server (free up internet bandwidth)  

Does not support deployment of non-Microsoft updates  

Price: Free for licensed users  

<br>

### Microsoft EndPoint Manager

Supports management and distribution of Microsoft and non-Microsoft software updates and apps  

Supports various types of endpoints: Windows/ non-Windows platforms  

WSUS is part of it  

Advanced admin control features  

Price: Charges apply  

Alternatives:  ManageEngine, Freshservice, Malwarebytes, ....etc  

<br>

### Get updates from Microsoft website only

Microsoft may send email notifications about security updates  

Users should download updates from Microsoft Update website  

Microsoft does not distribute security updates by using email attachments  

Such emails containing attached "patch installers" are fake  

<br>

### Advantages of WSUS

System admin can control the updates to be applied  

Clients can be configured to get updates from a local WSUS server instead of downloading them from Microsoft's site, reducing network traffic  

WSUS is a means to provide updates to PCs that don't have Internet access  

<br>

### How a local WSUS server obtains the Windows Update files from a common Microsoft Update Services server from the Internet. The local WSUS server acts as an agent to maintain a curated update repository to provide an Enterprise level control to distribute these updates accordingly  

Diagram  
![image](../images/Pasted%20image%2020230711191957.png)

<br>

### WSUS requirements

Assume that WSUS clients are synchronizing with the server every 8 hours for a rollup of 30,000 clients  

Processor: 1.4 GHz x64 processor (2GHz or faster is recommended)  

Memory: additional 2 GB of RAM  

Available disk space: 40 GB  

Applies to: Win Server 2019, Win Server (Semi-Annual Channel), Win Server 2016, Win Server 2012 R2, Win Server 2012  

On top of hardware requirements, the WSUS role requires to run on IIS and with a few other Windows components  

<br>

### WSUS features

Admin must approve updates before WSUS clients can install them. Observe various approval options while working on practical 5  

WSUS clients can be controlled by <b>Group Policy</b> to connect to WSUS server to check for updates  

After updating, WSUS clients will notify WSUS Server  

<b>WSUS Server can maintain update status of all clients</b>  

<br>

### WSUS operations

WSUS server needs Internet access to Microsoft Update serer to get info about security updates  

<br>

Known as synchronization
* Initial synchronization may take a while depending on the selection choices  

<br>

### Performing Software Update Services Common Admin Tasks

WSUS has 2 logs for tracking events

