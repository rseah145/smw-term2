# Navigation
* [Create an IPsec Policy](#create-an-ipsec-policy)


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
