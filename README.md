# SECURE SHELL PROTOCOL : SSH

1. [Introduction](#1-introduction)
2. [Operations on Linux distributions](#2-Operations-on-Linux-distributions)
4. [Problems encountered and found solutions](#3-problems-encountered-and-found-solutions)
5. [Theory](#4-theory)
6. [Conclusion](#5-conclusion)
7. [Ressources](#6-ressources)

## 1. Introduction
The Secure Shell protocol, commonly called SSH, is a secure communication protocol used to access and execute commands on remote computer systems.

SSH offers a number of features : encryption of exchanged data, authenticated access, tunneling, port forwarding and secure file transfer.

SSH replaces less secure protocols such as Telnet.

## 2. Operations on Linux distributions
Before you start, you must have **root mode access** to a remote computer system, such as a physical server or a Virtual Private Server (VPS).

**Root** refers to the primary or superuser account which has complete administrative privileges on a system, allowing it to perform any operation on it.

### Create a personal user account
Open a Terminal - also known as a shell or a Command Line Interface - on your local Operating System.

Then, type in the command prompt :

(press ENTER after each command line or after responding to a Terminal request)
```
ssh root@ip_address
```
The *ip_address* field corresponds to the public IP address of your remote computer system, for example *70.80.90.10*. A domain name also works if exists ! In that case, we can type *@remoteserver.com* instead of *@70.80.90.10*.

Type the password attached to the **root account** of your remote computer system and press ENTER.

**You must now be connected on your remote computer system as root !**

Then, type the following command line to create your user account :
```
adduser user_name
```
Here too, user_name corresponds to the username you choose for your account, for example *jane*.

Type a chosen password for your acccount. Retype your password to confirm !

The Terminal allows you to enter optionnal values such as *Full name*, *Room number*, and more. You can pass these steps by typing ENTER and set default values.

At the end, confirm the account creation by typing Y (Yes) or press ENTER directly to confirm.
```
Is the information correct? [Y/n] 
```
When a Terminal request is made, the uppercase value is the chosen one if you press ENTER without typing anything.

## 3. Problems encountered and found solutions

## 4. Theory

## 5. Conclusion

## 6. Ressources


