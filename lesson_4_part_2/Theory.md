# Continuation from slide 44 of lecture 4 slides (theory)  


# Objectives
Intro to GP  


# Navigation
* [Intro to GP](#introduction-to-group-policy-gp)
* [Securing Win Server 2016 using Security Policies](#win-server-2016-using-security-policies)
* [Establishing Account policies](#establishing-account-policies)
* [Establishing Audit policies](#establishing-audit-policies)
* [Configuring User Rights](#configuring-user-rights)
* [Configuring Security Options](#configuring-security-options)
* [List of settings under "User Configuration"](#list-of-settings-under-user-configuration)
* [Policy Application Order and Troubleshooting](#policy-application-order-and-troubleshooting)
* [Policy Application Order and Example](#policy-application-order-and-example)
* [Resultant Set of Policy and gpresult](#resultant-set-of-policy-and-gpresult)


<br>

## Introduction to Group Policy (GP)

GP in Win Server 2016 allows standardization of working environment of clients and servers via policies in AD.  <br>

#### Characteristics of GP

* Can be set for a <b>site</b>, <b>domain</b>, <b>OU</b>, or <b>local computer</b>
* Cannot be set for non-OU folder containers
* Settings are stored in GPO
* GPOs can be local and nonlocal
* Can be set up to affect all user accounts and PCs
* When GP is updated, old policies are removed or updated for all clients

<br>

PC configuration applies to computers  
![image](../images/Pasted_image_20230629173324.png)

<br>

User configuration applies to users  
![image](../images/Pasted_image_20230629173342.png)


<br>

## Win Server 2016 using Security Policies

Security policies are a subset of individual policies within a larger GP for a site, domain, OU, or local PC

<br>

Security policies
* <b>Account Policies</b>
* Audit Policy
* <b>User Rights</b>
* Security Options
* IP Security Policies (IPSec)
<br>

### Establishing Account Policies

Acc policies == all security measures set up in GP that applies to all accs or to all accs in a container when AD is installed.  

Acc policies affect 3 main areas. Password, Account, and Kerberos security.  
<br>

PW security
* 1 option is to set a pw expiration period, requiring users to change pws at regular intervals
* Some orgs require that all pws have a minimum length  
<br>

Specific pw security options
* Enforce pw history
* Max. password age
* Min. password age
* Min. password length
* Pws must meet complexity requirements
* Store pw using reversible encryption
<br>

Acc Lockout, where the OS can employ acc lockout to bar access to an account (including true account owner) after a number of failed login attempts

Lockout can be set to released after a specified period of time, or by intervention from the server admin

Common policy is to have a lockout after 5 to 10 failed logon attempts, admin can set lockout to release after a designated time

<br>

Acccount lockout parameters
* Acc lockout duration
* Acc lockout threshold
* Reset acc lockout count after
<br>

Acc Lockout is mainly used to prevent brute force attacks on passwords, but it can cause Denial of Service attacks
* With a lockout of 30 mins, an attacker can lockout all user accounts with incorrect pws to cause a DoS to the system via unable to login  
<br>

### Establishing Audit Policies

Examples of events an org can audit
* Account logon (and logoff) events
* Acc management
* Directory service access
* Logon (and logoff) events at the local computer
* Object access
* Policy change
* Privilege use
* Process tracking
* System events

Audit policies are a passive defensive measure (wait and see)
<br><br>

### Configuring User Rights

Enable an acc or grp to perform predefined tasks  

The most basic right is the ability to access a server, more advanced rights give privs to create accs and manager server functions

<br>

2 general categories of rights
* Privileges, relate to the ability to manage server or AD functions
* Logon rights, to how accs, PCs, and services are accessed
<br>

Becareful to not mix up user rights with permissions, e.g.
* Right to read from file vs perm to read from file
* Right to logon to server vs perm to logon to server
<br>

<b>Examples of privs</b> (* important)
* Add workstations to domain
* Back up files and directories
* Change system time
* Create permanent shared objects
* Generate security audits
* Load and unload device drivers
* Perform volume maintenance tasks
* Shutdown system

Perms may work with user rights side by side, some operational task may need both perm and user right to enable users to carry out tasks

<br>

Examples of logon rights
* Access this PC from network
* Allow logon locally
* Allow logon through Remote Desktop Services (RDP)
* Deny access to this PC from network
* Deny logon as a service
* Deny logon locally
* Deny logon through RDP

User rights is generally any option under "User Rights Assignment" from  
PC config -> Policies -> Windows Settings -> Security Settings -> Local Policies -> User Rights Assignment  
within GP management editor

<br>

### Configuring Security Options

Specialized security options divided into categories
* Accounts
* Audit
* DCOM
* Devices
* Domain Controller
* Interactive Logon
* Microsoft Network Client
* Network access
* Network security
* Recovery console
* Shutdown
* System cryptography
* System objects
* System settings
* User Account Control (UAC)

UAC is the most common feature a user can encounter out of the listed categories  
Right below "User Rights Assignment" in GP management editor

<br>

### List of settings under "User Configuration"

Further reference for user configurations of GP

Link: https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/bb742376(v=technet.10)?redirectedfrom=MSDN

![image](../images/Pasted%20image%2020230702165607.png)

<br>

### Policy Application Order and Troubleshooting

Policies can be applied at Domain level, at OU level, etc

Any settings that do not conflict with other settings will be applied

What about policy settings that conflict?

Example
* Policy at Domain level states that only Admins can shutdown PCs
* Policy at Sales OU level states that Admins and Mgr1 can shutdown PCs

Every GPO contains the same no. of possible system config setting entries  <br>
There were at least  5000 plus entries during the Windows 7 and Windows Server 2008 era.  <br>
The actual number has been kept growing for every major release of newer windows systems.  <br>
Fortunately, each of these entries can be in one of the two states: defined or not defined.  <br>
The domain controller only send the 'defined' entries to the target machine, thus, sending a GPO to a system may not involve a lot of network bandwidth.  <br>
Policy settings conflict only occurs when two different GPOs have defined the same entries (but with different definitions) , and both of them are received by the same domain client system.  

<br>

Important to understand the order in which settings in the GP should be configured

Usual order in which GPs are applied is <b>LSDOU</b>
1. Local
2. Site
3. Domain
4. OU

<br>

Settings that conflict will be applied based on the usual order of application

<b>Last setting</b> applied will take effect

[Supplimentary video for GPO precedence](https://www.youtube.com/watch?v=orQns7K-brM)

[Another supplimentary video](https://www.youtube.com/watch?v=iS_DV_zH5aU)

Policy Application Order AKA GP precedence determines final setting that is used when there are GPO setting conflicts

<br>

From the example
* Policy at Domain level states that only Admins can shutdown PCs
* Policy at Sales OU level states that Admins and Mgr1 can shutdown PCs
<br>

The Policy at Sales OU level will be applied last
* Admins and Mgr1 can shutdown PCs in the Sales OU
* Only Admins can shutdown PCs that are not in Sales OU
<br>

Note
* This only applies to PCs that receive the GPO
* Since PCs that are not in the Sales OU will not receive the particular GPO thus Mgr1 only has limited shutdown privs on certain grps of PCs
<br>

In Win Server 2003 and previous versions, settings related to Acc Policies can only be set at the domain level

From Win Server 2008, its possible to have different password policies and acc lockout policies for different OUs

<br>

### Policy Application Order and Example

Special Config Options
* Blocking GP inheritance
	* Container option to prevent objects from inheriting GPO
* Enforced
	* GPO option to force policies to all child containers and win all setting conflicts (highest precedence)
* Multiple GP Link Order Setting
<br>


[Reference](https://learn.microsoft.com/en-us/archive/blogs/musings_of_a_technical_tam/group-policy-basics-part-2-understanding-which-gpos-to-apply) material for Blocking Policy Inheritance and Enforced Policies

Blocking inheritance and enforced GPO are the features that may override or alter the normal policy application order effect

Keep in mind that Block Inheritance is a Container option

Enforced GPO is an attribute of GPO

<br>

#### Case studies

Scenario 1

ABC Company has a Policy at domain level that sets every user's desktop wallpaper to their logo

Domain policies will be inherited by all objects

![image](../images/Pasted%20image%2020230702172827.png)

<br>

Scenario 2

If there are 2 Policies at domain level to set different desktop wallpaper

Policy with lowest Link Order number is applied last

![image](../images/Pasted%20image%2020230702173037.png)

<br>

Scenario 3

Sales OU Admin creates a Policy for Sales OU to put latest sales figures as desktop wallpaper

By <b>LSDOU</b>, OU policy is applied last

![image](../images/Pasted%20image%2020230702173338.png)

<br>

Scenario 4

Sales OU Admin can also choose to block Inheritance, meaning policies will not be inherited

There is no Sales OU GP

![image](../images/Pasted%20image%2020230702173744.png)

<br>

Scenario 5

Domain Admin can enforce his policy, lower level admins cannot block the policy

![image](../images/Pasted%20image%2020230702173846.png)


#### Conclusion for Policy Application Order

To avoid complexity in GPs
* Limit no. of GPOs (Can a single domain policy apply to all?)
* Minimize use of Block Inheritance and Enforced to avoid complexity

<br>


### Resultant Set of Policy and gpresult

<b>Resultant Set of Policy</b>
* Used to make the implementation and troubleshooting of GPs much simpler for an admin
* Can query the existing policies that are in place and then provide reports and the results of policy changes
* RSoP supports 2 modes, planning and logging
* Available within Group Management Policy Console (GMPC) & Command Line Interface (CLI)

<br>

GPRESULT
* Is a command to be ran on a workstation to check its current accepted and active GPOs