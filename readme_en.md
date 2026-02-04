*This project has been created as part of the 42 curriculum by rjuarez-*

# 📜 Born2beroot

## 📖 Description

### Objective:

	1. Create and configure a virtual machine with Debian or Rocky Linux
	2. Implement advanced security measures: encrypted partitions, firewall, strong password policies, secure sudo configuration
	3. Develop a monitoring script (monitoring.sh) that displays system information every 10 minutes
	4. Document everything in a professional README with technical comparisons

### Questions

__Rocky vs. Debian:__

	◦ Rocky:
		▪ It's a distribution built on top of Red Hat Enterprise.
		▪ Its purpose is enterprise servers (stable, secure, and with long-term support).
		▪ It's used in production environments.
		▪ It's used for servers (Apache/Nginx), databases (PostgreSQL, MySQL), cloud infrastructure, containers and Kubernetes, and legacy systems that require extreme stability.
	◦ Debian:
		▪ Web and network servers (the "operating system of the Internet")
		▪ Embedded systems and Raspberry Pi
		▪ Desktops for advanced users
		▪ Specialized distributions (Kali for penetration testing, Tails for privacy)

	Why Debian 13:
	1. I already use it at home and I'm used to it.
	2. It's more focused on end users.
	3. It's easier to set up than Rocky.
	4. The documentation is more extensive.

__APTITUDE vs APT:__

	◦ apt:
		▪ Modern and simple tools (apt install/remove/update).
		▪ Faster and recommended for normal use.
	◦ aptitude:
		▪ Older package manager with an interactive NCURSES interface.
		▪ Resolves dependencies more intelligently but is slower.

	Key Differences: aptitude keeps track of "automatic" packages, apt does not. Aptitude suggests solutions to conflicts, apt simply fails.

__SELINUX vs APPARMOR:__

They are Mandatory Access Control (MAC) security systems for Linux. Enabled by default.

	◦ SELinux:
		▪ Complex policies based on labels/contexts.
		▪ Higher security but harder to configure.
		▪ Used in RHEL, Fedora, CentOS
	◦ AppArmor:
		▪ Profiles based on file paths.
		▪ Simpler to use and debug.
		▪ Used in Debian, Ubuntu, openSUSE

	Key Difference: SELinux labels everything (files, processes), AppArmor uses paths. AppArmor is more user-friendly for desktop, SELinux is more robust for servers.

__UFW vs FIREWALLD:__
They are firewall programs that act as a barrier. Their function is to monitor, filter, and control incoming and outgoing data traffic, allowing only secure connections and blocking unauthorized or malicious access:

	◦ ufw:
		▪ Simple to configure with commands like ufw allow, ufw deny.
		▪ Ideal for simple servers or beginner users.
		▪ Allows configuration of rules by port, service, or IP.
		▪ Can be managed with a single command: ufw enable/disable.
	◦ Firewalld:
		▪ Uses zones to define trust levels (public, home, internal, etc.).
		▪ Allows hot changes (does not interrupt existing connections).
		▪ More flexible and powerful for complex environments.
		▪ Manages rules with firewall-cmd or graphical interfaces.
	Key Difference: While firewalld is used in more complex and powerful environments, ufw is used in simple environments or for beginners. ufw only allows...

__VIRTUALBOX vs UTM:__
Both are for creating virtual machines, but on different host operating systems.

	◦ VirtualBox:
		▪ Free, cross-platform virtualization software (Windows, Linux, macOS).
		▪ Enhanced guest tools.
		▪ Support for advanced network and storage configurations.
		▪ Ideal for general projects and development environments.
	◦ UTM:
		▪ Exclusive application for macOS
		▪ Offers better performance on Apple Silicon hardware (M1/M2).
		▪ Includes QEMU emulation to run non-native systems (x86_64 on ARM).
		▪ Modern graphical interface optimized for macOS.

	Key Difference: VirtualBox is versatile and cross-platform; UTM is the optimal solution for modern macOS, especially on Macs with Apple Silicon where VirtualBox may have compatibility limitations.

