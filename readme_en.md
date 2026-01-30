*This project has been created as part of the 42 curriculum by rjuarez-*

# 📜 Born2beroot
## 📖 Description
### Objective:

	1. Create and configure a virtual machine with Debian or Rocky Linux
	2. Implement advanced security measures: encrypted partitions, firewall, strong password policies, secure sudo configuration
	3. Develop a monitoring script (monitoring.sh) that displays system information every 10 minutes
	4. Document everything in a professional README with technical comparisons

### Overview:

### Linux Distribution Choices:

I chose Debian 13.2.0 for the following reasons:

	1. I already use it at home and am accustomed to it.
	2. It is simpler than Rocky.
	3. There is more documentation.

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

### Execution Order

Pre-checks: Check in the virtual machine configuration under Network/ Advanced/ Port Forwarding that there is a connection with port 4242 as guest port.
text

	1. Check hard disk

	2. Check AppArmor

	3. sudo
		◦ Installation

	4. ssh
		◦ Installation
		◦ Configuration of ssh files
		◦ Check status
		◦ Restart ssh
		◦ Check status
		◦ Verify with ss -tunlp
		◦ Connect from host machine.

	5. ufw
		◦ Installation
		◦ Check status
		◦ Activate
		◦ Check status
		◦ Allow port
		◦ Check status

	6. Groups and users
		◦ Create user
		◦ Create group user42
		◦ Assign users to user42 and sudo groups

	7. Password policies
		◦ Configure strong password for sudo.
		◦ Configure strong password policy.

	8. Script monitoring.sh
		◦ Create script
		◦ Schedule its execution every 10 min.

	9. Change hostname

## 🔄 Resources

## 📚 Documentation
### System
__Distribution Version__

	cat /etc/os-release

__Kernel Version__

	uname -a
	uname -r

__Hostname Change__

	    sudo hostnamectl set-hostname <new system name>
	Edit /etc/hosts to reflect the change:
    	127.0.0.1 your_login42
	Reboot for the name to update.

__Hard Disk__

__Check Hard Disk Partitions:__

	lsblk
### User Management
__Create User:__

	sudo adduser <username>
__Add User to Group:__

	sudo adduser <username> <group name>
__Remove User from Group:__

	sudo gpasswd -d <user> <group_name>
__Change Password:__

	sudo passwd root
	sudo passwd <username>

__Change a User's Primary Group:__

	sudo usermod -g <new_primary_group> <user>

__List Users__

	getent passwd
	cut -d: -f1 /etc/passwd
	getent passwd | awk -F: '$3 >= 1000 && $3 < 65534 {print $1}'

### Group Management

__Create Group:__

	sudo addgroup <group name>

__Delete Group:__

	sudo groupdel <group name>
	sudo groupdel -f <group name>

__Check Group Status:__

	getent group <group name>

__See Existing Groups:__

	nano /etc/group

### System Package Updates

__Check for Updates__

	apt update

__Install Updates__

	apt upgrade -y

### SUDO

__Installation:__

	apt install sudo
### AppArmor
__Installation:__

	But it comes installed by default

	sudo apt install apparmor apparmor-utils
__Check during boot:__

	sudo journalctl -u apparmor
__Check status:__

	sudo systemctl status apparmor
__Verify that it is active and runs at startup:__

	sudo systemctl is-enabled apparmor
	sudo systemctl is-active apparmor
__Check AppArmor Status:__

	sudo systemctl status apparmor
### SSH
__Install OpenSSH Tool:__

	apt install ssh
__Check SSH Service Status:__

	sudo service ssh status

__Restart Service:__

	sudo service ssh restart

__Configuration Files:__

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

__Installation__

	sudo install ufw
__Enable__

	sudo ufw enable
__Disable__

	sudo ufw disable
__Reload Rules__

	sudo ufw reload
__Check Status__

	sudo ufw status
__Allow Ports__

	Single Ports
		sudo ufw allow <port>[ , <port>]
	Port Range
		sudo ufw allow [<port>:<port>]
__Deny Ports__

	sudo ufw deny <port>

### Password

__CONFIGURE STRONG PASSWORD FOR SUDO__

__Create a sudo folder in /var/log/__

	mkdir -p /var/log/sudo
__Create and edit the file and write__

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
__CONFIGURE STRONG PASSWORD POLICY__

__Edit the file /etc/login.defs__

	nano /etc/login.defs

	PASS_MAX_DAYS   30 (password expiration time in days)
	PASS_MIN_DAYS   2  (minimum number of days allowed before changing a password)
	PASS_WARN_AGE   7  (Number of days to warn before password expires)
__Install libpam-pwquality__

	apt install libpam-pwquality
__Edit /etc/pam.d/common-password__

	/etc/security/pwquality.conf

We use - because it must contain at least one character; if we use + we refer to at most those characters.
text

	retry=3         (Number of retries)
	minlen=10       (Minimum number of characters the password must contain)
	ucredit=-1      (Must contain at least one uppercase letter.)
	dcredit=-1      (Must contain at least one digit)
	lcredit=-1      (Must contain at least one lowercase letter.)
	maxrepeat=3     (Cannot have the same character more than 3 times in a row.)
	reject_username (Cannot contain the username.)
	difok=7         (Must have at least 7 characters that are not part of the old password)
	enforce_for_root(Enforce this policy for the root user.)

__Check password policies__

	sudo chage -l <username>

__Apply them to previous users:__

	sudo chage -M 30 <username>
	sudo chage -m 2 <username>
	sudo chage -W 7 <username>

### Monitoring Script

__Creation and editing of the script__

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

	chmod 711 /usr/local/bin/monitoring.sh
__Configure cron__

	sudo crontab -e
Add the line:
	*/10 * * * * /usr/local/bin/monitoring.sh