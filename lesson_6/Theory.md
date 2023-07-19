# Objectives  

IP Security Issues and IPsec Concepts  

IPsec Implementation, IPsec Policies and IPsec Rules  

Know implications of deploying IPsec  

Plan an IPsec deployment  

Understand default IPsec policies  

Configure IPsec policies  

Deploy and manage IPsec policies  

<br>

# Navigation  
* [IPsec Security Issues and Concepts](#ipsec-security-issues-and-concepts)  
* [IPsec Implementation](#ipsec-implementation)  
* [Planning an IPsec Deployment](#planning-an-ipsec-deployment)  

<br>

## IPsec Security Issues and Concepts  

### IPsec Concepts  

IPsec is a set of protocols used to ensure private, secure communications over IP networks by using cryptographic security services based on  
* Protecting the contents of IP packets
* Providing packet filtering
* Enforcing trusted communication
* Securing communication with encryption of the info traveling the network
* Mostly used as an option for IPv4 implementations  
* From Win Server 2008 R2, IKE2 has been a default built-in VPN protocol for Remote Access Services

<br>

### IP Security Issues  

Original TCP/IP Network protocol was not designed with security in mind
* Source spoofing
* Replay packets
* No data integrity or confidentiality

<br>

Subject to
* DOS attacks, Replay attacks, Spying and more

<br>

IPsec is an optional feature that the network admin can use to protect TCP/IP network traffic, by using combinations of encryption and hashing schemes

<br>

### IPsec Control Elements

3 main security control elements
* <b>Internet Key Exchange (IKE)</b> protocol for exchanging encryption keys, AKA IKEEXT in Microsoft Implementation  
* <b>Authentication Header (AH)</b> provides authenticity guarantee for packets, data origin authentication, data integrity, and reply protection  
* <b>Encapsulating security payload (ESP)</b> proivdes confidentiality through encryption  

<br>

IPsec also uses IP Compression (IPComp), which is used to compress raw IP data  

From the 3 main security control elements, IKE is the 1st to be used when 2 parties want to communicate  

If the IKE operation (i.e. Host-to-host authentication) fails, there is no further communication possible  

Once IKE is cleared, the 2 parties may use AH, and/or ESP to protect subsequent traffic data  

<br>

### IKE Module  

Internet Key Exchange (IKE) module  

1 of the key components of IPsec  

Responsible for  
* Initial security negotiation (<b>Phase 1</b>), based on the Authentication method(s) defined in the IPsec rule  
* Determine secret keyring material (<b>Phase 2</b>), to secure network communication, and related to Filter Action setting of the associate IPsec rule  

<br>

Phase 1 is required to support phase 2 operations, the 2 phases use different security and encryption schemes  

The 2 phases use different DH (Diffie-Hellman) asymmetric key encryptions to exchange data between 2 communication parties  

<br>

## IPsec Implementation  

Win Server 2016 supports implementation of IP security (IPsec)  

When an IPsec communication begins between 2 PCs, the PCs 1st negotiate (using <b>IKE module</b>) and authenticate between the receiver and sender (Using <b>Microsoft AuthIP extension</b>)  

Next, extra hashing scheme (<b>optional</b>) will help to ensure data integrity at packet header  

Finally, data is encrypted with integrity check (<b>optional</b>) at the NIC of the sending PC as it is formatted into an IP packet  

<br>

Reminder  
* Authentication Header (AH) – is a packet level authentication protocol to ensure the data integrity of the exchanged packets
* Encapsulating Security Payload(ESP) – is another packet level protocol which provides both encryption and integrity check features to protect the data packets

<br>

IPsec can provide security for all TCP/IP-based apps and communications protocols  

<br>

IPsec security policy  
* <b>IPsec security policies</b> can be established through the <b>Group Policy</b> in an AD  
* IPsec security policies can also be configured through the IP Security Policies Management MMC snap-in  

<br>

Windows Firewall Advanced Security Rule
* IPsec security can also be configured via Windows Firewall Advanced Security Rules  
* Provide more IKE authentication options and AES encryption support  

<br>

Admins may use IPsec security policies to define IPsec configurations and these IPsec policies have to be deployed to the target systems to take effect  

Another apporach to configure IPsec security is to use Windows Firewall Advanced Security Rules  

GP is helpful for admins to manage and distribute IPsec policies to targeted domain machines  

IPsec security policy is independent to Group Policy  

IPsec policy can be defined at the individual system locally via local policy  

<br>

### Weighing IPsec Trade-offs  

Deploying IPsec can affect network performace and compatibility with other services and apps  

Do not deploy IPsec if the security it provides is not required  

Applying IPsec or not is a critical decision to make by the network or security admins  

<br>

Impact of IPsec
* Time needed to establish IPsec connection
* Time needed to filter and encrypt packets  
* Increased packet size  
* Network inspection technologies used in routers, firewalls, <b>Intrusion Detection Systems (IDS) may not work on IPsec packets</b>  
* App compatibility with other platforms  
* Effect on AD and DC connections  

<br>

IKE initial connection time will take longer to be established  

During actual data exchange, each packet may be encrypted, causing some overhead  

Additional packet header will be needed too  

Adding in IPsec protection may upset some of the existing security measures  

One of the most popular IKE authentication schmes for an AD is Kerberos, thus, it is complicated if IPsec is involving DCs as Kerberos is hosted by controllers  

<br>

## Planning an IPsec Deployment  

Configuration of IPsec is based on
* The way IPsec is used  
* Types of client OS in the network  
* Types of network devices in the network  

<br>

Different OS and Network Devices may have different ways for IPsec implementation  

<br>

IPsec is not recommended for the following  
* To secure communication between domain members and their DCs, as it reduces network performance, and configuration of IPsec policy is complicated  
* To secure all traffic in a network, reduces network performance, IPsec can't support multicast and broadcast traffic, and <b>Network tools that need to inspect packet headers may not work</b>

<br>

Protected network traffic could also help adversaries to carry out data exfiltration undetected, thus IPsec could also decrease network security when used for all network traffic

<br>

### IPsec Security solutions  

| Concern | Solution |
| :---------| :---------|
| Attacks using specific protocols or Denial of Service attacks | Use IPsec traffic blocking, packet filtering, or policy filter lists allowing only traffic from trusted sources over specified protocols to specific addresses and ports. |
|  Eavesdropping or sniffing | Use the Encapsulating Security Payload (ESP) protocol to encrypt data with Triple Data Encryption Standard (3DES) or Data Encryption Standard (DES). |
| Identity spoofing | Use Kerberos, public key certificates, or preshared key authentication to verify the identity of PC. |
| Modification of data | Use a cryptographic checksum that incorporates a secret key to provide data integrity. |

<br>

Proper uses of IPsec
* Packet filtering such as using IPsec with the Routing and Remote Access Service to permit or block inbound or outbound traffic  
* <b>Securing host-to-host traffic on specific paths</b>
* <b>Securing traffic to servers</b>
* Combining L2TP and IPsec (L2TP/IPsec) for securing VPN scenarios  
* Incorporating site-to-site or gateway-to-gateway tunnelling

<br>

The highlighted cases are suitable to be used within a LAN  

In a simple network structure a <b>single IPsec policy</b> can be designed that may contain <b>multiple rules</b>, <b> One policy for all</b>  

In a large environment, many different IPsec policies may be designed  

Factors that can increase the numebr of policies required include  
* PC Roles
* Sensitivity of data traveling over the network  
* PC OS
* Domain relationships and memberships  
* Number of apps requring IPsec

<br>

In Windows IPsec implementation, each Windows system can only be deployed with at most 1 IPsec policy  

In the event that a system needs to host different apps with IPsec protections, an IPsec policy needs to be defined for this system with multiple rules  

Each rule is to cater for 1 type of app  

