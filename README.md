# SECURE SHELL PROTOCOL : SSH

1. [Introduction](#1-introduction)
2. [Operations on Linux distributions](#2-Operations-on-Linux-distributions)
4. [Problems encountered and found solutions](#3-problems-encountered-and-found-solutions)
5. [Theory](#4-theory)
6. [Conclusion](#5-conclusion)
7. [Ressources](#6-ressources)

## 1. Introduction
The Secure Shell protocol, commonly called SSH, is a secure communication protocol used to access and execute commands on remote computer systems.

SSH offers a number of features : encryption of all exchanged data, authenticated access, port forwarding, tunneling and secure file transfer.

SSH replaces less secure protocols such as Telnet.

## 2. Operations on Linux distributions
Before you start, you must have **root mode access** to a remote computer system, such as a physical server or a Virtual Private Server (VPS).

**Root** refers to the primary or superuser account which has complete administrative privileges on a system, allowing it to perform any operation on it.

### 2.1. Create a personal user account
Open a Terminal - also known as a shell or a Command Line Interface - on your local Operating System. You can also press CTRL+ALT+T to open it.

Then, type in the command prompt :
```
ssh root@ip_address
```
Type the password attached to the **root** of your remote computer system then press ENTER.

Always press ENTER after a command line or after responding to a Terminal request.

Note that the *ip_address* field corresponds to the public IP address of your remote computer, for example *70.80.90.10*. A domain name also works if exists ! In that case, we can type *@remoteserver.com* instead of *@70.80.90.10* if our remote computer system has the domain name *remoteserver.com*.

**You must now be connected on your remote computer system as root !**

Then, type the following command line to create your personal user account :
```
adduser user_name
```
Here too, *user_name* corresponds to the username you will choose for your personal account, for example *jane*.

Type a chosen password for your personal user acccount. Retype your password to confirm !

The Terminal allows you to enter optionnal values such as *Full name*, *Room number*, and more. You can pass these steps by typing ENTER and set default values.

At the end, confirm the account creation by typing Y (Yes) or press ENTER directly to confirm.
```
Is the information correct? [Y/n] 
```
When a Terminal request is made, the uppercase value will be the chosen one if you press ENTER without typing anything.

Before logging out from the **root** account, you need to extend the privileges for your new personnal account ! You will need this privileges to manage the SSH configuration later !

**WE NEVER USE THE ROOT ACCOUNT TO OPERATE ON A SYSTEM ! SAFETY ISSUE !**

### 2.2. Extend your privileges for your user account
Always as **root**, type the command line :
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
Type the password chosen before for your personal user account. You are now connected to your remote computer system as *user_name* !

If you want, you can type a few commands to test the connection and find out which account you are logged in with :
```
whoami
```
And to find out which directory you are in :
```
pwd
```
### 2.3. SSH key authentication
On your local computer system - type *exit* if you are still connected to your remote computer system - type the command line :
```
ssh-keygen -t ed25519
```
Then, press ENTER to validate the directory in which the 2 SSH keys will be generated (keep the default directory).

Choose a passphrase, confirm the passphrase !

Your SSH private key and your SSH public key are now in your local directory */home/user_name/.ssh/* with the names *id_25519* and *id25519.pub* for the SSH public key.

You must now copy your SSH public key on the remote computer system with the command line below :
```
ssh-copy-id user_name@ip_address
```
The SSH public key is copied in the file */home/user_name/.ssh/authorized_keys*

Try now to connect to the remote computer system with your personal user account :
```
ssh user_name@ip_address
```
From now on, the Terminal will ask you to type your passphrase to access the remote computer system !

### 2.4. Disable access via the user account password
Now that we can access the remote computer system with SSH key authentication, we want to block access using the user account password. This means that only the local computer system with the SSH private key will be able to connect to the remote computer system !

To do this, we need to open the SSH configuration file */etc/ssh/sshd_config* and change some parameters :
```
sudo nano /etc/ssh/sshd_config
```
Go to the section :
```
# To disable tunneled clear text passwords, change to no here!
# PasswordAuthentication yes
```
Uncomment *PasswordAuthentication* and change the value to *no* :
```
PasswordAuthentication no
```
Press CTRL+X, then Y, then ENTER to save the file with the new configuration.

Restart the SSH service with the command line :
```
sudo systemctl restart ssh
```
### 2.5. File Transfer with SCP (Secure Copy Protocol) and SFTP (Secure File Transfer Protocol)
On your local computer system, create a new file named *chat.txt* with the command line :
```
echo miaou > chat.txt
```
Then, type :
```
cat chat.txt
```
*miaou* is displayed in the Terminal ^^

We now want to transfer *chat.txt* file to the remote computer system !

**Use SCP with the command line :**
```
scp chat.txt user_name@ip_address:/home/user_name/
```
The *chat.txt* file will be copy in the directory */home/user_name/* of the remote computer.

You can check with the command :
```
ssh user_name@ip_address "ls /home/user_name/"
```
The command *ssh user_name@ip-address ls /home/user_name/* seems to work too.

**Use SFTP with the command line from the directory where the fil you want to transfer is :**
```
sftp user_name@ip_address
```
It opens a channel between the two computers :
```
Connected to ip_address.
sftp> 
```
You can navigate in the remote system with the *cd* command (Change Directory) and copy the *chat.txt* file where you want by using the command :
```
put chat.txt
```
## 3. Problems encountered and found solutions

## 4. Theory

## 5. Conclusion

## 6. Ressources
[Markdown Cheat Sheet](#https://www.markdownguide.org/cheat-sheet/)


