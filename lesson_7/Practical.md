# Navigation
* [Add appropriate server role to Server 2016 to facilitate an Enterprise Root CA](#add-appropriate-server-role-to-server-2016-to-facilitate-an-enterprise-root-ca)
* [Setting up the default Enterprise autoenrollment policy](#setting-up-the-default-enterprise-autoenrollment-policy)
* [Adding in HTTPS protocol support to the certsrv website](#adding-in-https-protocol-support-to-the-certsrv-website)
* [Installation of CA Certificate Chain](#installation-of-ca-certificate-chain)
* [Add Certificate Template to the Issuing CA](#add-certificate-template-to-the-issuing-ca)
* [Requesting and Installing a Machine Cert for IPSec Authentication](#requesting-and-installing-a-machine-cert-for-ipsec-authentication)
* [Prep only. Add HTTPS protocol support for server1 website (2nd attempt to enroll an SSL cert)](#prep-only-add-https-protocol-support-for-server1-website-2nd-attempt-to-enroll-an-ssl-cert)

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

13. Click Submit when the page loads after prompt  
![image](../images/Pasted%20image%2020230819173403.png)  

14. When prompted again via a pop-up, click Yes  
![image](../images/Pasted%20image%2020230819173450.png)  

15. Within the next page that is loaded after clicking the submit button (means the request is accepted), click on the Install this certificate link  
![images](../images/Pasted%20image%2020230819173611.png)  

16. Wait for a while, once the message "Your new certificate has been successfully installed" is shown, the user cert has been installed  
![image](../images/Pasted%20image%2020230819173725.png)  

17. Examine the cert repository as shown, a new "Personal" cert (corresponding to user1) has been added into the repo. Double-click on the newly added cert within the Certificates window to inspect the cert's details  
![images](../images/Pasted%20image%2020230819173955.png)  
![image](../images/Pasted%20image%2020230819174049.png)  
![image](../images/Pasted%20image%2020230819174105.png)  

18. So far, 2 certs have been installed from the Root CA, 1 is the Trusted CA Root cert installed for the PC, and the other is the User Cert installed for the User1 account on the same PC

<br>

## Add Certificate Template to the Issuing CA  

1. Logon to the Root CA Server with a Domain Admin account  

2. Go to start, and type "MMC" in the "search" box  

3. When MMC is open, choose "File" -> "Add/Remove Snap-in"  

4. Locate and add in "Certificate Templates", close the Add/Remove Snap-in screen  
![image](../images/Pasted%20image%2020230819183023.png)  

5. Click OK to add the snap-in. Double-click Certificate Templates on the left side of the screen. The screen should display all the available templates along with additional paramters, as shown  
![image](../images/Pasted%20image%2020230819183213.png)  

6. Highlight "IPsec", right-click the template and choose Duplicate Template. When prompted for compatibility options, choose the appropriate settings for the Cert Authority and the Cert Recipient. For the lab setup, Server 2012R2 is used for both settings as shown  
![image](../images/Pasted%20image%2020230819183422.png)  

7. In the General tab, in the template display name text box, type "SMW IPSec", in the template name, type "SMW IPSec Template"  
![image](../images/Pasted%20image%2020230819183537.png)  

8. In the Issuance Requirement tab, ensure "CA certificate manager approval" box is NOT checked (ensure auto-enrolment)  
![image](../images/Pasted%20image%2020230819183632.png)  

9. In the Request Handling tab, check "Allow private key to be exported" checkbox (ensure requester can get the private key from the issuing server)  
![image](../images/Pasted%20image%2020230819183747.png)  

10. Click Apply, then click OK to confirm the Duplicate Template operation  

11. Close the MMC window, there's no need to save the MMC settings  

12. The steps taken are to create a new Cert Template. To verify the above operations are completed correctly and to actually "enable" the new template in the Root CA, do the following:  
At the Root CA server, at the Server Manager, under Tools menu, select Certification Authority (first/second choice in Tools menu)  
Expand the Root CA node on the left pane  
At the left pane, click on Certificate Template to view all current enabled templates at the main pane. Each template refers to a type of certificates the CA can issue. User, Computer, and Web Server are the types that have been relevant to the Lab exercises  
![image](../images/Pasted%20image%2020230819184200.png)  

Right-click on the Certification Template and choose New -> Certificate Template to Issue  
![image](../images/Pasted%20image%2020230819184248.png)  

At the next screen, select SMW IPSec template (the one created earlier), and click OK to enable it. Now, the cert service will allow users/machines to enroll for an IPSec specific cert based on this newly enabled template. This template will be used later on  
![image](../images/Pasted%20image%2020230819184416.png)  

<br>

## Requesting and Installing a Machine Cert for IPSec Authentication  

Note: From practical 6, an IPsec policy "ServerOneICMP" was setup that requires all ping packets between affected systems to be encrypted using a Pre-Shared Key. That will not be changed to use Certs instead of Pre-Shared Keys  

1. Logon to the DC with a Domain Admin account. Go to Group Policy Management  

2. Modify the IPSec policy - serverOneICMP to only use Certificate Authentication scheme. First add the certificates-based authentication, and you have to choose the <Your Root CA\> certificate for the Authentication as shown (Select your Root CA cert by clicking on "More Choices" in the Windows Security window and scroll till you find it)  
![image](../images/Pasted%20image%2020230819185229.png)  

3. Remove the Pre-Shared Key authentication method  
![image](../images/Pasted%20image%2020230819185430.png)  

4. Modify IP Filter List to use All ICMP Traffic  
![image](../images/Pasted%20image%2020230819185557.png)  

5. Apply the securedICMPPolicy to the Win10 and Server1 machines. Verify the settings by issuing ping commands between the 2 machines. It should not work as both the Win10 and Server1 machines do not have the required certs yet  

6. Pinging Server1 works from the PDC, but Pinging Win10 client does not work  

7. Logon to the Win10 client as domain admin account  

8. Bring up an MMC session and add-in the Certificates snap-in. Choose "Computer account" when you are prompted for the certificates type as shown  
![image](../images/Pasted%20image%2020230819191253.png)  

9. On the next page, choose "The snap-in will always manage the local computer" and bring up the mmc window  
![image](../images/Pasted%20image%2020230819191338.png)  

10. Expand the Certificates (Local Computer) node, highlight and double-click the "Personal" certificcate container  
![image](../images/Pasted%20image%2020230819191442.png)  

11. Right-click on Personal, All Tasks, Request New Certificate  
![image](../images/Pasted%20image%2020230819191536.png)  
![image](../images/Pasted%20image%2020230819191605.png)  

12. Click Next  

13. For Select Certificate Enrollment Policy, click Next  
![image](../images/Pasted%20image%2020230819191701.png)  

14. For Request Certificates, check "SMW IPSec", Click Enroll  
![images](../images/Pasted%20image%2020230819191828.png)  

15. At the next page view the cert and then click the finish button  
![image](../images/Pasted%20image%2020230819191919.png)  

16. Repeat the steps on Server1 to obtain an IPSec Cert  

17. Ping between the 2 machines (Server1 and Win10 client), the ping should now work as both the Win10 client and Server1 machines have the IPSec certs. Test pings with the PDC as well (Win10 client/Server1 to PDC).  

<br>

### Reset policy settings  

18. On the DC, in GPM, un-assign IPsec policy from  the GPOs that associate with the Win10 client and the Server1 machines. The end.  

<br>

## Prep only. Add HTTPS protocol support for server1 website (2nd attempt to enroll an SSL cert)  

1. Logon to Win10 client with any account, browse to https://<FQDN of server1\> to verify that HTTPS has not been set up yet  

2. Browse to http://<FQDN of server1\>/webdoc, webdoc should be there if lab 4 was done properly  
![image](../images/Pasted%20image%2020230819201737.png)  
The method used earlier (first HTTPS SSL cert attempt) is based on a built-in wizard of IIS manager to enroll an SSL cert for the website. This 2nd method provides an SSL cert for HTTPS without the warning page  

3. Ensure at least PDC, Root CA server (smwca) and server1 are up and running  

<br>

4. On smwca, login as a user in the Enterprise Admin group (e.g. smw_srv2016), bring up AD CS management console (Certification Authority), right-click on "Certificate Templates" node on the left pane and click on the "Manage" menu  
![image](../images/Pasted%20image%2020230819210551.png)  

5. Within the Certifcate Templates console, locate the "Web Server" entry in the middle pane, right-click on it and access the properties menu  
![image](../images/Pasted%20image%2020230819210703.png)  

6. Navigate to the security tab, add "Domain Computers" and give it the allow "Enroll" permission as shown (click Apply and OK to save the settings)  
![image](../images/Pasted%20image%2020230819210830.png)  

7. Logon to server1 with a domain admin account, open MMC and add the cert snap-in (This PC + For local PC options)  
![image](../images/Pasted%20image%2020230819211217.png)  

8. Access the personal node under the certificates node, right-click on it and request a new cert  
![image](../images/Pasted%20image%2020230819211346.png)  

9. At request certs, select the web server cert that is now accessible due to the changes made at steps 4-6  
![image](../images/Pasted%20image%2020230819211450.png)  

10. Click on the link below web server and fill out the info for the general tab as shown  
![image](../images/Pasted%20image%2020230819211603.png)  

11. Navigate to the subject tab, fill out the subject names by selecting a type from the type dropdown box, entering a value, followed by clicking on "Add >". Fill out the Subject names as shown. The most important type/value entry is common name and it must be set to the FQDN of server1, as shown (Locality has been set to "sg", cut-off)  
![image](../images/Pasted%20image%2020230819211934.png)  

12. Set the alternative name section to the values shown  
![image](../images/Pasted%20image%2020230819212046.png)  

13. Click Apply and OK to save the additional cert info  
(Note:	The modern-day browsers (since 2017) require the SSL certificate to contain an Alternative name entry (DNS) to hold the FQDN of the server. Without this DNS entry in the certificate, most of the browsers will flag it out as an invalid certificate warning!)  
![image](../images/Pasted%20image%2020230819212138.png)  

14. Now select Web Server and make the cert request by clicking the Enroll button  
![image](../images/Pasted%20image%2020230819212317.png)  

15. When the result page is shown, click Finish to close the menu. Close MMC as well  
![image](../images/Pasted%20image%2020230819212358.png)  

16. Proceed to "bind" the newly created SSL cert to the server1 website. On Server1 open the IIS management console (IIS Manager)  
