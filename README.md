*Este proyecto ha sido creado como parte del currículo de 42 por rjuarez-*

# 📜 Born2beroot

## 📖 Descripción

### Objetivo:
    1. Crear, instalar y configurar una máquina virtual con Debian o Rocky Linux. 
    2. Implementar medidas de seguridad avanzadas: particiones cifradas, firewall, políticas de contraseñas fuertes, configuración de sudo segura.
    3. Desarrollar un script de monitoreo (monitoring.sh) que muestre información del sistema cada 10 minutos
    4. Documentar todo en un README profesional con comparativas técnicas.

### Cuestiones
__Rocky vs Debian:__

    ◦ Rocky:
        ▪ Es una distribucion creada a partir de Red Hat Enterprise.
        ▪ Su propisito es Servidor empresarial (estable, seguro y con soporte a largo plazo).
        ▪ Se usa en Entornos de producción.
        ▪ Se usa para servidores( Apache/Nginx), bases de datos (PostgreSQL, MySQL), Infraestructura de nube, Contenedores y Kubernetes y Sistemas legacy que requieren estabilidad extrema.
    ◦ Debian: 
        ▪ .Servidores web y de red (el "sistema operativo de Internet")
        ▪ Sistemas embebidos y Raspberry Pi
        ▪ Escritorios para usuarios avanzados
        ▪ Distribuciones especializadas (Kali para pentesting, Tails para privacidad)
    Por que Debian 13:
    1. Ya lo uso en casa y me he acostumbrado a él.
    2. Esta más enfocado para usuarios.
    3. Es más sencilla la configuracion que Rocky.
    4. La docuentación es mas extensa.

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

### Creacion de la maquina virtual
Crearemos una maquina virtual nueva donde elegiremos:

    -La carpeta donde se almacenará la maquina virtual.
    -La imagen ISO con la distribucion de linux elegida.
    -Omitir la instalacion desatendida.

Con respecto al hardware a emular he optado por lo siguiente:

    -De memoria RAM 4GB (4096MB).
    -2 procesadores.
    -12GB de Disco duro.

Una vez que tenemos la maquina preparada, la arrancamos y comenzará la instalacion de la distribucion que viene el la imagen ISO.

###  Instalacion de la distribución

1. Usaremos la instalacion NO grafica.
2. Seleccionamos el idioma, ubicacion y teclado relacionado con España.
3. En el nombre de maquina o Hostname, ponemos [nombre usuario]42.
4. Dejamos Dominio en blanco.
5. Pide la contraseña de superusuario o root y la verificamos.
6. Pide un usuario nuevo (el usuario de 42) y la contraseña corresppondiente.
7. Seleccionamos la franja horaria de España.
8. El particinamiento del disco duro lo haremos de manera Manual para dejarlo con 1 disco con una particion (boot) y otro disco encriptado, con 3 particiones cifradas (root, home y swap).
9. Pide una contraseña para cifrar.
10. Verificamos la estructura de discos.
11. Configuramos de donde queremos que provengan el gestor de paquetes, omitiendo la configuracion del proxy.
12. No marcamos ninguna de los entornos de escriitorio.
13. Podemos dejar instalado el cargador de arranque Grub y seleccionamos el disco duro virtual.
14. Reiniciamos.

### Orden de ejecucion de la parctica.
Comprobaciones previas: Comprobar en la confiuracion de la maquina virtual en Red/ Avanzado/ Reenvio dde puertos que este una conexion con el puerto 4242 en puerto invitado.

**OJO: Hay que clonar la maquina virtual y trabajar sobre ella.**

