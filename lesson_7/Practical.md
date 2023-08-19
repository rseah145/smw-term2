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
![image](../images/Pasted%20image%2020230819154012.png)  

3. Click on User Certificate  

--NOTE COPIED EVERYTHING FROM THE PRACTICAL FROM HERE ON AS IT DOES NOT WORK FOR MY VMS--  

1.       Click on **User Certificate**.
![image](../images/Pasted%20image%2020230819170753.png)  

2.       Click Submit in the following page to get a User Certificate .

Most probably, you will see the following 'Error Page.'

For this type of certificate, the 'Autoenrollment' feature is turned on by default, thus, the users do not need to manually download and install the signed certificates.

However, Microsoft automated the above by using a propitiatory technology, ActiveX, which is only supported by Microsoft Explorer browser. Even Edge does not support ActiveX!
![image](../images/Pasted%20image%2020230819170817.png)    

3.       To overcome the issue, we need to enable the Internet Explorer mode in the Edge browser when access this ActiveX oriented page: 

                    i.          In the address bar for Microsoft Edge, type edge://settings/defaultbrowser and then click Enter.  
 ![image](../images/Pasted%20image%2020230819170903.png)    

                  ii.          Configure the default browser to have the above configurations. Then press the Add button to add in your cert server URL as a Internet Explorer mode page.

                 iii.         Confirm the FQDN is added to the settings:

                 iv.         One more step is required to enable the activeX content in the IE mode: You need to add https://<your smwca server> as a trusted site for the Internet Explorer.

At windows search box, type in 'Internet Options', and open the legacy Internet Explorer Properties windows:

At the Security Tab, add in the scwca site url to the Trusted Sites list.

                   v.         Now you can open https://<FQDN of your cert server>/cert again. After the page is loaded, click the ... and open the settings menu. You can click on the Reload in Internet Explorer mode to reload the web page.

                 vi.         The browser now will remain in Internet Explorer mode and you can proceed again to request of a user certificate.

                vii.         The ActiveX page should be working now and you will be prompted with the following popup. Click the Yes button to allow the page to download and install the user cert for you.

              viii.          You may submit your request now.

4.       If you request is accepted you will see the following page:

5.       Click Install this certificate. Wait a while for the message “Your new certificate has been successfully installed”.

6.       You can examine your certificate repository. You will see a new 'Personal' certificate (corresponding to user1) is added into your repository:

(The above is only a sample screen shot for your reference; the detail information will be different from your own system.)

7.       So far, you have installed two certificates from your own Root CA. One is the Trusted CA Root Certificate (at the pervious exercise) installed for the computer, the other is the User Certificate installed for user1 account in this computer.

**Lab Exercise 7-4**: **Adding in Certificate Template to the Issuing CA***

_*(In our lab setup we only use one single CA server, thus it is our root CA and it is also our Issuing CA.)_

|   |
|---|
|Set up as an Issuing CA to issue IPSec certificates to other systems|

|   |
|---|
|Windows 2019 Server<br><br>(Root CA)|

 

  

1.       Logon to your Root CA Server with the Domain Admin Account.

2.       Go to Start, then in the ‘_search_’ box type **MMC.** Press **Enter.**

3.       When the MMC opens, choose **File, Add/Remove Snap-in.**

4.       Locate and add in ‘Certificate Templates’. Close the Add/Remove Snap-in screen.

5.       Click OK to add in the snap-in. Double-click Certificate Templates on the left side of the screen. Your screen should display all the available templates along with additional parameters, as shown in the figure below.

6.       Highlight the ‘IPSec’. Right-click the template and choose Duplicate Template.

When being prompted for the Compatibility options, choose appropriate settings for the Certificate Authority and the Certificate Recipient.

For the current ST2612 Lab setup, the we should select Server 2012R2 for both settings.

7.       On the **General Tab**, In the Template display name text box, type ‘SMW IPSec', in the Template name, type 'SMW IPSec Template'.

8.       On the Issuance Requirement tab, ensure the **CA certificate manager approval** box is **NOT** checked. (This is to ensure the auto-enrolment.)

