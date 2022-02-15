# born_2_be_root
This project consists of having you set up your server by following specific rules.


<br> You must choose as an operating system either the latest stable version of Debian (no
testing/unstable), or the latest stable version of CentOS. </br>

<br> You must create at least 2 encrypted partitions using LVM. Below is an example of the
expected partitioning: </br>

![](https://github.com/ilnrzakirov/born_2_be_root/blob/main/%D0%A1%D0%BD%D0%B8%D0%BC%D0%BE%D0%BA%20%D1%8D%D0%BA%D1%80%D0%B0%D0%BD%D0%B0%202022-02-15%20%D0%B2%2012.58.46.png)

<br> A SSH service will be running on port 4242 only. For security reasons, it must not be
possible to connect using SSH as root.</br>

<br>You have to configure your operating system with the UFW firewall and thus leave only
port 4242 open.</br>

For detailed information, refer to the **[subject of this project](https://github.com/ilnrzakirov/born_2_be_root/blob/main/en.subject.pdf)**

## Installation
At the time of writing, the latest stable version of [Debian](https://www.debian.org) is *Debian 10 Buster*. Watch *bonus* installation walkthrough *(no audio)* [here](https://youtu.be/2w-2MX5QrQw).

## *sudo*

### Installing *sudo*
Switch to *root* and its environment via `su -`.
```
$ su -
Password:
#
```
Install *sudo* via `apt install sudo`.
```
# apt install sudo
```
Verify whether *sudo* was successfully installed via `dpkg -l | grep sudo`.
```
# dpkg -l | grep sudo
```

### Install UFW

```bash
apt install ufw -y
```

### Enable Firewall

```bash
ufw enable
```

### Adding User to *sudo* Group
Add user to *sudo* group via `adduser <username> sudo`.
```
# adduser <username> sudo
```
>Alternatively, add user to *sudo* group via `usermod -aG sudo <username>`.
>```
># usermod -aG sudo <username>
>```
Verify whether user was successfully added to *sudo* group via `getent group sudo`.
```
$ getent group sudo
```
`reboot` for changes to take effect, then log in and verify *sudopowers* via `sudo -v`.
```
 
  ## SSH

### Step 1: Installing & Configuring SSH
Install *openssh-server* via `sudo apt install openssh-server`.
```
$ sudo apt install openssh-server
```
Verify whether *openssh-server* was successfully installed via `dpkg -l | grep ssh`.
```
$ dpkg -l | grep ssh
```
Configure SSH via `sudo vi /etc/ssh/sshd_config`.
```
$ sudo vi /etc/ssh/sshd_config
```
To set up SSH using Port 4242, replace below line:
```
13 #Port 22
```
with:
```
13 Port 4242
```
To disable SSH login as *root* irregardless of authentication mechanism, replace below line
```
32 #PermitRootLogin prohibit-password
```
with:
```
32 PermitRootLogin no
```
Check SSH status via `sudo service ssh status`.
```
$ sudo service ssh status
```
>Alternatively, check SSH status via `systemctl status ssh`.
>```
>$ systemctl status ssh
>```

### Step 2: Installing & Configuring UFW
Install *ufw* via `sudo apt install ufw`.
```
$ sudo apt install ufw
```
Verify whether *ufw* was successfully installed via `dpkg -l | grep ufw`.
```
$ dpkg -l | grep ufw
```
Enable Firewall via `sudo ufw enable`.
```
$ sudo ufw enable
```
Allow incoming connections using Port 4242 via `sudo ufw allow 4242`.
```
$ sudo ufw allow 4242
```
Check UFW status via `sudo ufw status`.
```
$ sudo ufw status
```

### Step 3: Connecting to Server via SSH
SSH into your virtual machine using Port 4242 via `ssh <username>@<ip-address> -p 4242`.
```
$ ssh <username>@<ip-address> -p 4242
```
Terminate SSH session at any time via `logout`.
```
$ logout
```
>Alternatively, terminate SSH session via `exit`.
>```
>$ exit
>```

  
## Apparmor

Requirements:

> <...> AppArmor for Debian must be running at startup too.

install apparmor utils and profiles

```bash
apt install apparmor-utils apparmor-profiles -y
```

Check apparmor

```bash
apparmor_status
```
 
 ## Passwords policy

### Password Age policy

Requirements:

> - Password has to expire every 30 days.
> - The minimum number of days allowed before the modification of a password will be set to 2.
> - The user has to receive a warning message 7 days before their password expires.
>

```bash
vim /etc/login.defs
```

Change below lines to the file

```bash
160 PASS_MAX_DAYS   99999
161 PASS_MIN_DAYS   0
162 PASS_WARN_AGE   7
```

to

```bash
160 PASS_MAX_DAYS   30
161 PASS_MIN_DAYS   2
162 PASS_WARN_AGE   7
```

These rules will apply **only** to new users. To change age policy, existing users use:

To change PASS_MAX_DAYS

```bash
chage -M <num_days> <user_name>
```

To change PASS_MIN_DAYS

```bash
chage -m <num_days> <user_name>
```

Check

```bash
chage -l <user_name>
```
  
 ### Script
![](https://github.com/ilnrzakirov/born_2_be_root/blob/main/7383C583-65F5-40B0-8F33-2E30307D809A_1_105_c.jpeg)
 
  
