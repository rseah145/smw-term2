# Important stuff, not everything is covered

# From 4-6 onwards


# Navigation
* [Customizing and deployment of GP](#customizing-and-deployment-of-gp)
* [Group Policy Application Order](#group-policy-application-order)

<br>

## Customizing and deployment of GP

Linking GPO to an OU via "Active Directory Users and Computers" on DC
![image](../images/Pasted%20image%2020230705215234.png)

<br>

Cached logins by disconnecting the network, a user that has never logged in already to the workstation via a domain account can't as there's no connection to the DC. However, a user that has logged in before (e.g. Mgr1) can login again as the logon was cached.
![image](../images/Pasted%20image%2020230705215906.png)

<br>

## Group Policy Application Order

Modified "Default Domain Policy" policy in GPME with "gpupdate /force" ran afterwards takes effect on the lock screen for win server 2012 (memsrv1)
![image](../images/Pasted%20image%2020230705220408.png)
![image](../images/Pasted%20image%2020230705220502.png)

<br>

MemberServerOU to block inheritance of the Default Domain Policy (current user info will be displayed again) via GPM on the DC (! icon shows blocked inheritance)
![image](../images/Pasted%20image%2020230705220735.png)
![[Pasted image 20230705220836.png]]