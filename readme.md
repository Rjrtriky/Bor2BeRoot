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
Aptitude vs apt:
    ◦ apt: 
        ▪ Herramientas modernas y simples (apt install/remove/update).
        ▪ Más rápido y recomendado para uso normal.
    ◦ aptitude: 
        ▪ Gestor más antiguo con interfaz NCURSES interactiva. 
        ▪ Resuelve dependencias de forma más inteligente pero es más lento.
	Diferencias clave: aptitude mantiene registro de paquetes "automáticos", apt no. Aptitude sugiere soluciones a conflictos, apt simplemente falla.

SELinux vs AppArmor:
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

ufw vs firewalld: ambos son firewall (cortafuegos):
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

VirtualBox vs UTM: Ambos son para crear maquinas virtuales, pero en sistemas operativos anfitriones distintos.
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
#### Sistema
    Version de distribution
		cat /etc/os-release
    Version de Kernel
		uname -a
		uname -r
    Cambio de nombre HOST
			sudo hostnamectl set-hostname <nuevo nombre de sistema>
		Edita /etc/hosts para reflejar el cambio:
			127.0.0.1 tu_login42
		reiniciar para que el el nombre actualice
       



## 🔄 Documentacion



