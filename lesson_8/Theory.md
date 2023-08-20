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
* Uses IP Security (IPsec) for additional authentication and for data encryption, IPsec i a set of IP-based secure communications and encryption standards created through IETF  

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
* AES-256, AES-192, AES-129, and 3DES encryption algorithms  
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