## ⚙️ Instructions

### Creating the Virtual Machine
We will create a new virtual machine where we will choose:

	-The folder where the virtual machine will be stored.
	-The ISO image with the chosen Linux distribution.
	-Skip the unattended installation.

Regarding the hardware to emulate, I have chosen the following:

	-4GB of RAM (4096MB).
	-2 processors.
	-12GB of hard drive space.

Once the machine is ready, we boot it and the installation of the distribution contained in the ISO image will begin.

### Distribution Installation

1. We will use the non-graphical installation.
2. Select the language, location, and keyboard layout for Spain.
3. In the hostname field, enter [username]42.
4. Leave the domain field blank.
5. Enter the superuser or root password.
6. Enter a new user (username 42) and the corresponding password.
7. Select the Spanish time zone.
8. Manually partition the hard drive, creating one disk with a single partition (boot) and another encrypted disk with three encrypted partitions (root, home, and swap).
9. Enter a password for encryption.
10. Verify the disk structure.
11. Configure the package manager source, omitting the proxy configuration. 12. We left all desktop environments unchecked.
13. We can leave the Grub bootloader installed and select the virtual hard drive.
14. We restarted.

### Practice Execution Order

Preliminary Checks: Verify in the virtual machine configuration under Network/Advanced/Port Forwarding that a connection is established using port 4242 as the guest port.

