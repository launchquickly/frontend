# Set-up development environment

**Objective**: Set-up and install the necessary software to provide a Vagrant based development environment for GOV.UK Frontend

Vagrant based development environment for building frontends. This will consist of 2 VMS:

- **fe-code** - desktop environment for running IDE etc
- **fe-server** - server environment to host and run code within

with SSH communication configured and the necessary software installed.

Please read [VS Code Remote Development](https://code.visualstudio.com/docs/remote/remote-overview), for more information on this sort of set-up, if interested.

Vagrant VMs are being used for both desktop and server configurations to make set-up and configuration as repeatable and automated as possible.

## VM configuration details

### fe-code

- Ubuntu 20.04 desktop 
    - UK keyboard and formats set-up
    - Timezone set to UTC
- VS Code 
    - remote SSH extension
- Configuration
    - SSH keys and config to connect to fe-server (frontend.scot.local)

### fe-server

- Ubuntu 20.04 server
    - UK keyboard and formats set-up
    - Timezone set to UTC
- Git 2.x
- unzip utility
- master branch of github.com/launchquickly/frontend repository cloned to /home/vagrant/frontend
    - configured for ssh access

## Pre-requisites

- [Virtualbox](https://www.virtualbox.org/wiki/Downloads) 6.1.x or above is installed on host machine
- [Vagrant](https://www.vagrantup.com/downloads) 2.2.7 or above is installed on host machine

## Steps to set-up

### On host machine

1. [Download](https://github.com/launchquickly/frontend/archive/master.zip) zip of repository
2. Unzip repository to directory you wish to use
3. Open command prompt and change into frontend/dev directory of repository
4. Create, configure, and start both VMs from command prompt:
```
C:\Users\AUser\dev\frontend\dev> vagrant up
```
5. Login to fe-code desktop as "vagrant" user using "vagrant" password

### On fe-code

6. Start VS Code
7. Within VS Code select the "Remote Explorer" view via the icons at the top left
8. Within the SSH TARGETS view right-click on the "frontend.scot.local" target and select "Connect to Host in Current Window". This will establish the remote connection with fe-server VM and install the extensions VS Code needs.
9. Select directory you wish to work from on fe-server. /home/vagrant or /home/vagrant/frontend are both reasonable starting points.

### fe-server - these steps can all be executed from fe-code via VS Code remote terminal session (Ctrl + Shift + ` shortcut to open) 

 10. (Optional) Configure git set-up  
     10.1. Set global gitconfig options, otherwise vagrant name and email will be used:

```console
vagrant@fe-server:~$ git config --global user.name "John Doe"
vagrant@fe-server:~$ git config --global user.email johndoe@example.com
```

 11. (Optional) Generate and upload ssh keys for git operations with github.com or similar: 

     11.1. Generate public/private keys. Note if you set a passphrase (recommended) you will have to use commandline to push changes to github.com, etc as VS code does not yet handle passphrases for remote repositories.

```console
vagrant@fe-server:~$ ssh-keygen -t rsa -b 4096 -C "johndoe@example.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/home/vagrant/.ssh/id_rsa):
Enter passphrase (empty no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/vagrant/.ssh/id_rsa
Your public key has been saved in /home/vagrant/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:4Jdh+Px8Dm8UNji0qOEDuvKus3yhQJcnSLFICyKu5Ew johndoe@example.com
The key's randomart image is:
+---[RSA 4096]----+
|+o.              |
|B.o    .  .      |
|oE. . o oo o     |
|*o +.o.=.o+ +    |
|oo..oo.oS  o o   |
|. ..  +. o  .    |
|. ...  .  +..    |
|+...       =.    |
|oO+        .o    |
+----[SHA256]-----+
vagrant@fe-server:~$
```

 11.2. Copy contents of public key to Github account, or similar.

```console
vagrant@fe-server:~$ cat ~/.ssh/id_rsa.pub
```

 11.3. Add any necessary host information to enable git ssh access to ~/.ssh/config. e.g.:

```console
vagrant@fe-server:~$ vi ~/.ssh/config

Host github.com
  User johndoe
  Hostname ssh.github.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/id_rsa
  Port 443

```
