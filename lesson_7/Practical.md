# Navigation
* [Add appropriate server role to Server 2016 to facilitate an Enterprise Root CA](#add-appropriate-server-role-to-server-2016-to-facilitate-an-enterprise-root-ca)
* [Setting up the default Enterprise autoenrollment policy](#setting-up-the-default-enterprise-autoenrollment-policy)
* [Adding in HTTPS protocol support to the certsrv website](#adding-in-https-protocol-support-to-the-certsrv-website)
* [Installation of CA Certificate Chain](#installation-of-ca-certificate-chain)

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

1. On the cert server, open IIS and select Server Certificates as shown
![image](../images/Pasted%20image%2020230817150808.png)  

2. On the certs page, cick "Create Domain Certificate..." on the right menu
![image](../images/Pasted%20image%2020230817150903.png)  

3. Fill out the cert details as shown, ensure that the Common Name is the Fully Qualified Domain Name (FQDN) as the certsrv  
![image](../images/Pasted%20image%2020230817151030.png)  

4. On the next page fill out the online cert authority as shown, cick on the select button to automatically select an online CA which should be the certsrv and enter an identifier within friendly name, click finish afterwards  

Refer to [this](https://learn.microsoft.com/en-us/answers/questions/229049/user-request-certificate-0x80092013-(-2146885613-c)) if needed for CRL troubleshooting  

![image](../images/Pasted%20image%2020230817151214.png)  
![image](../images/Pasted%20image%2020230817154517.png)  

5. Now navigate to the Default Web Site and click Bindings on the right menu  
![image](../images/Pasted%20image%2020230817155445.png)  

6. In Site Bindings, click Add, use https type and select the SSL cert with the friendly name used earlier in the Add Site Binding window and click OK in the Add Site Binding window to add the HTTPS site with the created SSL cert
![image](../images/Pasted%20image%2020230817155629.png)  

<br>

## Installation of CA Certificate Chain  

1. On the Win10 client using the domain admin account (use admin account privs on this system) search for internet options and open the internet options menu  
![image](../images/Pasted%20image%2020230817160414.png)  
![image](../images/Pasted%20image%2020230817160439.png)  

2. Navigate to the current Trusted Root Certification Repository via Content tab -> Certificates button -> Trusted Root Certification Authorities tab and find it within this tab  
![image](../images/Pasted%20image%2020230817160631.png)  
![image](../images/Pasted%20image%2020230817160759.png)  

3. The remaining steps are used to download and install a Root CA cert to a non-domain PC manually (all done on the win10 client)  

4. Open a web browser and access the CA server at the URL "https://<FQNA of Root CA server\>/certsrv" (warning is due to how domain cert is not up to date as modern browsers require to check the "DNS" entry)  
![image](../images/Pasted%20image%2020230817161139.png)  

5. Login to the interface using a domain user account (downloading a Root CA cert does not require special privilege, but you need to be the admin of the client system to install the downloaded Root CA cert on that system)  
![image](../images/Pasted%20image%2020230817161426.png)  
![image](../images/Pasted%20image%2020230817161458.png)  

6. Click on "Download a CA cert, cert chain or CRL" link  
![image](../images/Pasted%20image%2020230817161657.png)  

7. Check that Current\[<Your Root CA Record\>] is selected and click Download CA cert chain to save the cert to the Download directory  
![image](../images/Pasted%20image%2020230817161909.png)  

8. DO NOT OPEN THE DOWNLOADED FILE DIRECTORY, but go to the download folder and right-click the file and choose "Install Certificate" to install the cert chain to the win10 client, click next to start the cert import wizard  
![image](../images/Pasted%20image%2020230817162128.png)  
![image](../images/Pasted%20image%2020230817162159.png)  

9. At the certificate store page, select "Place all certificates in the following store" and choose "Trusted Root Certification Authorities"  
![image](../images/Pasted%20image%2020230817162547.png)  

10. Click OK, click Next and Click Finish to complete the operation  

11. Just verify that <Your Root CA\> is still in your cert repository  

12. Repeat the above steps for memsrv1 to verify that the memsrv1 has <Your Root CA\> installed with the cert chain in its "Trusted Root Certification Authorities" repository  

Note: [short demo video](https://web.microsoftstream.com/video/1aa26ac9-1099-4396-8e2f-501cac37751e) of how to download and install your own Enterprise Root CA cert in memsrv1  

<br>

## Requesting and Installing a Personal User Cert  

note: done to "enroll" a user cert for user1@<your domain\> from the Enterprise Root CA via the web interface  

1. On Win10 client, login as user1, use the Edge or internet explorer web browser and access the AD CS web server https://<Your AD CS Web server\>/certsrv. login as user1 when prompted  
![image](../images/Pasted%20image%2020230819153515.png)  

2. Click on Request a certificate link  
![image](../images/Pasted%20image%2020230819171331.png)  

3. Click on User Certificate  

--NOTE: USAGE OF THE WEB INTERFACE FOR CERTS REQUIRES KERBEROS, IT WON'T WORK WITHOUT KERBEROS--  
![image](../images/Pasted%20image%2020230819171122.png)  

4. Click submit to get a user cert  
![image](../images/Pasted%20image%2020230819171216.png)  

5. This error page will be shown as the "Autoenrollment" feature is turned on by default for this type of cert, thus, users do not need to manually download and install the signed certs  
![image](../images/Pasted%20image%2020230819171525.png)  

6. However, Microsoft automated autoenrollment by using a proprietary tech "ActiveX", whicch is only supported by MS IE browser, even Edge does not support ActiveX  

7. To overcome the issue, enable Internet Explorer mode in Edge via the settings URL used and the settings that are highlighted to access the ActiveX oriented page, URL is "edge://settings/defaultbrowser"  
![image](../images/Pasted%20image%2020230819171953.png)  

8. Next click "Add" beside the Internet Explorer mode pages section and add the cert server URL as a Internet Explorer page  
![image](../images/Pasted%20image%2020230819172126.png)  
![image](../images/Pasted%20image%2020230819172209.png)  

9. One more step is required to enable ActiveX content in IE mode, add https://<your smwca server\> as a trusted site for Internet Explorer. In Windows search box, type in "Internet Options", and open the legacy Internet Explorer Properties windows  
![images](../images/Pasted%20image%2020230819172516.png)  

10. Reopen the Edge browser, browse to https://<FQDN of cert server\>/certsrv again. After the page is loaded, click the ... and open the settings menu, click on the "Reload in Internet Explorer mode" button to reload the web page  
![image](../images/Pasted%20image%2020230819172850.png)  

11. Enter the user1 credentials when prompted. The browser will now remain in IE mode as shown by the IE icon beside the URL and within the tab. Repeat steps 2-4 to get the user cert  
![image](../images/Pasted%20image%2020230819173028.png)  
![image](../images/Pasted%20image%2020230819173144.png)  

12. After clicking on the "User Certificate" link, the ActiveX page should be working, showing a prompt. Click Yes to allow the page to download and install the user cert on your behalf  
![image](../images/Pasted%20image%2020230819173308.png)  
