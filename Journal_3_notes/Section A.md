
# Notes from video 1 ["MicroNugget: How Does Kerberos Work? by Don Jones"](https://www.youtube.com/watch?v=kp5d8Yv3-0c)  

Native authentication protocol in Active Directory, used by Windows networks everywhere.  

3 different components, a client, file server, and a Key Distribution Center (in AD, would be within the DC).  
![image](../images/Pasted%20image%2020230814110358.png)  

<br>

Client takes on most of the processing burden in Kerberos.  

<br>

## Kerberos Client Logon Authentication Process

1. Client creates an encrypted authenticator, an identifier with its secret key that is valid within a limited date and time period so that it can't be used in a replay attack. Only a portion of it is unencrypted, which is the username and other info used to know which user sent their authenticator by the KDC. (As represented by the image beside the client and its key)   
![image](../images/Pasted%20image%2020230814111749.png)  

2. 