1. [Check hard disk](#hard-disk)

2. [Check AppArmor](#apparmor)

3. [sudo](#sudo)

	◦ Installation
4. [ssh](#ssh)

	◦ Installation<br>
	◦ SSH configuration files<br>
	◦ Check status<br>
	◦ Restart SSH<br>
	◦ Check status<br>
	◦ Verify with ss -tunlp<br>
	◦ Connect to host computer.<br>

5. [ufw](#ufw)

	◦ Installation<br>
	◦ Check status<br>
	◦ Activate<br>
	◦ Check status<br>
	◦ Enable port<br>
	◦ Check status<br>

6. [Groups](#group-management) and [users](#user-management)

	◦ Create user<br>
	◦ Create group user42<br>
	◦ Assign users to groups user42 and sudo<br>

7. [Password Policies](#password)

	◦ Configure strong password for sudo.<br>
	◦ Configure strong password policy.

8. [monitoring.sh Script](#monitoring-script)

	◦ Create script<br>
	◦ Schedule its execution every 10 minutes.

9. [Change Hostname](#system)

## Resources

__CLASSIC REFERENCES:__

* Linux documentation using man and at https://man7.org/linux/man-pages/man2/read.2.html
* https://programminghistorian.org/es/lecciones/introduccion-a-markdown
* UPM notes.

__USE OF AI:__

* Troubleshooting errors when creating virtual machines (on my own computer).
* Translating documentation.
* Troubleshooting the readme.md file format and translating it into English.

## 📚 Documentation
### System
#### Distribution Version

	cat /etc/os-release

#### Kernel Version

	uname -a
	uname -r

#### Hostname Change

	    sudo hostnamectl set-hostname <new system name>
	Edit /etc/hosts to reflect the change:
    	127.0.0.1 your_login42
	Reboot for the name to update.

### Hard Disk

#### Check Hard Disk Partitions:

	lsblk
### User Management
#### Create User:

	sudo adduser <username>
#### Add User to Group:

	sudo adduser <username> <group name>
#### Remove User from Group:

	sudo gpasswd -d <user> <group_name>
#### Change Password:

	sudo passwd root
	sudo passwd <username>

#### Change a User's Primary Group:

	sudo usermod -g <new_primary_group> <user>

#### List Users

	getent passwd
	cut -d: -f1 /etc/passwd
	getent passwd | awk -F: '$3 >= 1000 && $3 < 65534 {print $1}'

### Group Management

#### Create Group:

	sudo addgroup <group name>

#### Delete Group:

	sudo groupdel <group name>
	sudo groupdel -f <group name>

#### Check Group Status:

	getent group <group name>

#### See Existing Groups:

	nano /etc/group

### System Package Updates

#### Check for Updates

	apt update

#### Install Updates

	apt upgrade

### SUDO

#### Installation:

	apt install sudo
### AppArmor
#### Installation:

But it comes installed by default

	apt install apparmor apparmor-utils
#### Check during boot:

	sudo journalctl -u apparmor
#### Check status:

	sudo systemctl status apparmor
#### Verify that it is active and runs at startup:

	sudo systemctl is-enabled apparmor
	sudo systemctl is-active apparmor
#### Check AppArmor Status:

	sudo systemctl status apparmor
### SSH
#### Install OpenSSH Tool:

	apt install ssh
#### Check SSH Service Status:

	sudo service ssh status

#### Restart Service:

	sudo service ssh restart

#### Configuration Files:

	/etc/ssh/sshd_config
		Port 4242
		PermitRootLogin no
	/etc/ssh/ssh_config
		Port 4242

Check Listening Ports:

ss -tuln | grep “22”
ss -tuln | grep “4242”

Connect by Host PC:

	ssh <username>@localhost -p 4242
	ssh <username>@127.0.0.1 -p 4242

### UFW

#### Installation

	apt install ufw
#### Enable

	sudo ufw enable
#### Disable

	sudo ufw disable
#### Reload Rules

	sudo ufw reload
#### Check Status

	sudo ufw status
#### Allow Ports

	Single Ports
		sudo ufw allow <port>[ , <port>]
	Port Range
		sudo ufw allow [<port>:<port>]
#### Deny Ports

	sudo ufw deny <port>

### Password

#### CONFIGURE STRONG PASSWORD FOR SUDO

##### Create a sudo folder in /var/log/

	mkdir -p /var/log/sudo
##### Create and edit the file and write

	nano /etc/sudoers.d/sudo_config

	# Maximum number of attempts to enter password
	Defaults  passwd_tries=3
	# Custom message when entering an incorrect password
	Defaults  badpass_message="The password is incorrect."
	# Log file for sudo events
	Defaults  logfile="/var/log/sudo/sudo.log "
	# Enable logging of command input and output
	Defaults  log_input, log_output
	# Directory to store I/O logs
	Defaults  iolog_dir="/var/log/sudo"
	# Require a terminal to run commands with sudo
	Defaults  requiretty
	# Define the secure PATH for commands executed with sudo
	Defaults  secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"
#### CONFIGURE STRONG PASSWORD POLICY

##### Edit the file /etc/login.defs

	nano /etc/login.defs

	#password expiration time in days
	PASS_MAX_DAYS 30
	#minimum number of days allowed before changing a password
	PASS_MIN_DAYS 2
	#Number of days to warn before password expires
	PASS_WARN_AGE 7
##### Install libpam-pwquality

	apt install libpam-pwquality
##### Edit /etc/pam.d/common-password

	/etc/security/pwquality.conf

We use - because it must contain at least one character; if we use + we refer to at most those characters.
text

	#Number of retries
	retry=3
	#Minimum number of characters the password must contain
	minlen=10
	#Must contain at least one uppercase letter.
	ucredit=-1
	#Must contain at least one digit
	dcredit=-1
	#Must contain at least one lowercase letter.
	lcredit=-1
	#Cannot have the same character more than 3 times in a row.
	maxrepeat=3
	#Cannot contain the username.
	reject_username
	#Must have at least 7 characters that are not part of the old password
	difok=7
	#Enforce this policy for the root user.
	enforce_for_root

##### Check password policies

	sudo chage -l <username>

##### Apply them to previous users:

	sudo chage -M 30 <username>
	sudo chage -m 2 <username>
	sudo chage -W 7 <username>

### Monitoring Script

#### Creation and editing of the script

	nano /usr/local/bin/monitoring.sh

	#!/bin/bash
	# 1. Arquitectura del sistema y versión del kernel
	ARCH=$(uname -a)
	# 2. Número de núcleos físicos
	PHYS_CPU=$(grep "physical id" /proc/cpuinfo | sort -u | wc -l)
	# 3. Número de núcleos virtuales (threads)
	VIRT_CPU=$(nproc)
	# 4. RAM usada y porcentaje
	MEM_USED=$(free -m | awk 'NR==2{print $3}')
	MEM_TOTAL=$(free -m | awk 'NR==2{print $2}')
	MEM_PERC=$(awk "BEGIN {printf \"%.2f\", ($MEM_USED/$MEM_TOTAL)*100}")
	# 5. Disco disponible y porcentaje
	DISK_SDA1_USED=$(df -h --output=source,used,size,pcent | grep "sda1" | awk '{print $2}')
	DISK_SDA1_SIZE=$(df -h --output=source,used,size,pcent | grep "sda1" | awk '{print $3}')
	DISK_SDA1_PERC=$(df -h --output=source,used,size,pcent | grep "sda1" | awk '{print $4}')

	DISK_ROOT_USED=$(df -h --output=source,used,size,pcent | grep "root" | awk '{print $2}')
	DISK_ROOT_SIZE=$(df -h --output=source,used,size,pcent | grep "root" | awk '{print $3}')
	DISK_ROOT_PERC=$(df -h --output=source,used,size,pcent | grep "root" | awk '{print $4}')

	DISK_HOME_USED=$(df -h --output=source,used,size,pcent | grep "home" | awk '{print $2}')
	DISK_HOME_SIZE=$(df -h --output=source,used,size,pcent | grep "home" | awk '{print $3}')
	DISK_HOME_PERC=$(df -h --output=source,used,size,pcent | grep "home" | awk '{print $4}')
	# 6. Porcentaje de uso de CPU
	CPU_LOAD=$(top -bn1 | grep "Cpu(s)" | awk '{print $2 + $4}')
	# 7. Último reinicio
	LAST_BOOT=$(who -b | awk '{print $3 " " $4}')
	# 8. LVM activo o no
	LVM_ACTIVE=$(lsblk | grep -q "lvm" && echo "yes" || echo "no")
	# 9. Número de conexiones activas
	TCP_CONN=$(ss -tun | grep ESTAB | wc -l)
	# 10. Número de usuarios conectados
	USER_LOG=$(users | wc -w)
	# 11. Dirección IPv4 y MAC
	IPV4=$(hostname -I | awk '{print $1}')
	MAC=$(ip link | grep ether | awk '{print $2}')
	# 12. Número de comandos ejecutados con sudo
	SUDO_CMDS=$(journalctl _COMM=sudo | grep COMMAND | wc -l)

	# Mostrar todo con wall
	wall "
	#Architecture: $ARCH
	#CPU physical: $PHYS_CPU
	#vCPU: $VIRT_CPU
	#Memory Usage: $MEM_USED/$MEM_TOTAL MB ($MEM_PERC%)
	#DISK:     sda1: $DISK_SDA1_USED/$DISK_SDA1_SIZE ($DISK_SDA1_PERC)
	#          root: $DISK_ROOT_USED/$DISK_ROOT_SIZE ($DISK_ROOT_PERC)
	#          home: $DISK_HOME_USED/$DISK_HOME_SIZE ($DISK_HOME_PERC)
	#CPU load: $CPU_LOAD
	#Last boot: $LAST_BOOT
	#LVM use: $LVM_ACTIVE
	#TCP Connections: $TCP_CONN ESTABLISHED
	#User log: $USER_LOG
	#Network: IP $IPV4 ($MAC)
	#Sudo: $SUDO_CMDS cmd
	"

You need to make sure the file has execute permissions.

	chmod 777 /usr/local/bin/monitoring.sh
#### Configure cron

		sudo crontab -e
	Add the line:
		*/10 * * * * /usr/local/bin/monitoring.sh