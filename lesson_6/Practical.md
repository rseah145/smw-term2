# Navigation
* [Create an IPsec Policy](#create-an-ipsec-policy)
* [Configure Authentication, Encryption Methods and Protocol for IPsec](#configure-authentication-encryption-methods-and-protocol-for-ipsec)
* [Testing IPsec Policy](#testing-ipsec-policy)

<br>

## Create an IPsec Policy  

1. Log on to the DC as domain admin  

2. Within GPMC, right-click on GPOs and create a new GPO with the name "securedICMPPolicy"  
![image](../images/Pasted%20image%2020230815105132.png)  
![image](../images/Pasted%20image%2020230815105217.png)  

3. Edit the newly created GPO and navigate to the IPsec Policy setting menu as shown  
![image](../images/Pasted%20image%2020230815105408.png)  

4. Right-click on the "IP Security Policies on Active Directory" and click "Create IP Security Policy" to open the IP Security Policy wizard  
![image](../images/Pasted%20image%2020230815105605.png)  
![image](../images/Pasted%20image%2020230815105642.png)  

5. On the policy name page enter "ServerOneICMP"  
![image](../images/Pasted%20image%2020230815110829.png)  

6. On the requests for secure communication page, take the default and go to the confirmation page  

7. Ensure that the "edit properties" box is checked an click Finish  
![image](../images/Pasted%20image%2020230815111030.png)  

8. Close the properties window that pops-up after completing the IP security wizard  
![image](../images/Pasted%20image%2020230815111143.png)  

<br>

## Configure Authentication, Encryption Methods and Protocol for IPsec  

Modify Server1 ICMP IPsec Policy to enforce IPsec communication using pre-shared key authentication for ICMP traffic between 2 machines  

1. Edit the properties of securedOneICMPPolicy via GPO editor for securedICMPPolicy -> PC configuration -> Policies -> Windows Settings -> Security Settings -> IP Security Policies  
![image](../images/Pasted%20image%2020230815112610.png)  

2. Right-click on "ServerOneICMP" in the right window of GPME and choose "Properties"  
![image](../images/Pasted%20image%2020230815112721.png)  
![image](../images/Pasted%20image%2020230815112812.png)  

3. In the rules tab, ensure the default entry "\<Dynamic\>" is unchecked and click on the "Add" button to bring up the Security rule wizard  
![image](../images/Pasted%20image%2020230815113002.png)  

4. Use the following settings for each following page (Click next after setting each page)  
![image](../images/Pasted%20image%2020230815113213.png)  
![image](../images/Pasted%20image%2020230815113247.png)  

<br>

On the IP Filter List page, click add and add in a new IP Filter List with the name "ServerOneICMP Traffic"  
![image](../images/Pasted%20image%2020230815113415.png)  
![image](../images/Pasted%20image%2020230815113502.png)  

Within the IP Filter List pop-up menu click "Add" and set the following IP Filter Description and Mirrored property (check the mirrored option). Click Next  
![image](../images/Pasted%20image%2020230815113657.png)  

On IP Traffic Source page, select A specific IP Address or Subnet and enter the IP of memsrv1. Click Next  
![image](../images/Pasted%20image%2020230815113852.png)  

Set destination to any IP address. Click next  
![image](../images/Pasted%20image%2020230815113959.png)  

Set IP Protocol type to ICMP. Click Next  
![image](../images/Pasted%20image%2020230815114043.png)  

Complete the IP Filter setup  
![image](../images/Pasted%20image%2020230815114203.png)  

Close the ServerOneICMP Traffic Filter list and move on to the next step  
![image](../images/Pasted%20image%2020230815114309.png)  

Back in the security rule wizard, select "ServerOneICMP Traffic" filter list as the default IP Filter List by clicking on the radio button beside it and checking it. Click Next  
![image](../images/Pasted%20image%2020230815114531.png)  

Within the Filter Action page, click add and name the new filter action "ICMP security"  
![image](../images/Pasted%20image%2020230815114655.png)  
![image](../images/Pasted%20image%2020230815114736.png)  

Use the following options for the remaining setup pages for the new filter action  
![image](../images/Pasted%20image%2020230815114845.png)  
![image](../images/Pasted%20image%2020230815114916.png)  
![image](../images/Pasted%20image%2020230815115011.png)  
![image](../images/Pasted%20image%2020230815115053.png)  

Select the newly created filter action and click next  
![image](../images/Pasted%20image%2020230815115159.png)  

For authentication method, select pre-shared key and set the string value to "abc123def". Click next and complete the security rule setup  
![image](../images/Pasted%20image%2020230815115329.png)  

Final results  
![image](../images/Pasted%20image%2020230815115435.png)  

<br>

## Testing IPsec Policy  

1. Do the following ping tests before deploying the IPsec Policy, ensure that all firewalls allow ICMP traffic first (Use GPs to adjust VMs firewall settings. Use Local Policy to adjust host notebook firewall settings, if the host notebook is also used in the ping tests).  
Ping memsrv1 from Win10 client  
Ping Win10 client from memsrv1  
Ping memsrv1 from DC  
Ping DC from memsrv1  
Ping Win10 client from DC  
Ping DC from Win10 client  

<br>

All Pings should be successful  

2. Assign the ServerOneICMP policy   
Use GPMC and edit the securedICMPPolicy under GPOs  
In GPME, right-click "ServerOneICMP" under IP Security Policies (PC Config -> Windows Settings -> Security Settings -> IP security Policies) and choose "Assign"  
![image](../images/Pasted%20image%2020230815121528.png)  

3. Close GPME, and link the securedICMPPoliccy GPO to the OU that hosts memsrv1  
Right-click on the OU and click "Link an Existing GPO"  

