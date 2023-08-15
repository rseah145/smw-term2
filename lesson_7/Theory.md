# Objectives

Define certificate requirements  

Plan, build, and manage certification authority hierarchies  

Select a certificate enrollment and renewal method  

Configure and deploy certificate authorities (CA)  

Considerations of secured Web Content access  

Deploy and manage SSL certificates  

Configure a Web Server for SSL certificates  

Configure a client for SSL certificates  

Determine certificate renewal  

<br>

# Navigation  

* [Defining Certificate Requirements](#defining-certificate-requirements)  
* [Selecting a Certificate Enrollment and Renewal Method](#selecting-a-certificate-enrollment-and-renewal-method)
* [Configuration of the Web Server for SSL Certificates](#configuration-of-the-web-server-for-ssl-certificates)
* [Configuration of the Client for SSL Certificates](#configuration-of-the-client-for-ssl-certificates)
* [Certificate Renewal](#certificate-renewal)

<br>

## Defining Certificate Requirements  
Microsoft Win Server enables secure data access based on the use of <b>digital certificates</b>  

Certificates allow users or systems to prove to others that they are who they say they are  

Before you can use digital certificates, you need to design a <b>Public Key Infrastructure (PKI)</b>  

<br>

<b>To protect exchanging Messages from eavesdropping attack with Asynchronous Encryption Algorithm<b>
* A key-pair is associated with an individual party
* Each individual party will keep its private key as a secret but share out its public key to everyone
* Messages beng encrypted with the public key can only be decrypted by the corresponding private key
* A digital signed message with the individual private key can only be verified by its corresponding public key

<br>

<b>There is one problem</b>
* How can we obtain a genuine public key of a particular party before we can establish a secure communication channel with the party?

<br>

<b>Public Key Infrastructure (PKI)</b>
* Refers to tech that includes a series of features relating to authentication and encryption
* Based on a system of certificates, each cert contains a public key and the name of the subject
* Allows an org to publish, use, renew, and/or revoke certs and enroll clients

<br>

### What is a digital cert?  

Vid [src](generally_glad_grubworm_LmcLZhFf) (watch from 14:21 and onwards)  

Why do we need it?  
To hold a trustworthy public key, this key will be used for subsequent secure communication between the cert recipient and the owner of the public key, the owner of the public key is the one who knows the corresponding private key  

<br>

How to tell if a digital cert is genuine?  
We need to have Root CA and its subsidary Issuer CAs which will be responsible for signing on the digital cert  
If we receive a digital certificate and we can verified it is signed by a trustworthy Root CA (or its subsidiaries) , we will trust the certificate as well as the content , that is the public key  

<br>

Certificates may be required for the following apps
* Digital signatures
* Secured e-mail
* <b>Secured HTTP communication (HTTPS/SSL)</b>
* Internet authentication
* <b>Software code signing</b>
* IP security
* Smart card logon
* Encrypted file system
* <b>802.1x authentication</b>

<br>

### Certificate Authority (CA)  

A Certificate Authority (CA) is an entity that issues digital certs to other parties  

The Root CA can issue certs to subordinate or intermediate or issuing CAs  

Issuing CAs can issue certs to users or clients  

<br>

### Certificate Authority Hierarchies and Roles  

Types of hierarchies that can be used for CAs  
* Rooted trust model  
* Cross-certification trust model  
* Hybrid trust model  

<br>

Roles that can be chosen for CAs include  
* For standalone CA, Rudimentary CA, and Intermediate CA  
* For Enterprise CA, Basic-security CA, Medium-security CA, and High-security CA  

<br>

### Rooted Trust Model    

A model in which the root CA has a self-signed cert, and the CA issues a cert to all direct subordinate CAs  

CAs in this model can be online or offline; allows flexibility in deploying and managing PKI  

Each CA serves a single role within the hierarchy and is not dependent on other CAs; allows rooted trust hierarchies to be more scalable and easier to administer than other hierarchies  

Possible to add a new CA to a hierarchy  

Rooted trust hierarchies fall into 2 subcategories: two-tier CA hierarchy and three-tier CA hierarchy  

Any CA in a rooted trust hierarchy is either a root or a subordinate but never both  

Each CA is responsible for processing requests, issuing certs, revoking certs, and publishing CRLs  

Each CA can be managed independently  

Diagram illustration  
![image](../images/Pasted%20image%2020230815151717.png)  

Shows the Root CA is supposed to be trusted by everyone  

It will sign a few certs to empower a couple of servers to become intermediate CAs  

These few intermediate CAs will then empower the next level of CAs  

At the lowest level, these servers will be operational servers which issue out digital certs to clients which ask for services  

Note that the top level CA in the Rooted Trust Model is always based on a self-signed cert to distribute out its public key, everyone will unconditionally trust this self-signed cert and the public key it holds  

<br>

### Cross-Certificates Model  

A model in which all CAs are self-signed and trust relationships between CAs are based on cross-certitficates, certs issued by 1 CA will be trusted by PCs which belong to other CA hierarchy (cross hierarchy trust)  

Cross-certs does not need to be bidirectional  

Advantages: Low cost and greater flexibility  

Disadvantages: Greater administrative overhead and increase risk of unauthorized access to internal resources  

Diagram for Cross-Cert Model  
![image](../images/Pasted%20image%2020230815152154.png)  

Let's encrypt brief explanation of how Chain of Trust works in cross-cert model  
![image](../images/Pasted%20image%2020230815152251.png)  

<br>

### Using Third-Party CAs  

Useful when an org conducts most of its business with external

## Selecting a Certificate Enrollment and Renewal Method  

## Configuration of the Web Server for SSL Certificates  

## Configuration of the Client for SSL Certificates

## Certificate Renewal