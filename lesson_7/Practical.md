# Navigation
* [Add appropriate server role to Server 2016 to facilitate an Enterprise Root CA](#add-appropriate-server-role-to-server-2016-to-facilitate-an-enterprise-root-ca)
* [Setting up the default Enterprise autoenrollment policy](#setting-up-the-default-enterprise-autoenrollment-policy)
* [Adding in HTTPS protocol support to your certsrv website](#adding-in-https-protocol-support-to-your-certsrv-website)

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

7. Repeat the same steps used to configure web enrolment for configuring the online responder role service, which also does not require further input (image taken from prac, but it is the same thing)  
![image](../images/Pasted%20image%2020230815193257.png)  

<br>

## Setting up the default Enterprise autoenrollment policy  

1. On the PDC, edit the Default Domain Policy GPO via GPMC and GPME  
![image](../images/Pasted%20image%2020230815193926.png)  

2. In GPME, navigate to PC Configuration -> Windows Settings -> Security Settings -> Public Key Policies -> Automatic Certificate Request Settings  
![image](../images/Pasted%20image%2020230815194112.png)  

3. Right-click on the right panel and click on "New" -> "Automatic Certificate Request" to open the configuration wizard  
![image](../images/Pasted%20image%2020230815194228.png)  
![image](../images/Pasted%20image%2020230815194303.png)  

4. In the Automatic Certificate Request Setup Wizard, click next and only select the "Computer" certificate template, click next again to go to the next page  
![image](../images/Pasted%20image%2020230815194511.png)  

5. Click finish to confirm the setup  
![image](../images/Pasted%20image%2020230815194604.png)  

6. Completed setup for autoenrollment  
![image](../images/Pasted%20image%2020230815194644.png)  

<br>

## Adding in HTTPS protocol support to the certsrv website  

1. On the 