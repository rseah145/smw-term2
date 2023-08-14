
# Notes from video 1 ["MicroNugget: How Does Kerberos Work? by Don Jones"](https://www.youtube.com/watch?v=kp5d8Yv3-0c)  

Native authentication protocol in Active Directory, used by Windows networks everywhere.  

3 different components, a client, file server, and a Key Distribution Center (in AD, would be within the DC).  
![image](../images/Pasted%20image%2020230814110358.png)  

<br>

Client takes on most of the processing burden in Kerberos.  

Kerberos does not transmit user's passwords across the network, it uses them as shared secrets so that the KDC can decrypt the authenticator using the same password per user that authenticates (Symmetric encryption).  

<br>

## Kerberos Client Logon Authentication Process

1. Client creates an encrypted authenticator, an identifier encrypted with the supplied user's password on a login attempt that is valid within a limited date and time period so that it can't be used in a replay attack. Only a portion of it is unencrypted, which is the username and other info used to know which user sent their authenticator by the KDC. (As represented by the image beside the client and its key)   
![image](../images/Pasted%20image%2020230814111749.png)  

2. KDC receives the client's authenticator, see's the unencrypted portion, looks up info for that user from the cleartext, grabs the password for that user from their database and attempts to decrypt the authenticator with it (shared secret/ same pink key).  
![image](../images/Pasted%20image%2020230814112355.png)  
![image](../images/Pasted%20image%2020230814112804.png)  

3. (Failed to authenticate) If the client's authenticator can't be decrypted it means that the authenticator sent by the client was encrypted with a password that the KDC does not know about, meaning it fails to authenticate due to the user supplying an invalid password.  

3. (Successful authentication) If the client's authenticator can be decrypted it means that the authenticator sent by the client was encrypted using the same password that is stored within the KDC database, which authenticates the user.  

4. After successfully authenticating a user, the KDC discards the client's authenticator and creates a Ticket Granting Ticket (TGT) and encrypts a portion of the TGT using its own encryption key