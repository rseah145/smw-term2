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
* 
