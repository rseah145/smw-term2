# Note: There's no practical for lesson 8  

<br>

# Objectives  

Understand Win Server 2016 remote access services  

Implement and manage a virtual private network  

Configure VPN server  

Troubleshoot virtual private network installations  

Install and configure Remote Desktop Services  

<br>

# Navigation  
* [Remote Access Services](#remote-access-services)
* [Implementing a Virtual Private Network](#implementing-a-virtual-private-network)

<br>

## Remote Access Services  

### Intro to Remote Access  

Remote Access Services (RAS) role  
* Enables remote access through 3 means: Virtual Private Networking, Direct Access, and Web Application Proxy  

<br>

Virtual Private Network (VPN)  
* Like a tunnel through a larger network that is restricted to designated member clients only (e.g. Cisco Anyconnect VPN and Palo Alto Global protect VPN)  

<br>

Related Topics (Textbook)  
* Direct Access, establishes 2 "tunnels" for connecting to a Direct Access erver used in remote access. Transparent to users and always on  
* Web Application Proxy, publishing apps so that external users to an org can access those apps on the org's servers  

<br>

## Implementing a Virtual Private Network  

VPNs can use an Internet connection or an internal network connection as a transport medium to establish a connection with a VPN server  

A VPN uses LAN protocols as well as tunneling protocols to encapsulate the data as it as sent across a public network such as the Internet  

Benefit of using a VPN is users can connect to a local ISP and connect through the ISP to the local network  

VPN is used to ensure that any data sent across a public network, such as the Internet, is secure. VPN creates an encrypted tunnel between the client and the RAS server  

<br>

Typical VPN use cases  
* Ensure secured communication between 2 sites
* Ensure secured communication between a site with remote clients to allow remote clients to access resources offered by the site
* Secured Internet connection to a remote client, which is a remote client establishing a VPN connection to a site, then using the gateway of the site to access the Internet. Security between the remote client and the Internet is taken care of by security measures within the site  

<br>

Diagram with 3 remote client to site VPN implementations  
1. Top: Firewall based VPN for remote client access to office network  
2. Middle: Router based VPN for remote client access to office network  
3. Bottom: Microsoft RAS based VPN (basic router) for remote client access to office network  
![image](../images/Pasted%20image%2020230820162616.png)  

<br>

### More theory  

A VPN server uses 2 or more NICs for communication  
* 1 NIC used to connect to the private network inside the org that uses private IP addressing  
* The other NIC connects to the external public network  

<br>

To create this tunnel, the client first connects to the Internet by establishing a connection using a remote access protocol  

Once connected to the Internet, the client establishes a 2nd connection with the VPN server  

The client and the VPN server agree on how the data will be encapsulated and encrypted across the virtual tunnel  

Caveats:
* Do not use any of your DC to be the VPN server  
* VPN server is public facing and it should be situated in the DMZ segment  

<br>

Typical VPN configuration uses a Win Server with 2 NICs 
