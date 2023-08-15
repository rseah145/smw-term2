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
![image](../images/Pasted%20image%2020230815192606.png)  

At CA name page use all the default values

<br>

For the rest of the pages, review and use the default settings  

<br>

Click configure button on the confirmation page  
![image](../images/Pasted%20image%2020230815192733.png)  

Click Yes when a pop up for configuring the remaining Role services is shown for the 2 remaining service, the web enrollment and the online responder  
![image](../images/Pasted%20image%2020230815192917.png)  

<br>

5. For credentials page, use the same credentials for setting up the CA  

6. Select the next role server and click next  
![image](../images/Pasted%20image%2020230815193047.png)  

Web Enrolment configuration has no prompt for any options  

7. Repeat the same steps used to configure web enrolment for configuring the online responder role service, which also does not require further input  

![[Pasted image 20230815193257.png]]