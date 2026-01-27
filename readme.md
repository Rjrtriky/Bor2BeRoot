*Este proyecto ha sido creado como parte del currículo de 42 por rjuarez-*

# 📜 Born2beroot

## 📖 Descripción

### Objetivo:
    1. Crear y configurar una máquina virtual con Debian o Rocky Linux
    2. Implementar medidas de seguridad avanzadas: particiones cifradas, firewall, políticas de contraseñas fuertes, configuración de sudo segura
    3. Desarrollar un script de monitoreo (monitoring.sh) que muestre información del sistema cada 10 minutos
    4. Documentar todo en un README profesional con comparativas técnicas

### Vision general:

### Elecciones de distribución de Linux:

He elegido Debian 13.2.0 por los siguientes motivos:

    1. Ya lo uso en casa y me he acostumbrado a él.
    2. Es mas sencillo que Rocky.
    3. Hay mas docuentación.

### Cuestiones

__APTITUDE vs APT:__

    ◦ apt: 
        ▪ Herramientas modernas y simples (apt install/remove/update).
        ▪ Más rápido y recomendado para uso normal.
    ◦ aptitude: 
        ▪ Gestor más antiguo con interfaz NCURSES interactiva. 
        ▪ Resuelve dependencias de forma más inteligente pero es más lento.
	
    Diferencias clave: aptitude mantiene registro de paquetes "automáticos", apt no. Aptitude sugiere soluciones a conflictos, apt simplemente falla.

__SELINUX vs APPARMOR:__

Son sistemas de seguridad obligatoria (MAC) para Linux. Viene activo por defecto.

    ◦ SELinux: 
		▪ Políticas complejas basadas en etiquetas/contextos. 
		▪ Mayor seguridad pero más difícil de configurar. 
		▪ Usado en RHEL, Fedora, CentOS
	◦ AppArmor: 
		▪ Perfiles basados en rutas de archivos. 
		▪ Más simple de usar y depurar. 
		▪ Usado en Debian, Ubuntu, openSUSE

    Diferencia clave: SELinux etiqueta todo (archivos, procesos), AppArmor usa rutas. AppArmor es más amigable para escritorio, SELinux más robusto para servidores.

__UFW vs FIREWALLD:__
Son firewall (cortafuegos), en estos casos programas que actúa como barrera. Su función es monitorear, filtrar y controlar el tráfico de datos entrante y saliente, permitiendo solo las conexiones seguras y bloqueando accesos no autorizados o maliciosos:

	◦ ufw: 
		▪ Sencillo de configurar con comandos como ufw allow, ufw deny.
		▪ Ideal para servidores simples o usuarios principiantes.
		▪ Permite configurar reglas por puerto, servicio o IP.
		▪ Se puede gestionar con un solo comando: ufw enable/disable.
	◦ Firewalld:
		▪ Usa zonas para definir niveles de confianza (public, home, internal, etc.).
		▪ Permite cambios en caliente (no interrumpe conexiones existentes).
		▪ Más flexible y potente para entornos complejos.
		▪ Gestiona reglas con firewall-cmd o interfaces gráficas.
	Diferencia clave: Mientras que firewalld se usa en entornos mas complejos y potentes, ufw se unsa en entornos simples o para principiantes. ufw permite solo por

__VIRTUALBOX vs UTM:__
Ambos son para crear maquinas virtuales, pero en sistemas operativos anfitriones distintos.

	◦ VirtualBox: 
		▪ Software de virtualización gratuito y multiplataforma (Windows, Linux, macOS).
		▪ Herramientas de invitado mejoradas.
		▪ Soporte para configuraciones avanzadas de red y almacenamiento. 
		▪ Ideal para proyectos generales y entornos de desarrollo.
	◦ UTM: 
		▪ Aplicación exclusiva para macOS
		▪ Ofrece mejor rendimiento en hardware Apple Silicon (M1/M2).
		▪ Incluye emulación QEMU para ejecutar sistemas no-nativos (x86_64 en ARM).
		▪ Interfaz gráfica moderna y optimizada para macOS.

	Diferencia clave: VirtualBox es versátil y multiplataforma; UTM es la solución óptima para macOS modernos, especialmente en Mac con Apple Silicon donde VirtualBox puede tener limitaciones de compatibilidad.


## ⚙️ Instrucciones

