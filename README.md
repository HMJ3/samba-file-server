# Samba File Server Setup

**Author:** Henrik Einola  
**Course:** ICI003AS2A-3007  
**Date:** March 23, 2025  
**Institution:** Haaga-Helia University of Applied Sciences  

## Video Demonstration (in Finnish)
[Watch Demonstration](https://www.youtube.com/watch?v=tWc8vdpudu0)

## Project Document

A Samba file server enables file sharing across different operating systems over a network. It lets you access your desktop files from a laptop and share files with Windows and macOS users.

## Verify requirements for installation

### Requirements

- Ubuntu version 16.04 LTS  
- Local Lan Network  

To verify we use commands:

```bash
lsb_release -a
```

Result:  
“Release Ubuntu 24.04.2. LTS”

Network: I will use my personal wifi-network

## Install and Configure Ubuntu Server

- Do a normal install  
- Change network settings: NAT > Bridged Adapter (server gets it’s own ip on network)  
- Verify ip address with cmd:
  
```bash
ip a
```

- Send ping from host > server  

## Install Samba

```bash
sudo apt update         # update system
sudo apt install samba  # install samba
whereis samba           # verify location of system
```

Now we have a live ubuntu server running with samba installed. We have verified that samba can be found on the system and that our network connection between host and server is working.

## Configure Samba

Steps:

```bash
mkdir /home/henrik/sambashare
sudo nano /etc/samba/smb.conf
```

Add the following at the bottom of the file:

```ini
[sambashare]
   comment = Samba on Ubuntu
   path = /home/username/sambashare
   read only = no
   browsable = yes
   valid users = henrik
```

![config](img/config.png)

## What have we added?

- **path:** The directory of our share  
- **read only:** Permission to modify the contents of the share folder is only granted when the value of this directive is no.  
- **browsable:** When set to yes, file managers such as Ubuntu’s default file manager will list this share under “Network” (it could also appear as browsable).  
- **guest ok:** Define if a guest user can access the server  
- **valid user:** Users who have access to the file server  

## Restart & Update Firewall Rules

By default UFW (Uncomplicated Firewall) on Ubuntu is disabled, I however want it enabled for security reasons.

```bash
sudo ufw enable            # enable firewall
sudo ufw status            # verify status
sudo service smbd restart  # restart samba, refresh config
sudo ufw allow samba       # open ports required for samba
```

## User Accounts & Connecting to Share

Since Samba does not use the system account password, we need to set up a Samba password for our user account.

```bash
sudo smbpasswd -a username
```

This has to be a username that is registered on the system, otherwise it will not save.

![user](img/user.png)

## Testing

We will conduct a test by connecting to our server from a Mac computer, and creating a test folder called “TestiKansio”.

![server](img/server.png)

## Let’s connect!

#### Step 1 - Open Finder  

#### Step 2 – Select “Go”  

#### Step 3 – Select “Connect to Server”  

![connect](img/connect.png)

#### Step 4 – Enter ip address and directory name  

![info](img/info.png)

#### Step 5 – Insert credentials and connect  

![credentials](img/credentials.png)

#### Step 6 – Create a folder in your file directory and verify server side upload  

![folder](img/folder.png)

## Result

Successful installation! Key point was that I verified early on that my network connection is working.

![result](img/result.png)

## Resource

https://ubuntu.com/tutorials/install-and-configure-samba#1-overview  
