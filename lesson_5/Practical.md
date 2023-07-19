# Done using the provided WSUS VM (Note that a manual setup will take a lot more time)  
# Basically Set up GPO with the following configuration as shown under the path to enable WSUS updates  
<br>

## Ensure WSUS server (Server2) is in memberserverOU  

![image](../images/Pasted%20image%2020230719133204.png)

<br>

## Computer Configuration -> Policies -> Administrative Templates -> Windows Components -> Windows Update  

![image](../images/Pasted%20image%2020230719132024.png)  

![image](../images/Pasted%20image%2020230719132229.png)  

<br>

## Configure Automatic Updates policy with the following options set  

![image](../images/Pasted%20image%2020230719132433.png)  

<br>

## Configure "Specify Intranet Microsoft Update Service Location" policy as such under the same policy path to configure automatic updates policy  

![image](Pasted%20image%2020230719134249.png)
