Self-evaluation  

1. Attempt to answer the following past exam question:  

In terms of the Microsoft Active Directory Authentication implementation:  

(a) State the default and the alternative authentication schemes for the Microsoft
Active Directory. (4 marks)  

Kerberos is the default authentication scheme, NTLMv2 is an alternative authentication scheme.  

<br>

(b) Briefly describe when will the alternative authentication scheme be applied? (4 marks)  

When Kerberos ports are blocked by firewall or If 1 or both systems are not in the same domain or If there is no Kerberos trust between 2 different forests.  

<br>

(c) Briefly describe the key workflow of the alternative authentication process. (6 marks)  

When a client enters their credentials to logon as a domain user within a domain joined workstation, Windows within the workstation generates a hash based on the credentials entered. They workstation then sends a challenge request to authenticate as that user to the Domain Controller. The Domain Controller then sends a random string back to the client workstation, the client then encrypts the random string using the hash value generated earlier and sends it back to the Domain Controller. The Domain Controller then retrieves the domain user's associated password hash and encrypts the random string. The 2 encrypted random strings are then compared, if they match the user is successfully authenticated, if it does not the authentication fails.  


For Question 2 (10 marks)  

![image](../images/Pasted%20image%2020230814155832.png)  

Client creates an encrypted authenticator, an identifier encrypted with the supplied user's password on a login attempt that is valid within a limited date and time period so that it can't be used in a replay attack. Only a portion of it is unencrypted, which is the username and other info used to know which user sent their authenticator by the KDC.
