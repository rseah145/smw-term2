# Navigation
* [Create an IPsec Policy](#create-an-ipsec-policy)
* [Configure Authentication, Encryption Methods and Protocol for IPsec](#configure-authentication-encryption-methods-and-protocol-for-ipsec)

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
