# Done using the provided WSUS VM (Note that a manual setup will take a lot more time)  
# Basically Set up GPO with the following configuration as shown under the path to enable WSUS updates  
<br>

## Ensure Win server 2012 R2 (Server1) is in memberserverOU  

![image](../images/Pasted%20image%2020230719135806.png)  

<br>

## Computer Configuration -> Policies -> Administrative Templates -> Windows Components -> Windows Update  

![image](../images/Pasted%20image%2020230719132024.png)  

![image](../images/Pasted%20image%2020230719132229.png)  

<br>

## Configure Automatic Updates policy with the following options set  

![image](../images/Pasted%20image%2020230719132433.png)  

<br>

## Configure "Specify Intranet Microsoft Update Service Location" policy as such under the same policy path to configure automatic updates policy  
## Port is 8530 NOT 8350

![image](../images/Pasted%20image%2020230719134404.png)  

![image](../images/Pasted%20image%2020230719153003.png)  

<br>

Run "gpupdate /force" with administrator privileges to have the changes take effect  

<br>

"gpresult /r /v" with the admin privilege shell should now show the GPO taking effect on the Win server 2012 R2 (Server1)  

![image](../images/Pasted%20image%2020230719135541.png)  

<br>

## Verification and there not being any updates on Win Server 2012 R2 (memSrv 1)

![image](../images/Pasted%20image%2020230719153559.png)  

<br>

## Verifying that Win Server 2012 R2 (memsrv1) appears in WSUS on Server2 (WSUS server)
## Server Dashboard -> Tools -> Windows Server Update Services -> \[WSUS-Server2\] -> PCs -> All PCs -> Unassigned PCs (Change status to "Any" and click on refresh button)

![image](../images/Pasted%20image%2020230719153945.png)  

<br>

## Setting up a Test PC Group to test updates from WSUS (on WSUS Server)  
## WSUS-Server2  

![image](../images/Pasted%20image%2020230719182832.png)  

![image](../images/Pasted%20image%2020230719183000.png)  

![image](../images/Pasted%20image%2020230719183110.png)  

![image](../images/Pasted%20image%2020230719183320.png)  

![image](../images/Pasted%20image%2020230719183412.png)  

<br>

## Approve and Deploy updates to the Test Group  
## Any 2 updates for practical  

![image](../images/Pasted%20image%2020230719183741.png)

<br>

### Click on the button for a PC group and approve install  

![image](../images/Pasted%20image%2020230719183841.png)  

![image](../images/Pasted%20image%2020230719183926.png)