9.       On the Request Handling Tab, check **Allow private key to be exported** checkbox. (This is to ensure the requester can get the private key from the Issuing Server.)

10.    Click **Apply**, then click **OK** to confirm the Duplicate Template operation.

11.    Close the MMC window. You do not need to save the MMC settings.

12.    The above steps are to create a new Certificate Template. To verify the above operations are completed correctly and to actually 'enable' the new template in your Root CA, do the followings:

a.        At your Root CA server, at the server manager, under Tools menu, select Certification Authority. (Should be the first or the second choice in the Tools menu.)

b.       Expand your Root CA node at the left pane.

c.        At the left pane, click on the Certificate Templates to view all the current enabled templates at the main pane. Each template refers to a type of certificates the CA can issue.   User, Computer, and Web Server are the types that have been relevant to our Lab exercises.

d.       Right click on the Certification Template and choose New à Certificate Template to Issue

e.        At the next screen, select SMW IPSec template (The one you have created it previously), and click OK to enable it. Now, your certificate service will allow users/machines to enroll for an IPSec specific certificate based on this newly enabled template. You will use this template in the next Activity.

**Lab Exercise 7-5: Requesting and Installation of a Machine Certificate for IPSec Authentication**

In the previous practical, we had set up an IPsec policy "ServerOneICMP" that requires all ping packets between affected systems to be encrypted using a Pre-Shared Key. We will now change it to use Certificates instead of Pre-Shared Keys.

|   |
|---|
|Server1|

|   |
|---|
|Windows 2016<br><br>(Issuing CA)|

|   |
|---|
|Issues IPsec certificates|

|   |
|---|
|Windows 10|

 

  

1.       Logon to your Domain Controller with a Domain Admin account. Go to Group Policy Management.

2.       Modify your IPSec policy – serverOneICMP   (created in the previous practical) to only use Certificate Authentication scheme. First you have to add the certificates-based authentication. and you have to choose the <Your Root CA> certificate for the Authentication (See following diagram).

The above configuration implies the hosts must own certificates that issued by kitty-SMWCA-CA.

3.       Now you can remove the Preshared Key Authentication method:

4.       To make the experiment with more excitement, modify the IP Filter List to use All ICMP Traffic. (see following diagram)

  

5.       Apply the securedICMPPolicy to the Windows 10 and Server1 images. Verify your setting by issue ping commands between these two machines. The ping should not work as both the Windows 10 and Server1 do not have the required certificates yet.

6.       Try to ping your Server1 and Windows 10 from your PDC, can they work? Why ?

7.       Logon to the Win 10 image as the Domain Admin. (or any Domain account that has Administrator rights on the local computer).

8.       Bring up a mmc session and add in the Certificates Snap-in. Choose ‘Computer account’ when you are prompted for the certificates type (see following diagram):

9.       On the next page, choose “The snap-in will always manage the local computer” and bring up the mmc window.

10.    Expand the Certificates (Local Computer) node, highlight and double click the ‘personal’ certificate container.

11.    Right-click on Personal, All Tasks, Request New Certificate.

12.    Click Next to proceed.

13.    For Select Certificate Enrollment Policy, click Next.

14.    For Request Certificates, check "SMW IPSec", Click Enroll.

15.      At the next page you may view the certificate and then click the finish button.

16.    Repeat the steps for the Server1 image to obtain an IPSec Certificate.

17.    Now try to ping between these two machines. The ping should now work as both Windows 10 and Server1 images have the IPSec certificates. Can any one of them now able to ping your PDC? Why?

**Reset policy settings:**

18.    On the Domain Controller, in Group Policy Management, un-assign IPsec policy from the GPOs that associate with the Windows 10 image and the Server1 image.

19.    That's all.

In the following part of the practical (Ex 7-6 and Ex 7-7), you will set up the IIS on Server1 to support SSL that requires **mutual** authentications.

|   |
|---|
|Request for SSL certificate|

|   |
|---|
|Web pages transmitted securely using HTTPS protocol.|

 

  

|   |
|---|
|Win 10|

|   |
|---|
|Server1|

|   |
|---|
|Root CA|

