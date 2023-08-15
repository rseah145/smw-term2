# Navigation
* [Add appropriate server role to Server 2016 to facilitate an Enterprise Root CA](#add-appropriate-server-role-to-server-2016-to-facilitate-an-enterprise-root-ca)

## Add appropriate server role to Server 2016 to facilitate an Enterprise Root CA  

1. Using a provided Win Server 2019 image. Perform a domain join  
![image](../images/Pasted%20image%2020230815190819.png)  

2. Set static IPv4 settings after joining the domain  
![image](../images/Pasted%20image%2020230815191147.png)  

3. Installation of the AD CS role
![image](../images/Pasted%20image%2020230815191250.png)  
![image](../images/Pasted%20image%2020230815191346.png)  

<br>

Enable Basic Authentication and Centralized SSL Certificate Support Services at IIS  
![image](../images/Pasted%20image%2020230815191500.png)  

<br>

Review all options, check "Restart the destination server automatically if required" box and click "install"  
![image](../images/Pasted%20image%2020230815191607.png)  

<br>

4. After the role(s) have been added to the server click on the flag icon on Server Manager and start the post-deployment configuration for AD CS (Use a user that belongs to the domain admins and enterprise admins groups)  
![image](../images/Pasted%20image%2020230815192120.png)  

In Role Services select CA only and click next  
![image](../images/Pasted%20image%2020230815192321.png)  

At type select Enterprise CA  
![image](../images/Pasted%20image%2020230815192405.png)  

At CA type select Root CA  
![image](../images/Pasted%20image%2020230815192442.png)  

At private key choose create a new private key  
![image](../images/Pasted%20image%2020230815192517.png)  

Use the following settings at the cryptography page  
