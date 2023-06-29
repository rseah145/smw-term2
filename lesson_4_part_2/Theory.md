# Continuation from slide 44 of lecture 4 slides (theory)  

## Objectives
Intro to GP  

## Supplimentary video for GPO precedence
[link](https://www.youtube.com/watch?v=orQns7K-brM)

## Navigation
* [Intro to GP](###introduction-to-group-policy)

### Introduction to Group Policy

GP in Win Server 2016 allows standardization of working environment of clients and servers via policies in AD.  <br>

#### Characteristics of GP

* Can be set for a <b>site</b>, <b>domain</b>, <b>OU</b>, or <b>local computer</b>
* Cannot be set for non-OU folder containers
* Settings are stored in GPO
* GPOs can be local and nonlocal
* Can be set up to affect all user accounts and PCs
* When GP is updated, old policies are removed or updated for all clients

PC configuration applies to computers
![image](https://github.com/b00tl04d/smw-term2/assets/108401257/ed79f424-aa32-4adb-aae8-8d1b8b19544f)


User configuration applies to users
![image](https://github.com/b00tl04d/smw-term2/assets/108401257/90641d16-23ea-4193-ba7e-99b53305ab75)