**Lab Exercise 7-6: Preparation only. Adding in HTTPS protocol support to your server1 web site.**

**(Second Attempt to enroll an SSL certificate.)**

  Verify if Server1 has IIS installed and offering http service (but not https yet).

1.     Logon to Win 10 with any account, browse to https://<FQDN of your Server1>

To verify that https is not available at the server1 web page yet.

2.     Browse to http://<FQDN of your Server1>/webdoc

webdoc should be there if you have completed lab 4 correctly.

                **Now is the** The approach we have used in Exercise 7-1-5 is based on a build-in wizard of IIS Manager to enroll an SSL certificate for the web site. It is easy but there is a better (but a bit more complicated) way to get a 'better' SSL certificate.  

3.     Our new approach is to use the MMC (with certificates snap-in) to enroll an SSL certificate for the Web server (our Server1).

However, when you try to request for a new certificate (machine cert for the purpose of supporting SSL / Web Hosting), you do not see such option exist:

                    i.          Make sure at least that your PDC, RootCA server (smwca) and server1 are up and running.

                  ii.          On server1, login as a user in the Domain Admin security group. (e.g. smw_srv2016.)

                 iii.         Start a MMC, add the certificates snap-in. (for local computer).

                 iv.         Access to personal node, try to enroll for a new certificate:

There are only two choices available, but none of them fit our needs.

                   v.          Click on the Show all template checkbox to investigate.

As scroll down to reach to the bottom of the list, you will see the 'Web Server' template and it is the one we need. However, it is unavailable because some sort of permissions issue.

Let us fix this permission issue at the ADCS server.

                 vi.         On smwca, login as an Enterprise Admin group user (e.g. smw_srv2016), bring up the ADCS management console (Certification Authority), right click on the 'Certificate Templates' node at the left pane and click on the 'Manage' menu.

 

                vii.         At the Certificate Templates Console, locate the **Web Server** entry from the list of all the Certificate Templates and access to its 'Properties' menu. (To check/update its security setting.)

              viii.         At the Web Server Properties menu, select the 'Security' Tab. You will see the current security settings for the Web Server Template. In order to enable a Web server to enroll to a Web Server certificate, we should allow 'Domain Computers' to have the Allow to 'Enroll' permission. Since this setting is missing. (or secured by default.) We need to add it in. To do that, Click at the Add button.

                 ix.         You should update the security permission settings as shown below. Thats', add in the entry of Domain Computers to have 'Read' and 'Enroll' allow permissions. Press Apply and OK buttons to save the settings. You may then close all the consoles at the smwca side, and return to the server1 to try to make a request for an SSL certificate again.  

                   x.         Repeat Step ii to step iv. This time the list of the available certificates should include Web Server. As you can see, to make request for a Web Server certificate, you need to provide more information, click at the link shown and bring up the next menu to fill out the required information.

                 xi.         At the General Tab, fill out meaningful Friendly name and Description regarding this new certificate you are going to enroll.

                xii.         At the Subject Tab, (It is highlighted with the         sign) as this is the Tab that requires most of the information.

As shown below, there are two main categories of information to be filled out, one is under the 'Subject name', the other is the 'Alternative name'. Take note that each of these categories accept multiple entries of input and each of the input entries is in <Type>=<Value> format.

The <Type> part refers to the available choices in the corresponding dropdown list.  For example, the first choice in the Subject name is 'Full DN'.

              xiii.         Add in the Subject name Type/Value entries:

The actual Type/Value entries for the above:

Common name/server1.kitty.org

Organization/ST2612

Organization Unit/Lab7

Locality/sg

State/sg

Country/SG

(Note: The most important Type/Value entry of the above is the common name, you have to set it to the FQDN of your server1. In this example, the FQDN is server1.kitty.org)

              xiv.         Add in one Type/Value entry to the Alternative name:

The actual Type/Value entries for the above:

DNS/server1.kitty.org

(Note: The modern-day browsers (since 2017) require the SSL certificate to contain an Alternative name entry (DNS) to hold the FQDN of the server. Without this DNS entry in the certificate, most of the browsers will flag it out as an invalid certificate warning! In this example, the FQDN is server1.kitty.org)

                xv.         Click Apply and OK button to commit the changes and return to the previous menu. Now you can proceed to select Web Server and make the certificate request by pressing the Enroll button.

              xvi.         When you see the request result page, click Finish to close the menu. Close the MMC. That's all.

