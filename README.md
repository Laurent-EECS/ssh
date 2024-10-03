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

### 2.1. Create a personal user account
Open a Terminal - also known as a shell or a Command Line Interface - on your local Operating System.

Then, type in the command prompt :

(press ENTER after each command line or after responding to a Terminal request)
```
ssh root@ip_address
```
The *ip_address* field corresponds to the public IP address of your remote computer system, for example *70.80.90.10*. A domain name also works if exists ! In that case, we can type *@remoteserver.com* instead of *@70.80.90.10*.

Type the password attached to the **root** of your remote computer system and press ENTER.

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

Before logging out, you need to extend your privileges for your personnal account ! You will need them to manage the SSH configuration.

**WE NEVER USE THE ROOT ACCOUNT TO OPERATE ON A SYSTEM ! SAFETY ISSUE !**

### 2.2. Extend your privileges for your user account
As **root**, type the command :
```
visudo
```
The command opens a configuration text file */etc/sudoers.tmp* in your Terminal.

Go to the section :
```
# User privilege specification
root    ALL=(ALL:ALL) ALL
```
Add the line *user_name* as below :
```
# User privilege specification
root    ALL=(ALL:ALL) ALL
user_name    ALL=(ALL:ALL) ALL
```
Press CTRL+X to exit and save the modification of the file, press Y to validate then ENTER to confirm.

After this operation, you can logout from **root**  by typing *exit* or *logout* :
```
exit
```
You can now access your remote computer system with the command :
```
ssh user_name@ip_address
```
Type the chosen password for your personal user account !

## 3. Problems encountered and found solutions

## 4. Theory

## 5. Conclusion

## 6. Ressources