### Orden de ejecucion
Comprobaciones previas: Comprobar en la confiuracion de la maquina virtual en Red/ Avanzado/ Reenvio dde puertos que este una conexion con el puerto 4242 en puerto invitado.

    1. Comprobar disco duro
    
    2. Comprobar AppArmor
    
    3. sudo
        ◦ Instalacion
    
    4. ssh
        ◦ Instalacion
        ◦ Configuracion de ficheros para ssh
        ◦ Comprobar estado
        ◦ Reiniciar ssh
        ◦ Comprobar estado
        ◦ Verificar con ss -tunlp
        ◦ Conectar con ordenador anfitrion.
    
    5. ufw
        ◦ Instalacion
        ◦ Comprobar estado
        ◦ Activar
        ◦ Comprobar estado
        ◦ Habilitar puerto
        ◦ Comprobar estatus
    
    6. Grupos y usuarios
        ◦ Crear usuario
        ◦ Crear grupo user42
        ◦ Asignar usuarios a los grupos user42 y sudo
    
    7. Politicas de contraseñas
        ◦ Configurar contraseña fuerte para sudo.
        ◦ Configuración de política de contraseñas fuerte.
    
    8. Script monitoring.sh
        ◦ Crear script
        ◦ Temporizar su ejecucion cada 10 min.
    
    9. Cambiar nombre del host

## 📚 Recursos

### Documentacion


## 🔄 Documentacion

### Sistema
__Version de distribution__

    cat /etc/os-release
__Version de Kernel__

    uname -a
    uname -r
__Cambio de nombre HOST__

        sudo hostnamectl set-hostname <nuevo nombre de sistema>
    Edita /etc/hosts para reflejar el cambio:
        127.0.0.1 tu_login42
    reiniciar para que el el nombre actualice
### Disco duro

__Comprobar particiones del disco duro:__

    lsblk
### Manejo de Usuarios

__Crear usuario:__

    sudo adduser <nombre usuario>
__Agredar usuario al grupo:__

    sudo adduser <nombre usuario> <nombre grupo>
__Quitar usuario del grupo:__

    sudo gpasswd -d <usuario> <nombre_del_grupo>
__Cambio de contraseña:__

    sudo passwd root
    sudo passwd <nombre usuario>
__Cambiar grupo principal de un usuario:__

    sudo usermod -g <new_grupo_primario> <usuario>
__Listado de usuarios__

    getent passwd
    cut -d: -f1 /etc/passwd
    getent passwd | awk -F: '$3 >= 1000 && $3 < 65534 {print $1}'

### Manejo de Grupos

__Crear grupo:__

    sudo addgroup <nombre grupo>
__Elimiar Grupo:__

    sudo groupdel <nombre grupo>
    sudo groupdel -f <nombre grupo>
__Comprobar estado del grupo:__

    getent group <nombre grupo>
__Saber que grupos hay:__

    nano /etc/group

### Actualizacion de paquetes de sistema

__Comprobar actualizaciones__

    apt update
__Instalar actualizaciones__

    apt upgrade -y

### SUDO

__Instalación:__

    apt install sudo
### AppArmor
__Comprobar estado de AppArmor:__

	sudo systemctl status apparmor
### SSH
__Instalar herramienta OpenSSH:__

    apt install ssh
__Comprobar estado del servicio SSH:__

    sudo service ssh status
__Reiniciar servicio:__

    sudo service ssh restart
__Ficheros de configuracion:__

    /etc/ssh/sshd_config
        Port 4242
        PermitRootLogin no
    /etc/ssh/ssh_config
        Port 4242
__Comprobar escucha por puertos:__

    ss -tuln | grep “22”
    ss -tuln | grep “4242”
__Conectar a PC anfitrión:__

    ssh <nombbre usuario>@localhost -p 4242

### UFW

__Instalacion__

    sudo install ufw
__Habilitar__

    sudo ufw enable
__Deshabilitar__

    sudo ufw disable
__Recargar las reglar de nuevo__

    sudo ufw reload
__Comprobar estatus__

    sudo ufw status
__Habilitar puertos__

    Puertos sueltos
        sudo ufw allow <puerto>[ , <puerto>]
    Rango de puertos
        sudo ufw allow [<puerto>:<puerto>]
__Desabilitar puertos__

    sudo ufw deny <puerto>

### Contraseña

__CONFIGURAR CONTRASEÑA FUERTE PARA SUDO__

__Crear una carpeta sudo, en /var/log/­__

    mkdir -p /var/log/sudo