4.     You can now proceed to 'bind' the newly created SSL cert to your Server1 web server. On Server1 open the IIS management console (IIS Manager):

Expand the Server1 node and select the Default Web Site at the left pane. Click on the 'Bindings...' action at the right pane.

5.     At the Site Bindings menu, you may add in the https binding. You need to select the SSL certificate that you have just enrolled from the RootCA. You can select it by the 'Friendly name' and your use the View button to examine the content of the certificate. You may check if it contains an alternative name.

Click OK to commit the binding. You may close the IIS manager too.

6.     To check. On Win 10, repeat step 1 of this exercise (EX 7-6). Try to surf to https//<the FQDN if your server1>/webdoc :

Now, you should have achieved two things:

The webpage is accessible via https.

The browser accepts the SSL certificate without issuing out any warning message.

Reflection prompt: After the completion of this extremely long Exercise 7-6, can you recount, how new ideas, tools, concepts, and/or knowhow you have gained in this exercise?

**Lab Exercise 7-7: Create a virtual directory on IIS that uses SSL**

1.       At your Server1, create a new directory C:\sslDemoWeb

2.       Click Start, Administrative Tools, Internet Information Service (IIS) Manager

3.       Expand your server, expand Sites, right click on Default Web Site and create a new Virtual Directory, with the alias name of ‘MutualSSL’ and the physical path ‘C:\sslDemoWeb’ (see following screenshot):

4.       Create the following html page with the name ‘default.htm’ in C:\sslDemoWeb. (Make sure you create “**default.htm**” and not “default.htm.txt”)

<html>

<body>

Yes, you can only see me when you have properly installed the SSL client certificate.

</body>

</html>

5.       Test and verify the above your setup by accessing the web page from your Win 10 image via http://_<FQDN of your Server1>_/MutualSSL and https://_<FQDN of your Server1>_/MutualSSL. At this point, the page is not restricted for https access only, you can use either the http or the https protocol to access the MutualSSL website without any problem.

|   |
|---|
|http|

 

  

|   |
|---|
|https|

Note: In case you have seen any warning / error messages when accessing to the page via https, you should alert your tutor to help you to troubleshoot it.

**Lab Exercise 7-8: Restricted for SSL Access**

1.       The next step is to enforce the MutualSSL site to only allow HTTPS access. Go to the console tree again, click the MutualSSL node, and then in the center pane, double-click on SSL Settings.

2.       At the SSL Settings pane, check the Require SSL box. Then click the ‘Apply’ link at the Actions pane

3.       Access to the MutualSSL site via **HTTP** protocol from your client image should be denied now. (You may need to refresh the page in your web browser)

4.       Try to access to the MutualSSL site via **HTTPS** protocol again. It shall remaining working.

**Lab Exercise 7-9: Configure SSL security with “Require Client Certificate” access**

1.       To tighten the security of the SSL Web Directory further, we can enable the ‘Require Client Certificates’ option. This means clients need to have a valid certificate too.

2.       After applying the change, try to access to https://<The FQDN of _yourServer1>_/MutualSSL from your Win 10 image (You may need to refresh the page in your web browser).

You may see a response similar to the screen shot shown at below. If you do not see this error, try logging in to your Win 10 as a different user. You will get this error if your current login user account does not have a User Certificate issued by the RootCA (smwca) stored in the Win 10.

You also need to disable the Anonymous Authentication method for the MutualSSL directory in order the let apply the client certificates authentication.

|   |
|---|
|click on the Authentication icon.|

|   |
|---|
|Only enable Windows Authentication and disable all the rest.|

3.       Apply what you have from the previous exercise, login as mrg1 (Domain user) to your Win 10, enrol a User certificate for mrg1. Try to access to https://<FQDN of your server1>/MutualSSL , you should be able to do so. 

(Hint: User MMC (Certificates Snap-in) is an easier approach to enroll a user certificate.)