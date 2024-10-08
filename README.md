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

Restart the SSH service with the command lines :
```
sudo systemctl stop ssh && sudo systemctl disable ssh
sudo systemctl enable ssh && sudo systemctl start ssh
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

**Use SFTP with the command line below from the directory where the file you want to transfer is :**
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
### 2.6. Port forwarding
With SSH, you can redirect a localhost port to a port of your remote computer system. This is often used in the case of a web server.

On the remote computer system, we must to install first a web service such as *Nginx* ou *Apache*.
```
# Update the package index on a Debian-based Linux distribution
sudo apt-get update
# Install the web service Nginx
sudo apt-get install nginx
# To know Nginx status
service nginx status
```
The install of a web service opens the port 80 of the remote computer system. You can check with the commands below :
```
# Install network tools
sudo apt-get install net-tools
# List listenning ports
netstat -tuln
# Install nmap
sudo apt-get install nmap
# Scan localhoss ports
nmap localhost
```

Now, on the local computer system, we can use the following command to redirect the chosen port, for example the port 8080 :
```
ssh -L 8080:localhost:80 user_name@ip_address
```
On the local computer system, open a web browser and type *localhost:8080* in the address bar, you should read *Welcome to nginx!*

### 2.7. Tunneling
To increase security, we can change the port used by SSH to avoid brute-force attacks on the remote computer system. For example, we can change to port 2222.
```
# Open the SSH configuration file on the remote computer system
sudo nano /etc/ssh/sshd_config
```
Add the line *Port 2222* above *#Port 22*
```
Port 2222
#Port 22
```
Press CTRL+X, then Y, then ENTER to save the file with the new configuration.

Restart SSH service :
```
sudo systemctl stop ssh && sudo systemctl disable ssh
sudo systemctl enable ssh && sudo systemctl start ssh
```
**In the case of a VPS, you may need to open port 2222 with your hosting provider !**

To connect to the remote computer system, use the new command line :
```
# SSH access via port 2222
ssh -p 2222 user_name@ip_address
```
The previous one (ssh user_name@ip_address) won't work anymore !

For the port forwarding, the command line becomes :
```
ssh -L 8080:localhost:80 -p 2222 user_name@ip_address
```
### 2.8. More protection with UFW (Uncomplicated FireWall)
We can add an extra layer of security with the FireWall UFW :
```
# Enable UFW
sudo ufw enable
# To view permissions
sudo ufw status
# To allow communication on the port 2222 (for SSH)
sudo ufw allow 2222
# We can add ports 80 and 443 for http and https requests
sudo ufw allow 80
sudo ufw allow 443
```
### 2.9. Fail2ban
Fail2ban protects the remote computer system by blocking IP addresses that would force a connection by trying multiple passwords.
```
sudo apt-get update
sudo apt-get install fail2ban
```
Configure Fail2ban for SSH :
```
# Open Fail2ban configuration file
sudo nano /etc/fail2ban/jail.local
# Add rules for jail
[sshd]
enabled = true
port = 2222
# Restart the service for updating the configuration
sudo systemctl restart fail2ban
```
To find out banned IP list :
```
sudo fail2ban-server status sshd
```
To re-authorise an IP address :
```
sudo fail2ban-client set sshd unbanip IP_address
```
### 2.10. Wireshark
Wireshark is a packet analyser. You can run it on your local computer system.
```
# Install Wireshark
sudo apt-get update
sudo apt-get install wireshark
# Run Wireshark
sudo wireshark
```
Select a network interface for analysis, such as eth0 (ethernet), wlan0 (Wifi).
```
# Identify network interfaces
ip link
```
You can filter results at the top of the Wireshark window.
Try *tcp.port==2222* to analyse your SSH connection and communication.
Try to connect on your remote computer system via port 2222 and see how Wireshark reacts.

To analyse what happens on the remote computer, you can install tshark (Wireshark for Terminal) :
```
sudo apt-get update
sudo apt-get install tshark
sudo tshark
```
CTRL+C to stop tshark !
You can filter packets with the options below :
```
sudo tshark -f "port 2222"
sudo tshark -Y "tcp.port==2222"
```
Work in progress ^^'

## 3. Problems encountered and found solutions
### 3.1. Disable access via the user account password
After modifying the sshd configuration file to disable access via the user account password, and while the SSH keys have been generated and copied to the remote computer system, the connection is still possible via a third-party computer !

The problem can be solved with the following commands :
```
sudo systemctl stop ssh && sudo systemctl disable ssh
sudo systemctl enable ssh && sudo systemctl start ssh
```
### 3.2. Port forwarding
By configuring the various layers of security to access the remote machine (SSH, UFW, host firewall), the port forwarding from local port 8080 to remote port 80 may fail.
Run the command again to solve the problem !
```
ssh -L 8080:localhost:80 user_name@ip_address
ssh -L 8080:localhost:80 -p 2222 user_name@ip_address
```
## 4. Theory
### 4.1. Step 1 : Calculation of the Shared Key
- **Generation of Ephemeral Keys :**

**Client :** Generates a pair of ephemeral keys (public and private) for the key exchange.

**Server :** Also generates a pair of ephemeral keys (public and private) for the key exchange.

- **Exchange of Public Keys :**

The client sends its ephemeral public key to the server.

The server sends its ephemeral public key to the client.

- **Calculation of the Shared Key :**

**Client :** Uses its ephemeral private key and the server's ephemeral public key to calculate the shared key.

**Server :** Uses its ephemeral private key and the client's ephemeral public key to calculate the shared key.

The shared key is the same for both parties, but it is never transmitted directly over the network.

### 4.2. Step 2 : Derivation of the Session Key
- **Key Derivation Function (KDF) :** The shared key is used as the basis for deriving the session key. This derivation can include additional steps to enhance security, such as the use of hash functions or key derivation functions (KDF).

- **Derivation Material :** In addition to the shared key, other information may be used to derive the session key, such as nonces (random numbers) and session identifiers.

### 4.3 Step 3 : Authentication with Ed25519
During the connection, the client signs a message with its Ed25519 private key. The server verifies this signature using the stored Ed25519 public key. If the signature is valid, the authentication is successful.

### 4.4. Step 4 : Using the Session Key
- **Symmetric Encryption :** The derived session key is used to encrypt and decrypt the data exchanged between the client and the server. Symmetric encryption algorithms like AES (Advanced Encryption Standard) are used for this task.

- **Data Integrity :** The session key is also used to generate message authentication codes (MAC) that ensure the integrity of the exchanged data.

## 5. Conclusion
The encryption method of SSH connections is essential to ensure the confidentiality, integrity, and authenticity of the exchanged data.

Additional measures such as changing the port and using Fail2Ban contribute to strengthening this protection by reducing the risk of brute force attacks and limiting unauthorized connection attempts.

Wireshark shows that the SSH protocol generates an extremely large number of packets due to its multi-stage encryption method. This increase in the number of exchanges may improve data confidentiality, as it makes data analysis more tedious.

## 6. Ressources
- [Markdown Cheat Sheet](https://www.markdownguide.org/cheat-sheet/)
- [Mistral AI](https://mistral.ai/fr/)
