# Important stuff, not everything is covered

# From 4-6 onwards


# Navigation
* [Customizing and deployment of GP](#customizing-and-deployment-of-gp)
* [Group Policy Application Order](#group-policy-application-order)
* [Disabling removable storage access using Group Policy (computer configuration settings)](#disabling-removable-storage-access-using-group-policy-computer-configuration-settings)
* [Using User Configuration to apply GPO settings to specific users](#using-user-configuration-to-apply-gpo-settings-to-specific-users)

<br>

## Customizing and deployment of GP

Linking GPO to an OU via "Active Directory Users and Computers" on DC
![image](../images/Pasted%20image%2020230705215234.png)

<br>

Cached logins by disconnecting the network, a user that has never logged in already to the workstation via a domain account can't as there's no connection to the DC. However, a user that has logged in before (e.g. Mgr1) can login again as the logon was cached.
![image](../images/Pasted%20image%2020230705215906.png)

<br>

## Group Policy Application Order

Modified "Default Domain Policy" policy in GPME with "gpupdate /force" ran afterwards on win server 2012 for the lock screen changes to take effect on it (memsrv1)
![image](../images/Pasted%20image%2020230705220408.png)
![image](../images/Pasted%20image%2020230705220502.png)

<br>

MemberServerOU to block inheritance of the Default Domain Policy (current user info will be displayed again) via GPM on the DC (! icon shows blocked inheritance). "gpupdate /force" was ran again for the changes to take effect on win server 2012 (memsrv1)
![image](../images/Pasted%20image%2020230705220735.png)
![image](../images/Pasted%20image%2020230705220836.png)

<br>

Lock screen showing current user that's logged in again
![image](../images/Pasted%20image%2020230705221239.png)

<br>

Domain admin enforcing Default Domain Policy so admins of OUs can't block its policies (Default Domain) (last img shows effect)
![image](../images/Pasted%20image%2020230705221442.png)
![image](../images/Pasted%20image%2020230705221506.png)
![image](../images/Pasted%20image%2020230705221807.png)

<br>

## Disabling removable storage access using Group Policy (computer configuration settings)

In DC, using ADUC 
Win 10 Client (SMWCLIENT1), User1, and Staff1 are in SalesOU  
Below that memsrv1 (Win Server 2012) is in MemServerOU
![image](../images/Pasted%20image%2020230705222314.png)
![image](../images/Pasted%20image%2020230705222459.png)

<br>

In GPMC, a new GPO is made "DisableUSBWin10GPO" (right-click GPOs -> New GPO)
![image](../images/Pasted%20image%2020230705222653.png)

<br>

Applying 3 Computer Configuration settings in "Removable Storage Access" to the newly created GPO below  

<br>

(Right-click created GPO -> edit -> PC config -> Policies -> Administrative Templates -> System -> Removable Storage Access)  
Deny read and write access (Enabled)  
![image](../images/Pasted%20image%2020230705223042.png)

<br>

(Policies -> Windows Settings -> Security Settings -> System Services)  
Portable Device Enumerator Service (Automatic)  
(Portable Device Enumerator Service is required to be running to allow the removable storage access control management via the Administrative Template settings.)  
![image](../images/Pasted%20image%2020230705223356.png)

<br>

## Hands-on with GPO Scope and WMI Filter

Creation of a WMI Filter on the DC GPMC for Windows 10 systems  
(Note: 10.%  matches with Windows 11, Windows 10 and Windows Server 2016.)  
(Also note: Link the GPO to the "salesOU" and "MemberServerOU" first)  
![image](../images/Pasted%20image%2020230705233136.png)


<br>

Applying the newly created WMI Filter to the DisableUSBWin10GPO in scope  
(Select the filter in "WMI Filtering" at the bottom)
![image](../images/Pasted%20image%2020230705234327.png)

<br>

Run "gpupdate /force" on both Win Server 2012 (memsrv1) and Win10 VM to get the latest GPOs (images not shown)

<br>

On Win Server 2012R2 (memsrv1) with a cmd or powershell session with admin rights, the command "gpresult /r" verifies the current GP config and shows that system has filtered out DisableUSBWin10GPO (computer settings) (GPO does not apply to unmatched )  
![image](../images/Pasted%20image%2020230705234454.png)

<br>

## Using User Configuration to apply GPO settings to specific users

Edit the DisableUSBWin10GPO. Clear deny read and write access settings under pc config section (not shown)  
From "enabled" to "not configured"  

<br>

Edit the DisableUSBWin10GPO. Apply 2 user config settings as shown run gpupdate /force as per usual (basically shift the settings to under "User Configuration" instead of "Computer Configuration")  
![image](../images/Pasted%20image%2020230705235330.png)  

All GPO testing is not noted down  

Do reflection prompt  

<br>

## Report Resulting Set of Policy currently deployed on Server1