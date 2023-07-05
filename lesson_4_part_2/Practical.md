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

