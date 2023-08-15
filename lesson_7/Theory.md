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

## Selecting a Certificate Enrollment and Renewal Method  

## Configuration of the Web Server for SSL Certificates  

## Configuration of the Client for SSL Certificates

## Certificate Renewal