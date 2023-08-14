
# Navigation
* [Kerberos Client Logon Authentication Process](#kerberos-client-logon-authentication-process)
* [Accessing file servers after authentication](#accessing-file-servers-after-authentication)

<br>

## Notes from video 1 ["MicroNugget: How Does Kerberos Work? by Don Jones"](https://www.youtube.com/watch?v=kp5d8Yv3-0c)  

Native authentication protocol in Active Directory, used by Windows networks everywhere.  

3 different components, a client, file server, and a Key Distribution Center (in AD, would be within the DC).  
![image](../images/Pasted%20image%2020230814110358.png)  

<br>

Client takes on most of the processing burden in Kerberos.  

Kerberos does not transmit user's passwords across the network, it uses them as shared secrets so that the KDC can decrypt the authenticator using the same password per user that authenticates (Symmetric encryption).  

Ticket Granting Tickets (TGTs) are usually usable for 8 hours by default. After the ticket expires, it will have to be destroyed and renewed so the KDC can re-validate a user from time to time.  

Note that the Domain Controller/ KDC will have all domain members logon passwords, which is why Kerberos tickets can be encrypted/decrypted using a shared secret (passwords).  

<br>

### Kerberos Client Logon Authentication Process

1. Client creates an encrypted authenticator, an identifier encrypted with the supplied user's password on a login attempt that is valid within a limited date and time period so that it can't be used in a replay attack. Only a portion of it is unencrypted, which is the username and other info used to know which user sent their authenticator by the KDC. (As represented by the image beside the client and its key)   
![image](../images/Pasted%20image%2020230814111749.png)  

2. KDC receives the client's authenticator, see's the unencrypted portion, looks up info for that user from the cleartext, grabs the password for that user from their database and attempts to decrypt the authenticator with it (shared secret/ same pink key).  
![image](../images/Pasted%20image%2020230814112355.png)  
![image](../images/Pasted%20image%2020230814112804.png)  

<br>

3. (Failed to authenticate) If the client's authenticator can't be decrypted it means that the authenticator sent by the client was encrypted with a password that the KDC does not know about, meaning it fails to authenticate due to the user supplying an invalid password.  

<br>

3. (Successful authentication) If the client's authenticator can be decrypted it means that the authenticator sent by the client was encrypted using the same password that is stored within the KDC database, which authenticates the user.  

<br>

4. After successfully authenticating a user, the KDC discards the client's authenticator and creates a Ticket Granting Ticket (TGT) and encrypts a portion of the TGT using its own encryption key. In short, the authenticated user's info is saved in the TGT and is encrypted using the KDC's encryption key. (Blue key and ticket + lock)  
![image](../images/Pasted%20image%2020230814113744.png)  

5. The KDC sends the TGT back to the client and the TGT is then stored within a special location in-memory of the client's PC called the "Kerberos Tray" that cannot be swapped out/saved to disk. When the PC crashes or it is shutdown/restarted all info within the Kerberos Tray is lost.   
![image](../images/Pasted%20image%2020230814114051.png)  

<br>

### Accessing file servers after authentication  

1. Client is logged on after they have received a TGT from the KDC and stored it within their Kerberos Tray.  

<br>

2. PC looks at the Kerberos Tray and does not find a ticket for the File Server. It then sends a copy of the TGT from the Kerberos Tray along with a request for a ticket to the file server to the KDC.  
![image](../images/Pasted%20image%2020230814121309.png)  

3. The KDC uses its key to decrypt the TGT. (if successful it authenticates, otherwise it does not authenticate) (Blue key == KDC's key in this diagram)

<br>

4. Once authenticated via the TGT sent by the client, the KDC generates a ticket for the File Server which is encrypted using the File Server's logon password. (No communication with the file server, ticket is created by the KDC)  
![image](../images/Pasted%20image%2020230814121903.png)  

5. The File Server ticket is then sent to the client and is stored within the client's Kerberos Tray.  
![image](../images/Pasted%20image%2020230814122143.png)  

6. For the next 8 hours (or for how long the ticket is set to be valid), whenever the client wants to access a resource on the file server, it just creates a copy of the file server ticket from its Kerberos Tray and sends it to the file server.  
![image](../images/Pasted%20image%2020230814122431.png)  

7. The file server uses its own password to decrypt the ticket sent by the client. (If successful, authenticates and verifies that the ticket was created by the KDC, the only other server to know its password, otherwise it fails to authenticate)  
![image](../images/Pasted%20image%2020230814122608.png)  
![image](../images/Pasted%20image%2020230814122655.png)  

8. [Important] After successfully authenticating via the ticket, the file server opens the ticket, gets the username and other info such as what groups the user is a member of and uses that info to decide what resources the user can access on the file server. <b>Every time the client wants to access the file server, it has to send/resend a copy of the file server ticket from their Kerberos Tray.</b>  
![image](../images/Pasted%20image%2020230814123302.png)  

<br>

The reason that the client needs to send/resend a copy of their file server ticket each time they want to access the file server is so that the file server does not need to store each user's/client's ticket in-memory. Because the file server works with many users, if it had to keep each user's/client's tickets it would be a pain to manage.  

kerbtray.exe can be downloaded and used to view all of the tickets within the Kerberos Tray