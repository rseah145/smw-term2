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
* [Patch Management Process](#patch-management-process)

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

<br>

Synchronization log keeps track of 
* Time of the last and next scheduled synchronization
* Success and Failure notification
* Update packages that have been downloaded and/orr updated since the last synchronization, or that failed synchronization
* Whether synchronization was a Manual or Automatic synchronization  

<br>

Approval log keeps track of the content that has been approved or not approved  

<br>

### Content Synchronization

During synchronization, new security updates can be handled in 2 ways, automatically approve new versions of previously approved updates or do not automatically approve new versions of approved updates  

In a testing envrionment, 2nd option is better. Otherwise, the testers may overlook and have skipped the testing of the new updates  

<br>

### WSUS Policy Options for Clients

Policy Option(s) and their comments (Policy : comment) (policy and its comment separated by a colon)

<br>

Notify for download and notify for installation: A user logged on with local admin rights will need to select the option to download the update whenever the "New update available for download" notification appears in the taskbar. To complete the installation, the user will then need to install the software update when the "New update available for installation" notification appears.  

<br>

Automatically download and notify for installation: The Automatic Updates client automatically downloads newly approved updates that apply to the client PC. To install the updates, a user logged on with local admin rights will need to select the option to install the software update when the "New software update available for installation" notification appears.  

<br>

Automatically download and schedule the installation: A user logged on with local admin rights can install an update before the scheduled installation time, or delay the restart (if this is required) after installation completes. For users without local admin rights, the update will install in the background at the scheduled time. These users can only delay a PC restart if the "No auto-restart for scheduled Automatic Updates installations" policy setting is enabled.  

<br>

These are the common Windows Update Options for client machines. In an Enterprise network, the Domain admins can set the effective option for all the domain machines via GPOs. Domain users without local nor domain admin rights will not be able to override the GP based setings.

<br>

### WSUS PC Groups

WSUS clients can be placed into PC groups  

<br>

Sample usage: some clients can be put into a "Test" PC group  
* Admin approve new security updates for the "Test" group
* PCs in the "Test" group apply the updates
* Admins test the results before allowing other PCs to apply the updates

<br>

Sample usage: Servers with the same roles can be put into the same group where they can receive relevant updates at the same time  

WSUS PC groups are independent to the Security Grouping and Organizational Unit assignment of the PCs  

These groups are only relevant in the WSUS context  

This feature helps the admins to plan for their patch management strategy. For example, PCs in different groups may receive different set of patches at different schedules.  

Testing groups can be setup to do pre-deployment test for new updates  

<br>

## Patch Management Process

### Managing updates through stages

Five-Stage approach
1. Stage 1: Receive Microsoft Security, release communications  
2. Stage 2: Evaluate Risk  
3. Stage 3: Evaluate Mitigation  
4. Stage 4: Deploy Updates (6 steps)
5. Stage 5: Monitor Systems

Assume the inventory and the system categorization processes are well taken care of by system admins (shorten from 8 to 5 steps in lesson)

<br>

### Stage 1: Receive Microsoft Security Release Communications

Microsoft sends out a notification if there is an issue affecting customer's security  

If security changes are required, a security update is released  

<br>

Patch Tuesday
* Security updates and the corresponding security bulletin normally released on the second Tuesday and sometimes forth of the month.  

<br>

"Exploit Wednesday"
* This term was coined as many exploitation events were seen shortly after the release of a patch  

<br>

Since 2015, Microsoft release security updates to home PCs, tablets and phones as soon as they are ready, while enterprise customers will stay on the monthly update cycle  

Urgent updates will be released immediately  

At stage 1, sys admins should look out for new relevant updates for their systems. As described earlier, for Microsoft updats, the admins can subscribe to the Advisories Alert Email Notifications to get the info in advance. Admins should be familiar with Microsoft Update releases pattern to avoid being a victim of fake patches  

<br>

Microsoft provides several ways of receiving info about updates
* Email: Security Notification Service Comprehensive Edition (remember, update installers are never attached to an email)
* RSS: Comprehensive Alerts (at https://msrc-blog.microsoft.com/feed/ )
* Twitter (at https://twitter.com/msftsecresponse )
* Security Update Portal at https://portal.msrc.microsoft.com/en-us/security-guidance

<br>

### Stage 2: Evaluate Risk

Admins should ask  
* Does the vulnerability apply to the organization? To answer this question, sys admin should have kept an update to date inventory list of all IT assets of the organization  
* Does the vulnerability represent a high risk to the organization?  

<br>

Inventory info is needed in this evaluation process  

<br>

The deployment of a security update has a cost
* Costs of testing the updates
* Costs of 'deploying' the updates
* Support costs in case of any negative result after the update (Example: important app does not work properly after the update)

<br>

Outcome of evaluation may include
1. Not apply the patch - because the update is not relevant to our inventory or the risk is very low which the cost to deploy is very high  
2. Defer the deploy until the next scheduled system maintenance down time: For the time being, choices are - bare the risk or apply some interim mitigation procedures. Such as shutting down some non mission critical services, or configure some adhoc firewall rules to protect the affected servers  
3. In the most unusual case, an emergency patch operation will be needed ASAP and it will disrupt the normal operations  

<br>

Flow chart showing stage 2: evaluate risk  
![image](../images/Pasted%20image%2020230711213735.png)

<br>

### Severity ratings

Microsoft has 4 severity ratings
* <b>Critical</b>: A vulnerability whose exploitation could enable the propagation of an Internet worm with little or no user action  
* <b>Important</b>: A vulnerability whose exploitation could result in compromise of the confidentiality, integrity, or availability of user's data, or of the integrity or availability of processing resources  
* <b>Moderate</b>: A vulnerability whose exploitation is mitigated to a significant degree by factors such as default configuration, auditing, or difficulty of exploitation  
* <b>Low</b>: A vulnerability whose exploitation is extremely difficult, or whose impact is minimal  

<br>

### Snapshot of security updates

Snapshot  
![image](../images/Pasted%20image%2020230711215903.png)

<br>

### Stage 3: Evaulate Mitigation

While admins are evaluating security updates, some short-term security control may be applied  

For example adjust firewall policies, or restrict a port only to a specific subnet instead of the whole network  

Stage 3 and 4 usually go in parallel  

Goal of stage 3 is to identify and apply a short-term fix to manage the vulnerability of relevant systems (safeguard systems until the patch is applied)  

Microsoft may provide some suggested mitigation or workarounds in their security advisories if the security update cannot be applied immediately  

Such mitigations and workarounds are meant for short-term use, they do nto replace security updates  

<br>

### Sample of mitigation factors from Microsoft security update  

Sample  
![image](../images/Pasted%20image%2020230711220222.png)

<br>

### Stage 4: Deploy Updates

Six steps to deploy an update  

<br>

Steps
1. Plan the deployment. Determine which updates need to be rolled out quickly and which ones can be subjected to a more thorough testing process. Deployment schedule - do some PC groups need to updates more urgently? By which date or time, must all PCs be updated?  
2. Determine whether the security update is available for download. If there is no security update available, Microsoft would advise the appropriate actions to take to protect the PC systems  
3. Obtain the required update files (from a reliable source). Security update files can be obtained from several sources like Microsoft security guide, Microsoft deployment tools, such as Microsoft Update, Windows Update, WSUS, or Endpoint Manager. Microsoft Download Center and Microsoft Update Catalog service are 2 more reliable sources  
4. Create the update package. If security updates need to be customised  
5. Test the package. To ensure that business-critical systems will continue to run successfully after the security update has been deployed. To ensure the package can be 'uninstalled' or <b>there is a way to roll back</b>. To ensure the system can be restarted properly. To ensure the update is <b>effective</b><script>alert(1)</script>