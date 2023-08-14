
# Navigation
* [Kerberos Client Logon Authentication Process](#kerberos-client-logon-authentication-process)
* [Accessing file servers after Kerberos authentication](#accessing-file-servers-after-kerberos-authentication)
* [klist.exe goto commands for troubleshooting Kerberos Authentication](#klist.exe-goto-commands-for-troubleshooting-kerberos-authentication)
* [LM Theory](#lm-theory)
* [NTLM Theory](#ntlm-theory)
* [NTLM Authentication](#ntlm-authentication)
* [Kerberos deep dive theory](#kerberos-deep-dive-theory)

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

### Accessing file servers after Kerberos authentication  

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

kerbtray.exe can be downloaded and used to view all of the tickets within the Kerberos Tray of a client, just use klist if possible.  

<br>

## Notes from video 2 ["Using Command Line Tools to Troubleshoot Kerberos Authentication by Je5ff Hicks"](https://www.youtube.com/watch?v=wNSfFBhLywk)  

For Windows XP, use kerbtray.exe (sits in the system tray, bottom right corner)  

For Windows 7 and newer, use klist.exe (Does not require special privileges to run)  

### klist.exe goto commands for troubleshooting Keberos Authentication  

Note: Pipe into more for viewing line-by-line is optional " | more", just remove " | more" from the command to view it without viewing the output line-by-line.  

#### Help menu  
klist -?  

#### Show Ticket Granting Ticket (TGT) info that the KDC has issued to the account
klist tgt | more  

#### Running klist without parameters defaults to "klist tickets" being ran, shows all tickets assigned/issued to the current user 
klist | more  

#### Clear ticket cache (Delete all tickets from Kerberos Tray, no warning prompt will be given), also purges TGT  
klist purge  

<br>

Access a domain resource (e.g. file server/ remote server share) after purging the TGT/ tickets to reinitiate the whole Kerberos process to get a TGT from the KDC, and Kerberos tickets to access the required domain resources.  

<br>

## Notes from video 3 ["NTLM - CompTIA Security+ SY0-401: 6.2 by Professor Messer"](https://www.youtube.com/watch?v=oWaI5-pgthI)  

### LM Theory  

#### LAN Manager (LANMAN), Microsoft and 3Com Network OS  

Before Windows NT  

<br>

#### LANMAN challenge/response  

Hash challenge, similar to CHAP  

<br>

Somewhat insecure  
* All uppercase ASCII  
* Password is 14-characters max  
* Passwords over 7 characters are split and encrypted separately  
* Passwords are not salted  

<br>

### NTLM Theory  

#### NT LAN Manager/ NTLMv1  

Used in early versions of Windows NT  
* Password is Unicode  
* Up to 127 characters long  
* Stored as a 128-bit MD4 hash, instead of DES in LANMAN  

<br>

#### NTLMv2  

Came out with Windows NT SP4 (Service Pack 4)  

<br>

New password response  
* MD4 password hash (same as NTLMv1)  
* HMAC-MD5 hash of username and server name  
* Variable-length challenge of timestamp, random data, domain name  

<br>

#### NTLM Vulnerabilities (NTLMv1 and NTLMv2)  

Some Windows password databases contain LM hash versions of the passwords  
* Compatibility with older systems  
* Legacy systems kept insecure NTLMv1 password hashes alongside the more secure NTLMv2 password hashes (If attackers got access to NTLMv1 database it would be much easier to decrypt hashes and gain access)

<br>

NTLM vulnerable to a credentials forwarding attack (Look up "Pass-the-hash" and NTLM Relay attacks)  
* Use credentials of 1 PC to gain access to another

<br>

## Notes from video 4 ["System Hacking & Penetration testing | NTLM Authentication by Jai Ashish"](https://www.youtube.com/watch?v=vR8784gTBP0)  

### NTLM Authentication  

Proprietary to Microsoft.  

#### When it is used

<br>

NTLM is used when
* There is no Kerberos trust between 2 different forests  
* Authentication is attempted by IP instead of DNS (Kerberos requires DNS)  
* If 1 or both systems are not in the same domain  
* If your firewall is blocking Kerberos ports (88 for TCP and UDP, Keberos 5 ticket service)

<br>

#### How it is used

Challenge response algorithm  

Passwords are not transmitted  

V1 came with NT, just say NO  

V2 came with NT SP4  

<br>

#### How it works

1. User types in username and password on Windows login screen  
![image](../images/Pasted%20image%2020230814143901.png)  

2. Windows OS takes the password entered and runs it through a hash algorithm to generate a hash for the password entered  
![image](../images/Pasted%20image%2020230814144016.png)  

3. Client sends a login request to the DC, as the PC has joined the domain, the DC knows the hash value associated to the login name (username) (DC knows all hash values associated to each domain user)  
![image](../images/Pasted%20image%2020230814144302.png)  

4. DC creates a random string and sends it to the client  
![image](../images/Pasted%20image%2020230814144808.png)  

5. Client encrypts the random string with the hash from step 2 and sends it back to the DC  
![image](../images/Pasted%20image%2020230814145150.png)  

6. DC retrieves the user's hash from its database, encrypts the random string using the retrieved hash and compares the encrypted random strings, 1 received from the client and 1 from the DC's database. If it matches (same resulting encrypted random string), it allows you to authenticate (login) to the client PC as that user, otherwise it returns a failed login attempt  
![image](../images/Pasted%20image%2020230814145641.png)  

<br>

## Notes from video 5 ["Kerberos Authentication Explained | A deep dive"](https://www.youtube.com/watch?v=5N242XcKAsM)  

More of an add-on to theory for video 1

Passwords are never sent across the network  

Encryption keys are never directly exchanged  

Client and Server can mutually authenticate each other  

Many orgs use it as basis for single sign on (SSO)  

<br>

### Kerberos deep dive theory  

#### Kerberos Realm

"Kerberos Realm" is the domain, the group of systems over which Kerberos has the authority to authenticate a user to a service  

Possible to have multiple Kerberos Realms and they can be interconnected

<br>

#### Components within the Realm

Within a Kerberos Realm there are "principal", a unique identity (either a user or a service/application)  

Clients/Users is a process that accesses a service on behalf of a user, there can be multiple clients/users within a realm (users that access stuff)  

Service is a resource provided to a client (e.g. a file server or an app that a user wants to access), there can be multiple services that clients can access  

Overview of "principal", "client/user", and "service" within Kerberos Realm  
![image](../images/Pasted%20image%2020230814151343.png)  

<br>

#### KDC

Supplies tickets and generates temporary session keys that allow a user to securely authenticate to a service  

KDC stores all the secret symmetrical keys for users and services  

2 servers within the KDC, the authentication server and the Ticket Granting Server (TGS)  

Authentication server confirms that a known user is making an access request and issues Ticket Graning Tickets (TGT)  

Ticket Granting Server confirms that a user is making an access request to a known service and issues service tickets  

Many messages are sent back and forth between users, services and the 2 servers within the KDC  

The 2 important types of messages to highlight are "Authenticators" and "Tickets"  

Authenticators are a record containing info that can be shown to be recently generated using the session key only known to the client and the server. <b>Allows the user to authenticate to the service, and the service to authenticate to the user (mutual authentication)</b>  

Tickets contain most of the info that needs to be passed. Client's identity, service ID, session keys, Time to Live, ...etc all encrypted using a server's secret key  

Overview with KDC, the 2 servers, and messages  
![image](../images/Pasted%20image%2020230814152815.png)  

<br>

#### Communication required for a user to access a service

1. User sends an unencrypted message to the authentication server, wanting to access a service  
![image](../images/Pasted%20image%2020230814153116.png)  

2. Authentication server validates the request that is coming from a known user, generates a TGT and sends it back to the user along with a message encrypted with the user's secret key  
![image](../images/Pasted%20image%2020230814153336.png)  
![image](../images/Pasted%20image%2020230814153409.png)  

3. User decrypts the message with their secret key and creates new messages