1. [Comprobar disco duro](#disco-duro)

2. [Comprobar AppArmor](#apparmor)

3. [sudo](#sudo)

    ◦ Instalacion
4. [ssh](#ssh)

    ◦ Instalacion<br>
    ◦ Configuracion de ficheros para ssh<br>
    ◦ Comprobar estado<br>
    ◦ Reiniciar ssh<br>
    ◦ Comprobar estado<br>
    ◦ Verificar con ss -tunlp<br>
    ◦ Conectar con ordenador anfitrion.

5. [ufw](#ufw)

    ◦ Instalacion<br>
    ◦ Comprobar estado<br>
    ◦ Activar<br>
    ◦ Comprobar estado<br>
    ◦ Habilitar puerto<br>
    ◦ Comprobar estatus

6. [Grupos](#manejo-de-grupos) y [usuarios](#manejo-de-usuarios)
    ◦ Crear usuario<br>
    ◦ Crear grupo user42<br>
    ◦ Asignar usuarios a los grupos user42 y sudo

7. [Politicas de contraseñas](#contraseña)
    ◦ Configurar contraseña fuerte para sudo.<br>
    ◦ Configuración de política de contraseñas fuerte.

8. [Script monitoring.sh](#scrip-de-monitoreo)
    ◦ Crear script<br>
    ◦ Temporizar su ejecucion cada 10 min.

9. [Cambiar nombre del host](#sistema)

##  Recursos

__REFERENCIAS CLASICAS:__

    -Documentación de Linux con man y en https://man7.org/linux/man-pages/man2/read.2.html
    -Apuntes de la UPM.

__USO DE IA:__

    -Consulta sobre errores al crear maquinas virtuales (en ordenador propio).
    -Traduccion de documentacion.
    -Consulta de formato de ficheros readme.md y traduccir al ingles.

## 📚 Documentacion

### Sistema
#### Version de distribution

    cat /etc/os-release
#### Version de Kernel

    uname -a
    uname -r
#### Cambio de nombre HOST

        sudo hostnamectl set-hostname <nuevo nombre de sistema>
    Edita /etc/hosts para reflejar el cambio:
        127.0.0.1 tu_login42
    reiniciar para que el el nombre actualice
### Disco duro

#### Comprobar particiones del disco duro:

    lsblk
### Manejo de Usuarios

#### Crear usuario:

    sudo adduser <nombre usuario>
#### Agredar usuario al grupo:

    sudo adduser <nombre usuario> <nombre grupo>
#### Quitar usuario del grupo:

    sudo gpasswd -d <usuario> <nombre_del_grupo>
#### Cambio de contraseña:

    sudo passwd root
    sudo passwd <nombre usuario>
#### Cambiar grupo principal de un usuario:

    sudo usermod -g <new_grupo_primario> <usuario>
#### Listado de usuarios

    getent passwd
    cut -d: -f1 /etc/passwd
    getent passwd | awk -F: '$3 >= 1000 && $3 < 65534 {print $1}'

### Manejo de Grupos

#### Crear grupo:

    sudo addgroup <nombre grupo>
#### Elimiar Grupo:

    sudo groupdel <nombre grupo>
    sudo groupdel -f <nombre grupo>
#### Comprobar estado del grupo:

    getent group <nombre grupo>
#### Saber que grupos hay:

    nano /etc/group

### Actualizacion de paquetes de sistema

#### Comprobar actualizaciones

    apt update
#### Instalar actualizaciones

    apt upgrade -y

### SUDO

#### Instalación sudo:

    apt install sudo
### AppArmor
Pero viene instalado por defecto
#### Instalacion AppArmor:

    sudo apt install apparmor apparmor-utils
#### Comprobar durante el arranque:

    sudo journalctl -u apparmor
#### Comprobar estado:

    sudo systemctl status apparmor
#### Verificar que está activo y se ejecuta al inicio:

    sudo systemctl is-enabled apparmor
    sudo systemctl is-active apparmor
#### Comprobar estado de AppArmor:

	sudo systemctl status apparmor
### SSH
#### Instalar SSH:

    apt install ssh
#### Comprobar estado del servicio SSH:

    sudo service ssh status
#### Reiniciar servicio:

    sudo service ssh restart
#### Ficheros de configuracion:

    /etc/ssh/sshd_config
        Port 4242
        PermitRootLogin no
    /etc/ssh/ssh_config
        Port 4242
#### Comprobar escucha por puertos:

    ss -tuln | grep “22”
    ss -tuln | grep “4242”
#### Conectar desde anfitrión:

    ssh <nombbre usuario>@127.0.1.1 -p 4242

### UFW

#### Instalacion

    apt install ufw
#### Habilitar

    sudo ufw enable
#### Deshabilitar

    sudo ufw disable
#### Recargar las reglar de nuevo

    sudo ufw reload
#### Comprobar estatus

    sudo ufw status
#### Habilitar puertos

    Puertos sueltos
        sudo ufw allow <puerto>[ , <puerto>]
    Rango de puertos
        sudo ufw allow [<puerto>:<puerto>]
#### Desabilitar puertos

    sudo ufw deny <puerto>

### Contraseña

#### CONFIGURAR CONTRASEÑA FUERTE PARA SUDO

#### Crear una carpeta sudo, en /var/log/

    mkdir -p /var/log/sudo
#### Crear y editar el fichero y escribimos

    nano /etc/sudoers.d/sudo_config
 
    # Número máximo de intentos para ingresar la contraseña
    Defaults  passwd_tries=3
    # Mensaje personalizado cuando se ingresa una contraseña incorrecta
    Defaults  badpass_message="La contraseña es incorrecta."
    # Archivo de registro(log) para eventos de sudo
    Defaults  logfile="/var/log/sudo/sudo.log"
    # Habilitar registro de entrada y salida de comandos
    Defaults  log_input, log_output
    # Directorio para almacenar logs de I/O
    Defaults  iolog_dir="/var/log/sudo"
    # Requiere una terminal para ejecutar comandos con sudo
    Defaults  requiretty
    # Define el PATH seguro para comandos ejecutados con sudo
    Defaults  secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"

#### CONFIGURACIÓN DE POLÍTICA DE CONTRASEÑAS FUERTE

#### Editar el fichero /etc/login.defs

    PASS_MAX_DAYS   30 (tiempo de expiración de la contraseña en dias)
    PASS_MIN_DAYS   2  (número mínimo de días permitido antes de modificar una contraseña)
    PASS_WARN_AGE   7  (Numero de dias para avisar antes de caducar la contraseña)

#### Instalar libpam-pwquality

    apt install libpam-pwquality
#### Editar /etc/pam.d/common-password

    /etc/security/pwquality.conf
Ponemos el - ya que debe contener como mínimo un carácter, si ponemos + nos referimos a como máximo esos caracteres.

    
	#Numero de reintentos
	retry=3
	#La cantidad mínima de caracteres que debe contener la contraseña
	minlen=10
	#Como mínimo debe contener una letra mayúscula.
	ucredit=-1
	#Como mínimo debe contener un dígito
	dcredit=-1 
	#Como mínimo debe contener una letra minúscula.
	lcredit=-1
	#No puede tener más de 3 veces seguidas el mismo carácter.
	maxrepeat=3
	#No puede contener el nombre del usuario.
	reject_username 
	#Debe tener al menos 7 caracteres que no sean parte de la antigua contraseña
	difok=7
	#Implementar esta política para el usuario root.
	enforce_for_root

#### Comprobar  las politicas de pass

    sudo chage -l <nombre usuario>
#### Aplicar las a los usuarios anteriores

    sudo chage -M 30 <nombre usuario>
    sudo chage -m 2 <nombre usuario>
    sudo chage -W 7 <nombre usuario>

### Scrip de Monitoreo
#### Creacion y edicion del script

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

Hay que asegurarse que el fichero tiene permisos de ejecucion

    chmod 777 /usr/local/bin/monitoring.sh

#### Configurar cron

        sudo crontab -e
    añadir la linea
        */10 * * * * /usr/local/bin/monitoring.sh

