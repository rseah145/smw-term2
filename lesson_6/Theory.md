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
* [Configuring IPsec Policies Between Networks and Hosts](#configuring-ipsec-policies-between-networks-and-hosts)

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

<br>

### 5 Elements that make up a rule

Each IPsec Policy can consist of multiple rules  

<br>

Each rule has 5 configurable elements  
* Connection Type (Lan, Remote, ALL)  
* Mode (Transport or Tunnel)  
* Filter List  
* Host-to-Host Authentication Method, used by IKE - phase 1  
* Filter Action (include choice of AH/ESP), negotiated in the IKE-phase 2, and carried out the agreed scheme in the actual traffic  

<br>

### Connection Type

The rule only applies for particular types of connections
* LAN
* Remote
* ALL

![image](../images/Pasted%20image%2020230719213601.png)

<br>

### Deciding which IPsec mode to use  

2 modes  

<br>

Transport mode
* Used to secure communications within a network and it can be server-to-server or server-to-client  
* IPsec provides end-to-end security  

<br>

Tunnel mode
* Used to secure communications between networks (E.g. Between 2 gateways)  
* Primarily used for interoperability with gateways or end systems that do not support L2TP/IPsec or PPTP VPN site-to-site connections  

<br>

In the simplest case
* IPsec protection is catering for systems within a LAN
* Just choose Transport mode  

<br>

When connections are involving the WAN (Wide Area Network)  
* Most probably we will use Tunnel Mode  
* Examples provided  

<br>

Routers and firewall devices will take care of site-to-site traffic security in most cases, based on IPsec tunnel mode  

Tunnel mode only protects traffic data between the 2 end-point devices, traffic between host to the corresponding end-point is not protected by AH nor ESP protocols  

<br>

### Case study

Using IPsec to secure network traffic between Alice's PC and the HR server located in the remote office  

![image](../images/Pasted%20image%2020230719214410.png)  

<br>

Scenario A, Tunnel between router and Firewall so Alice can connect to HR servers securely (gateway to gateway) (traffic between Alice's PC and HR server is protected by IPsec tunnel)  

![image](../images/Pasted%20image%2020230719220644.png)  

<br>

Scenario B, Tunnel between VPN client on Alice's PC and IPsec gateway (PC tunnel endpoint to edge router) (traffic from PC to router is protected by IPsec)

![image](../images/Pasted%20image%2020230723184634.png)

<br>

Scenario C, Tunnel between router and IPsec software running on HR Server where tunnel endpoints are the router (Alice PC's router) and the HR server. Difference between this and Scenario B is Only traffic forwarded from the router to the HR server is protected by the IPsec tunnel

![image](../images/Pasted%20image%2020230731202051.png)

<br>

Scenario D, Transport mode (A-C use Tunnel) between Alice's PC logging in to the Firewall to configure it (client to server). Alice's PC connects to the office firewall with a VPN connection, therefore Alice's PC can use IPsec in transport mode to establish a secured communication channel between the PC and the HR server. However, if the VPN is already providing secured communication, IPsec protection might be redundant

![image](../images/Pasted%20image%2020230731202506.png)

<br>

### IPsec Filters

IPsec filter is a specification in the IPsec rule that's used to match IP packets, filters are grouped together in a system wide Filter List  

In the IPsec rule properties setting, the system wide filter list will be available for the choice of the filter  

Packets which match a filter will be applied with the associated filter actions such as permit, block, or negotiate security  

<br>

Cavets  
* <b>A system configured with IPsec may not apply the expected security scheme cause the filter is set wrongly</b>
* <b>When the network traffic does not match the IPsec, it will not be blocked, but it will just pass through</b>  

<br>

IPsec filter is the other important element associated with an IPsec rule.  Compared with firewall filters, when incoming traffic is not matched with a firewall rule, traffic is blocked (implicit deny). For IPsec filter rules, if the traffic does not match any IPsec rule, IPsec will not do anything against the traffic (just let it through, no implicit deny)  

Filter list identifies traffic based on its source, destination, and protocol. Examples use Win2k3, Win2k8 are All ICMP Traffic and All IP Traffic  

Filter action is set for each type of traffic as identified by a filter list (Win Server since 2k3), Permit. Request Security, Require Security  

Admin may construct/ customise filter (rule) actions  

<br>

IPsec policy can be managed in 1 of 2 ways (either create the policy or filter list first)
* Create a new policy and define the set of rules for the policy, adding filter lists and filter actions as required
* First create the set of filter lists and filter actions, then create the policies and add rules that combine the filter lists with filter actions

<br>

### Planning Authentication Methods for IPsec

If the connection matches the Filter IKE (phase 1) will be invoked for initial authentication  

<br>

Available Authentication Methods  
1. Kerberos V5
2. Certificates, requires PKI (Public Key Infrastructure)
3. Pre-shared Key

<br>

The authentication process is done using DH asymmetric key encryption for data exchange  

<br>

Kerberos V5 authentication
* Default authentication method from Win server 2000 - present
* Based on mutual authentication
* <b>Use when all clients can authenticate using Kerberos V5</b>
* A method <b>that requires the least administrative effort</b>

<br>

Certificates authentication
* A method of granting access to users based on their unique ID
* Used in situations that include access to corporate resources, external business partner communication, or PCs that do not run Kerberos V5

<br>

Pre-shared Key authentication
* Used when other methods are not available
* It is a shared secret key (E.g. string to configure a pre-shared key)

<br>

Choosing an IPsec Authentication Method
* More than 1 can be selected
* With designated priority level
* IKE - Phase 1, will sort out if 2 parties share 1 common authentication method

<br>

If1 endpoint supports both Pre-shared Key and Certificates authentication, whilst the other endpoint only supports Kerberos authentication, they will not be able to establish an IPsec channel  

<br>

## Configuring IPsec Policies Between Networks and Hosts

IPsec authentication specifies how PCs will prove their identities to each other when establishing a SA (Security Association)  

| Security Requirement | Authentication Method|
| :---------| :---------|
| Communication within a Win 2000 or Server 2003 domain or between trusted Win 2000 or server 2003 domains | Kerberos V5 |
| Communication outside the domain or across the Internet where Kerberos V5 is not available but CA access is  | Public Key certificate |
| Communication with PCs that neither support Kerberos V5 nor have access to a CA | Pre-shared key |

<br>

Pre-shared Key is usually for testing, short term ad-hoc solution. As the common downside to all pre-shared key schemes is its difficult to maintain and secure  
Kerberos is the best for domain based systems  
Certs are ideal for non-domain based (or mixed) systems  

<br>

### Filter Action

Filter Action will only trigger when the incoming or outgoing connections match the filter  

<br>

Available types of actions are
* Permit
* Block
* Negotiate Security

<br>

Negotiate security is done at Phase 2 of IKE  

<br>

### Encryption Levels

2 basic categories of encryption. Symmetric key and Public key encryption  

Lifetime settings determine when a new key is generated  

In most IPsec implementations, packet level encryptions will be based on symmetric key encryptions  

E.g. 3DES and AES, older systems may use DES  

During IKE phase 2, both parties will try to negotiate for a commonly available encryption scheme. Once determined, the filter action will based on it to encrypt the data packets  

<br>

Methods of <u>hashing</u>
* Secure Hash Algorithm (SHA): <b>160-bit</b> encryption key, high security method
* Message Digest 5 (MD5): <b>128-bit</b> encryption key, lower performance than SHA  
* Hashing method is used to support AH (Authentication Traffic)  

<br>

Methods of <u>encryption</u>
* Data encrypion standard (DES): <b>56-bit</b> key, low security
* Triple DES (3DES): <b>168-bit</b>, medium-high security  
* Encryption method is used to support ESP (Encapsulating Security Payload Traffic)

*Note: These hashing and encryption methods are older, newer systems may provide more choices  

<br>

### IPsec Protocols

| Requirement | Protocol | Solution |
| :--------- | :--------- | :--------- |
| The data and the header need to be authenticated and protected from modification, but remain readable | AH | Used in situations where data is not confidential but must be authenticated, or where network intrusion detection or firewall filtering requires traffic inspection |
| Only data needs to be protected so it is unreadable, but the IP addressing can be left unprotected | ESP | Used when data must be kept confidential, such as file sharing, database traffic, or internal Web apps that have not been secured properly |
| Both the header and data need to be protected while data is encrypted | Both AH and ESP | Used for highest security. If possible, use ESP alone instead |

Image reference  
![image](../images/Pasted%20image%2020230731210701.png)

<br>

Tunnel mode uses double encapsulation, which is suitable for protecting traffic between network systems, such as an untrusted medium like the Internet
* AH Tunnel Mode: Encapsulates an IP packet by placing an AH header between internal IP header and external IP header  
* ESP Tunnel Mode: IP packet is first encapsulated with an ESP header, then the result is encapsulated with an additional IP header  

Image references  
![image](../images/Pasted%20image%2020230731211034.png)  

![image](../images/Pasted%20image%2020230731211110.png)  