__Crear y editar el fichero y escribimos__

    nano /etc/sudoers.d/sudo_config
 
    # Número máximo de intentos para ingresar la contraseña
    Defaults  passwd_tries=3
    # Mensaje personalizado cuando se ingresa una contraseña incorrecta
    Defaults  badpass_message="La contraseña es incorrecta."
    # Archivo de registro(log) para eventos de sudo
    Defaults  logfile="/var/log/sudo/sudo.log "
    # Habilitar registro de entrada y salida de comandos
    Defaults  log_input, log_output
    # Directorio para almacenar logs de I/O
    Defaults  iolog_dir="/var/log/sudo"
    # Requiere una terminal para ejecutar comandos con sudo
    Defaults  requiretty
    # Define el PATH seguro para comandos ejecutados con sudo
    Defaults  secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"

__CONFIGURACIÓN DE POLÍTICA DE CONTRASEÑAS FUERTE__

__Editar el fichero /etc/login.defs__

    PASS_MAX_DAYS   30 (tiempo de expiración de la contraseña en dias)
    PASS_MIN_DAYS   2  (número mínimo de días permitido antes de modificar una contraseña)
    PASS_WARN_AGE   7  (Numero de dias para avisar antes de caducar la contraseña)

__Instalar libpam-pwquality__

    apt install libpam-pwquality
__Editar /etc/pam.d/common-password__

    /etc/security/pwquality.conf
Ponemos el - ya que debe contener como mínimo un carácter, si ponemos + nos referimos a como máximo esos caracteres.

    
    retry=3         (Numero de reintentos)
    minlen=10       (La cantidad mínima de caracteres que debe contener la contraseña)
    ucredit=-1      (Como mínimo debe contener una letra mayúscula.)
    dcredit=-1      (Como mínimo debe contener un dígito)
    lcredit=-1      (Como mínimo debe contener una letra minúscula.)
    maxrepeat=3     (No puede tener más de 3 veces seguidas el mismo carácter.)
    reject_username (No puede contener el nombre del usuario.)
    difok=7         (Debe tener al menos 7 caracteres que no sean parte de la antigua contraseña)
    enforce_for_root(Implementar esta política para el usuario root.)

__Comprobar  las politicas de pass__

    sudo chage -l <nombre usuario>
__Aplicar las a los usuarios anteriores__

    sudo chage -M 30 <nombre usuario>
    sudo chage -m 2 <nombre usuario>
    sudo chage -W 7 <nombre usuario>

### Scrip de Monitoreo
__Creacion y edicion del script__

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
    DISK_SDA1_USED=$(df -h –output=source,used,size,pcent | grep “sda1” | awk ‘{print $2}’)
    DISK_SDA1_SIZE=$(df -h –output=source,used,size,pcent | grep “sda1” | awk ‘{print $3}’)
    DISK_SDA1_PERC=$(df -h –output=source,used,size,pcent | grep “sda1” | awk ‘{print $4}’)
    
    DISK_ROOT_USED=$(df -h –output=source,used,size,pcent | grep “root” | awk ‘{print $2}’)
    DISK_ROOT_SIZE=$(df -h –output=source,used,size,pcent | grep “root” | awk ‘{print $3}’)
    DISK_ROOT_PERC=$(df -h –output=source,used,size,pcent | grep “root” | awk ‘{print $4}’)
    
    DISK_HOME_USED=$(df -h –output=source,used,size,pcent | grep “home” | awk ‘{print $2}’)
    DISK_HOME_SIZE=$(df -h –output=source,used,size,pcent | grep “home” | awk ‘{print $3}’)
    DISK_HOME_PERC=$(df -h –output=source,used,size,pcent | grep “home” | awk ‘{print $4}’)
    # 6. Porcentaje de uso de CPU
    CPU_LOAD=$(top -bn1 | grep "load average" | awk '{print $(NF-2)}')
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
    #DISK:     sda1: $DISK_SDA1_USED/DISK_SDA1_SIZE ($DISK_SDA1_PERC)
    #          root: $DISK_ROOT_USED/DISK_ROOT_SIZE ($DISK_ROOT_PERC)
    #          home: $DISK_HOME_USED/DISK_HOME_SIZE ($DISK_HOME_PERC)
    #CPU load: $CPU_LOAD
    #Last boot: $LAST_BOOT
    #LVM use: $LVM_ACTIVE
    #TCP Connections: $TCP_CONN ESTABLISHED
    #User log: $USER_LOG
    #Network: IP $IPV4 ($MAC)
    #Sudo: $SUDO_CMDS cmd
    “

__Configurar cron__

        sudo crontab -e
    añadir la linea
        */10 * * * * /usr/local/bin/monitoring.sh

