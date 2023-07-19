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
* [IPsec ]

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
