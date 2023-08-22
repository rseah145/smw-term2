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
* [Intro to Remote Access Services](#intro-to-remote-access-services)
* [Implementing a Virtual Private Network](#implementing-a-virtual-private-network)
* [Using Remote Access Protocols](#using-remote-access-protocols)
* [Configuring a VPN Server](#configuring-a-vpn-server)
* [Troubleshooting VPN Installations](#troubleshooting-vpn-installations)
* [Connecting Through Remote Desktop Services](#connecting-through-remote-desktop-services)
* [Installing Remote Desktop Services](#installing-remote-desktop-services)
* [Configuring Remote Desktop Services](#configuring-remote-desktop-services)

<br>

## Intro to Remote Access  

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

<br>

Caveats:
* Do not use any of your DC to be the VPN server  
* VPN server is public facing and it should be situated in the DMZ segment  

<br>

Typical Win Server configuration to be used as a VPN server uses 2 NICs, AKA multi-homed dual NIC deployment  

VPN server may or may not be a part of a domain, if the Win Server is part of the domain, it can directly use the AD for client authentication  

<br>

## Using Remote Access Protocols  

Remote access protocol carries the network packets over a Wide Area Network (WAN) link  
* Encapsulates a packet, such as an IP datagram, so that it can be transmitted from a point at 1 end of a WAN to another point  

<br>

IP is the most commonly used transport protocol  

Several rermote access protocols are used by Win Server 2016 and its remote clients  

An early basic remote access protocol in use is PPP  

Different in LAN and WAN is in terms of the medium, hardware devices, bandwidth and topologies. The 2 types of networks usually runs on a different set of network protocols  

Remote Access Protocol needs to cater for LAN to WAN protocol conversion, and then WAN to LAN protocol conversion in order to allow 2 remote parties to communicate seamlessly as if both were on the same LAN  

<br>

Point-to-Point Protocol (PPP)  
* Used in legacy remote communications involving modems  
* Enables authentication of connections and encryptions for the network communications, but it is not considered to be as secure as modern options
* Can auto negotiate communications with several network communication layers at once  

<br>

When implementing a Win Server 2016 VPN server, there's 3 or 4 choices for remote access tunneling protocols:
* Point-to-Point Tunneling Protocol  
* Layer 2 Tunneling Protocol  
* Secure Socket Tunneling Protocol  
* <b>IKE v2 Protocol (IPSec, tunnel mode, IKE version 2)</b>  

<br>

IKEv2 is a newer version of IPsec which provides more configuration options  

<br>

Point-to-Point Tunneling Protocol (PPTP)  
* Offers PPP-based authentication techniques  
* Encrypts data carried by PPTP through using Microsoft Point-to-Point Encryption (MPPE)  

<br>

Layer Two Tunneling Protocol (L2TP)  
* Works similarly to PPTP  
* Uses Layer Two Forwarding that enables forwarding on the basis of MAC addressing  
* Uses IP Security (IPsec) for additional authentication and for data encryption, IPsec is a set of IP-based secure communications and encryption standards created through IETF  

<br>

PPTP is the easiest protocol to setup, most network devices support it, however, it is not a good for use in a VPN channel that requires long (or near permanent) sessions  

<br>

Secure Socket Tunneling Protocol (SSTP)  
* Employs PPP authentication techniques  
* Encapsulates the data packets in HTTP used through Web communications  
* Additionally uses an SSL channel for secure communication, SSL for data encryption between server and client. SSL has evolved into TLS  
* Viewed as more secure than PPTP or L2TP  

<br>

SSTP only requires port 443 to operate, thus, it is the most firewall friendly VPN protocol  
<br>

Internet Key Exchange Version 2 Protocol (IKE v2)  
* Employs IPsec in tunnel mode protocol over UDP ports 500 and 4500  
* Encapsulates datagrams by using IPsec ESP or AH headers for transmission over the network  

<br>

IKE v2 Encryption:
* AES-256, AES-192, AES-128, and 3DES encryption algorithms  
* 3DES is not recommended  

<br>

IKE v2 Pros:
* Believed to be a faster and extremely secure protocol  
* Supports mobility (MOBIKE), much more resilient to changing network connectivity with the ability to swtich between access points/ wired to wireless connection  

<br>

IKE v2 Cons:
* Proprietary (like SSTP)
* But there are open-source implementations
* Support by some Network Device Vendors

<br>

IKE v2 is the only built-in VPN protocol that supports mobility connections (e.g. remote clients are constantly moving)  

<br>

### Table of Remote Access Protocols  

Note that these built-in VPN protocols can be implemented in the RAS server concurrently, meaning RAS can be configured to support remote clients using any of the 4 VPN protocols to a site  

| VPN Protocol | Encryption | Ports | Compatible with | Notes |
| :---------------| :------------| :------------- | :-----------------| :---------|
| PPTP | MPPE with RC4 128-bit key | 1723&#160;TCP | Windows, Mac OS, iPhone, Android | PPTP is a widely used VPN protocl, easy to setup. <br>Not considered for site to site VPN setup. <br>Not recommended to be used for VPN sessions that last longer than a couple of hours. |
| L2TP | IPsec with 3DE 168-bit key | 500&#160;UDP<br>1701&#160;UDP<br>5500&#160;UDP| Windows, Mac OS, Android | Considered to be more secure than PPTP. |
| SSTP | SSL with AES 2048-bit key cert, 256-bit key for encryption | 443&#160;TCP | Windows | Only use port 443, usually not blocked by firewalls.<br>SSTP is considered a very secure protocol. |
| IKEv2 | IPsec with AES 256-bit key | 500&#160;UDP<br>4500&#160;UDP| Windows, Blackberry | Considered to be a fast and extremely secure protocol.<br>Supports mobility (MOBIKE). |  

<br>

## Configuring a VPN Server  

### General steps

1. Install the Network Policy and Access Services role  

2. Configure a Microsoft Win Server 2016 as a network's VPN server, including configuring the right protocols to provide VPN access to clients  

3. Configure a VPN server as a <b>DHCP Relay Agent</b> for TCP/IP communications  
4. Configure the VPN server properties  

5. Configure a remote access policy for security  

<br>

Activity 9-1 of textbook shows how to install the Network Policy, Access Services and Remote Access roles  

Activity 9-2 of textbook takes you through the initial setup of a VPN server  

<br>

### Simulated VPN Setup  

Diagram for a network infrastructure configuration to go through a simple RAS or VPN gateway setup. This simulated network requires 4 VMs, 3 of them are 3 Windows systems and the remaining VM is a Linux system that serves as a virtual router in this setup  
![image](../images/Pasted%20image%2020230820180935.png)  

<br>

### Configuring the Server's Firewall  

The VPN server must be able to send communications through the network  
* An early configuration step is to make sure its communications can go through a firewall setup at the server

<br>

If you are using Windows Firewall on your server  
* TCP and UDP ports used by VPN are unblocked by default when you configure a VPN server  
* However, it is important to ensure they are unblocked (Ports used by VPN)  

<br>

Activity 9-3 from textbook covers this  

Quick [video](https://login.microsoftonline.com/common/oauth2/authorize?client_id=cf53fce8-def6-4aeb-8d30-b158e7b1cf83&response_mode=form_post&response_type=code+id_token&scope=openid+profile&state=OpenIdConnect.AuthenticationProperties%3dAQAAAAIAAAAJLnJlZGlyZWN0Smh0dHBzOi8vd2ViLm1pY3Jvc29mdHN0cmVhbS5jb20vdmlkZW8vYzJmZTZhZmYtMWI4MS00OWE4LThmODMtNTFhMDUzNDI1YWQyCG5vbmNlS2V5mwEzdDlCbG5sM1JDM3pITll5ZGhrRE1BS2VEckNZNkZyZk5LeGtabkwxN0hRbjJyMDNmLVBnQ3FNaWdUV2Q3QTRzSFprUndDMi1oZjVMY0JmMVFpVWE4dHBxRV9EWjVyYVRCTlF3Mkc0UnFjUk5RN0RvUzcwZDNvSEpmSUZPTWFuVXgxSEMzUWgxaXIxT2NGblZTUVd4Y0tFZ1hKSQ&nonce=638281231760054001.MmE4MjJlN2QtNjg2MS00ZDMxLWE3YTktOGVhMDQzMDlmNzMwOGZkZmE5MzktYzJhYi00NGExLWE2ZDMtZTc5NTI0ZGIxNzJj&nonceKey=OpenIdConnect.nonce.itHphQD%2blGxtcIzoJ7iePQvl0UmQGuzNUgBjZ5YcHis%3d&site_id=500453&redirect_uri=https%3a%2f%2fweb.microsoftstream.com%2f&post_logout_redirect_uri=https%3a%2f%2fproducts.office.com%2fmicrosoft-stream&msafed=0&prompt=none&sso_reload=true) for VMs in the simulated network  

For a simple VPN setup in a lab environment, all server firewall configurations may be completed via installation wizard  

In a production system, additional protections are usually put in place with firewall devices. In this case, you need to understand the configuration requirement for various types of VPN protocols  

<br>

### Configuring VPN Properties  

RAS Demo 2, [Features of RAS](https://web.microsoftstream.com/video/3d1a4030-6bc9-41e5-8557-b7f68747d984)  

<br>

After the VPN server is setup  
* The configuration wizard will be invoked to guide you through the configuration  
* RAS Demo 3 - [Configuration Wizard](https://web.microsoftstream.com/video/c3d88644-ebdd-4618-b8e6-a29a6a1a3f82)  

<br>

After the initial setup. You can configure it further from the Routing and Remote Access tool by right-clicking the VPN server in the tree and clicking Properties  

<br>

2 tabs of the VPN Properties menu  
General tab shows the current setting of the VPN server  
Security tab provides the configurations of L2TP/IKEv2 channel (AKA L2TP/IPsec)  
This sample setup did not enable the IPsec protocol  
At the bottom part, the cert dropdown list is used to configure the SSTP VPN  
By default, PPTP, SSTP and IKEv2 are all enabled once the Remote Access Services is up and running  
![image](../images/Pasted%20image%2020230820181825.png) ![image](../images/Pasted%20image%2020230820181828.png)  

<br>

### Configuring a DHCP Relay Agent  

DHCP Relay Agent broadcasts IP configuration info between the DHCP server on a network and the client acquiring an address  

VPN server can be configured as a DHCP Relay Agent by using the Routing and Remote Access tool  

<br>

In basic terms:  
* The client contacts the VPN server to make a connection  
* The VPN server, as a DHCP relay agent, contacts the DHCP server for an IP address for the client  
* The DHCP server notifies the VPN server of the IP address  
* The VPN server relays this IP address assignment to the client  

<br>

RAS Demo 4a and 4b (Demo how a remote VPN client connects to an RAS server via PPTP)  
* [The VPN Connection Configuration at the remote client](https://web.microsoftstream.com/video/0382ec33-9b3c-44f5-8cb9-e67e479b0ab8)  
* [Running RSAT via VPN](https://web.microsoftstream.com/video/68260fb9-00ab-4d0f-8ccc-7d7bcca3a97a)  

<br>

Activity 9-4 from the textbook shows how to setup a DHCP relay agent  

Activity 9-5 from the textbook instructs in how to configure the DHCP relay agent hop count  

RAS Demo 5 [Configure DHCP Relay Agent and setup PPTP only VPN](https://web.microsoftstream.com/video/5ae4a886-8024-44db-b7e8-143a1182a56d)  

<br>

### Configuring VPN Security  

VPN security can be setup through a remote access policy, this greatly reduces administrative overhead and offers more flexibility and control fo authorizing connection attempts  

<br>

Elements of a Remote Access Policy  
* Access permission  
* Conditions  
* Constraints  
* Settings  

<br>

RAS Demo 6 - [Remote Access Policy for VPN via NPS](https://web.microsoftstream.com/video/b142248f-79bb-40cc-af50-d6514744e5be) (Network Policy Service)  

Remote Access Polciy can be setup to allow or deny remote access requests  

By default, the remote access policy is set to deny all  

Therefore, even when the RAS services is up and running, remote clients will not be able to connect to it until the Remote Access Policy is configured properly  

<br>

First step in evaluating access is to determine if access permission is enabled at the VPN server  
* Default permission for this policy is set to Deny access  
* Change the default permission to Grant acess in the default remote access policy  

<br>

The conditions of a remote access policy are a set of attributes that are compared with the attributes of the connection attempt  

If the connection attempt matches the conditions of a remote access policy, the constraints are evaluated  

<br>

Settings in the remote access policy are then examined  
* Settings include elements such as IP filters, encryption, IP settings, and others  

<br>

Establishing a Remote Access Policy  
* You can use the Routing and Remote Access tool to create and configure a remote access policy  
* To create a new remote access policy, click to activate and then right-click the Remote Access Logging & Policies folder in the tree under the VPN server, click Launch NPS to launch the Network Policy Server tool  

<br>

Activity 9-6 from the textbook shows how to configure a remote access policy  

Screenshot of NPS  
![image](../images/Pasted%20image%2020230820183251.png)  

### Monitoring VPN Users  

After configuring the VPN server and running it in production, you should periodically monitor the users who are connected  

Use the Routing and Remote Access tool to monitor connected users  

<br>

With the tool open:  
* Expand the elements under the server name in the left pane
* Click Remote Access Clients in the left pane  
* The right pane shows the users who are connected, if you want to disconnect a user, right-click the user and click Disconnect  

<br>

## Troubleshooting VPN Installations  

Troubleshooting VPN communications can be divided into hardware and software troubleshooting  

<br>

### Hardware solutions  

Use Device Manager to ensure network adapters and WAN adapters are working properly, also use Device Manager to ensure that an adapter has no resource conflicts  

For an external DSL adapter or a combined DSL adapter and router, ensure the device is properly configured and connected. Check its monitor lights for problems  

Call your ISP to determine if problems are present on the ISP's WAN  

<br>

### Software solutions  

Use the Services tool or Server Manager to ensure the following services are started:  
* IP Helper
* Remote Access Connection Manager  
* IKE and AuthIP IPsec Keying Modules  
* Routing and Remote Access  
* Netlogon  
* Remote Access Management service  
* Base Filtering Engine  
* Windows Firewall  
* Secure Socket Tunneling Protocol service  

<br>

Ensure Windows Firewall is setup to allow remote access  

Ensure that VPN server is enabled  

Check the <b>remote access policy</b> to be sure that access permission <b>is granted</b>  

Be certain that the VPN server is started  

In the Routing and Remote Access tool, check the network interface  

Ensure that the IP parameters are correctly configured to provide an address pool for either a VPN server  

Check to ensure the remote access policy is consistent with the user's access needs  

Open the Remote Access Management Console and click Dashboard in the left pane  

<br>

If only certain clients but not all are having connection problems, try these solutions:  
* Ensure clients are using the same communications protocol as the server  
* If you manage access to a VPN server by using security groups, ensure that each user account or PC account that needs access is in the appropriate group  

<br>

In short, troubleshooting VPN issues requires re-checking of the overall hardware connections and the system configurations  

This checking requires the thorough understanding of network infrastructure, VPN protocols, Windows and/or AD operations  

<br>

## Connecting Through Remote Desktop Services  

Remote Desktop Services (RDS) server enables clients to run sessions-based desktops, virtual desktops, and software apps on Win Server 2016 instead of running them on the client  

Using session-based desktops, the client accesses an RDS server to run apps on that server during a connection session  

<br>

Virtual desktops  
* Are used in association with accessing virtual machines on a virtual server through Hyper-V  
* Multiple virtual desktops can be in a pool of desktops for different purposes  

<br>

In general , this way of remote access offers two additional remote access features that VPN not supports  

One is allow thin client connections  

The other is, it enables the server administrators to know the details of the remote clients activities  

<br>

Win Server 2016 Remote Desktop Services are used for 3 broad purposes:  
* To run apps on a server instead of the client  
* To support thin clients
* To centralize program access  

<br>

Thin clients  
* Downsized PCs that have minimal Windows-based OSs that access a Win Server 2016 server so that most CPU-intensive operations are performed on the server  
* Generally used to save money and reduce training and support requirements  
* Also used for portable field or handheld remote devices  

<br>

Centralize control of how programs are used  
* Highly-classified or sensitive documenta can be stored and modified only on the server  
* Server can be configured to provide a high level of security  

<br>

RDS not only supports thin clients, but also other types of OSs including:  
* Windows 10  
* Windows Server 2016  
* UNIX and UNIX-based X-terminals  
* Linux  
* Mac OS  
* Tablet operating systems  

<br>

Four main components enable remote desktop server connectivity:  
* Win Server 2016 multiuser Remote Desktop Services  
* Remote desktop services client  
* Remote Desktop Protocol (RDP)  
* Remote Desktop Services administration tools  

<br>

See Table 9-6 from textbook for a description of the 4 main components  

<br>

When you install RDS, you can install different role services:  
* Remote Desktop Session Host  
* Remote Desktop Web Access  
* Remote Desktop Virtualization Host  
* Remote Desktop Licensing  
* Remote Desktop Gateway  
* Remote Desktop Connection Broker  

<br>

See Table 9-7 from textbook for a description of each service  

<br>

An RDS server employs RemoteApp  
* A feature that enables a client to run an app without loading a remote desktop on the client PC  
* The program appears to just be another program in a window running on the local PC  

<br>

A RemoteApp program is started by clients in the following ways:  
* From an icon on the client's desktop  
* From the client's Start menu  
* As a link on a website via Remote Desktop Web Access  
* As an .rdp (remote desktop protocol) file  

<br>

Simulated Remote Desktop Server setup diagram  
![image](../images/Pasted%20image%2020230820193800.png)  

<br>

## Installing Remote Desktop Services  

When the Remote Desktop Services role is installed, you also need to install the RDS Licensing role service to manage the number of terminal server user licenses you have obtained from Microsoft  

The RDS Licensing role server can be installed when you install the Remote Desktop Services role  

Licenses can be purchased either per user account or by client device  

<br>

When you install the RDS role, you implement Network Level Authentication (NLA)  
* NLA enables authentication to take place before the RDS connection is established  
* Designed to eliminate man-in-the-middle attacks  

<br>

Another element to consider before you install the RDS role is who will be allowed to access the RDS server. Create groups of user accounts in advance so that you can add these groups during the installation  

<br>

If operating in an AD environment  
* Consider creating a domain local group of users, such as RDS Users  
* Next, create different global groups of users, such as a global group for each department that will access the RDS server  
* Add the appropriate user accounts for each department's global group  
* Finally, add the global groups to the single domain local group  

<br>

Activity 9-8 from textbook shows how to install the Remote Desktop Service role  

Remote Desktop Services Demo (part 1) [video](https://web.microsoftstream.com/video/32f973bf-eec8-44bf-8a74-f6e36142e515)  

Diagram of Role Services for Remote Desktop Services  
There are 6 Features available in the RDS role, all of which are high on resource demands (use a lot of computing resource)  
![image](../images/Pasted%20image%2020230820194333.png)  

<br>

## Configuring Remote Desktop Services  

After installing the RDS role and the associated role services, use Server Manager to complete a basic configuration as shown from Activity 9-9 from the textbook  

### Built-in Remote Desktop Services  

The installation of a full sccale Remote Desktop Services may demand a high amount of system resources, through Remote Desktop Role and features  

<br>

For all Win Servers (and some Windows 10 editions), there are built-in Remote Desktop Services  
* With limited number of connections  
* With simplified configuration options  
* Run on the same Remote Desktop Protocol  
* [RD services Demo (part 2)](https://web.microsoftstream.com/video/575150e3-6e49-4c36-861e-f22939a0ecee)  

<br>

### Accessing an RDS Server from a Client  

Remote Desktop Services client PCs can sign in using the Remote Desktop Connection (RDC) client  
* The RDC client is already installed in Windows Vista through Windows 10 and in Win Server 2016  
* [RD Service Demo (part 3: RDC)](https://web.microsoftstream.com/video/ecfbf5ee-acff-47f9-b96e-6b010cf3c36e)  

<br>

The general steps to start RDC client in Windows 10 or Win Server 2016 are as follows:  
* Click Start and click the Windows Accessories folder  
* Click Remote Desktop Connection  
* Enter the name of the PC to access and click Connect  
* Provide the username and password and click OK  

