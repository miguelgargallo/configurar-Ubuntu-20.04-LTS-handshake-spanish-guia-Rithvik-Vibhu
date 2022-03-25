# configurar-Ubuntu-20.04-LTS
Cómo configurar tu VPS con Ubuntu 20.04 LTS

Definimos "sudo" como: sudo , que es un acrónimo de superuser do o substitute user do, es un comando que ejecuta un prompt elevado sin necesidad de cambiar su identidad. (Son, 2021)
Definimos "nano" delante o no delante de "sudo" porque "nano" es: GNU nano es un sencillo editor de texto basado en un terminal. (Ubuntu documentation wiki, 2017)
Definimos "cd" delante de directorios: El comando cd ("change directory") se utiliza para cambiar el directorio de trabajo actual en Linux y otros sistemas operativos tipo Unix. (Linuxize, 2021)

Con las flechas del teclado: arriba, abajo, nos podemos mover siempre entre comando cuando estemos en la consola escribiendo comandos.
Usaremos dentro del servidor, en Ubuntu, el editor de texto nano.

Vamos a nuestro VPS contratado
Instalamos Ubuntu 20.04 LTS, que es la versión mas estable con la que programar y configurar nuestro servidor.
Este servidor cuenta con unas prestaciones de 8 gb de Ram y 4 cores en 200gb de memoria SSD.



¿Cómo acceder a nuestro VPS Ubuntu 20.04 LTS?



Comenzamos la configuración de Ubuntu 20.04 LTS y nos encontramos con el acceso ssh, al ser la edición servidor, tendremos a nuestra disposición una consola de comandos.
Cojemos Git Bash en nuestro PC y comenzamos a escribir el acceso a Admin.
ssh admin@123.456.789.10

The authenticity of host '123.456.789.10 (123.456.789.10)' can't be established.
ED25519 key fingerprint is SHA256:KvTPDpjh6cPhCs7LAGvxwd4Awyffuwqrxiw8roeIMTY.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])?

yes

Warning: Permanently added '123.456.789.10' (XXXXXXX) to the list of known hosts.
admin@123.456.789.10's password:

nuestracontroseña
Entramos en el servidor y nos pone toda la información.
limpiamos la pantalla con "clean"
clean



¿Cómo cambiar las contraseñas a nuestros usuarios, root y admin desde el usuario admin de VPS Ubuntu 20.04 LTS?



Cambiamos la contraseña de root y admin por dos contraseñas totalmente seguras.
admin@nombredelamaquina:~$ sudo passwd root

New password:
Retype new password:
passwd: password updated successfully

admin@nombredelamaquina:~$ sudo passwd admin

New password:
Retype new password:
passwd: password updated successfully



¿Cómo habilitamos poder entrar con root a nuestro VPS Ubuntu 20.04 LTS?



Añadimos el acceso con root para ssh desde admin:
sudo nano /etc/ssh/sshd_config

Y nos encontrarmos con este archivo:
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
  GNU nano 4.8                           /etc/ssh/sshd_config
|#       $OpenBSD: sshd_config,v 1.103 2018/04/09 20:41:22 tj Exp $

# This is the sshd server system-wide configuration file.  See
# sshd_config(5) for more information.

# This sshd was compiled with PATH=/usr/bin:/bin:/usr/sbin:/sbin

# The strategy used for options in the default sshd_config shipped with
# OpenSSH is to specify options with their default value where
# possible, but leave them commented.  Uncommented options override the
# default value.

Include /etc/ssh/sshd_config.d/*.conf

#Port 22
#AddressFamily any
#ListenAddress 0.0.0.0
#ListenAddress ::

#HostKey /etc/ssh/ssh_host_rsa_key
#HostKey /etc/ssh/ssh_host_ecdsa_key
#HostKey /etc/ssh/ssh_host_ed25519_key

# Ciphers and keying
#RekeyLimit default none

# Logging
#SyslogFacility AUTH
#LogLevel INFO

# Authentication:

#LoginGraceTime 2m
#PermitRootLogin prohibit-password
#StrictModes yes
#MaxAuthTries 6
#MaxSessions 10

#PubkeyAuthentication yes

# Expect .ssh/authorized_keys2 to be disregarded by default in future.
#AuthorizedKeysFile     .ssh/authorized_keys .ssh/authorized_keys2

#AuthorizedPrincipalsFile none

#AuthorizedKeysCommand none
#AuthorizedKeysCommandUser nobody

# For this to work you will also need host keys in /etc/ssh/ssh_known_hosts
#HostbasedAuthentication no
# Change to yes if you don't trust ~/.ssh/known_hosts for
# HostbasedAuthentication
#IgnoreUserKnownHosts no
# Don't read the user's ~/.rhosts and ~/.shosts files
#IgnoreRhosts yes

# To disable tunneled clear text passwords, change to no here!
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Vamos a modificar temporalmente:
"#PermitRootLogin prohibit-password"
Por
"PermitRootLogin yes"

Y ejecutamos el proceso de guardado:
Control X para guardar
Le damos la tecla "y"
Y aceptar para confirmar el lugar del archivo.

La consola nos tiene que quedar así:

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
admin@nombredelamaquina:~$ sudo passwd root
New password:
Retype new password:
passwd: password updated successfully
admin@nombredelamaquina:~$ sudo passwd admin
New password:
Retype new password:
passwd: password updated successfully
admin@nombredelamaquina:~$ sudo nano /etc/ssh/sshd_config
admin@nombredelamaquina:~$
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Reiniciamos el servicio de ssh
sudo service ssh restart

Y escribimos el acceso a root tal cual:
admin@nombredelamaquina:~$ ssh root@123.456.789.10

The authenticity of host '123.456.789.10 (123.456.789.10)' can't be established.
ECDSA key fingerprint is SHA256:tP7S3tm3MnNpIRBrN2Z/dyVl+5WowCNNEWWpFwwFAAw.
Are you sure you want to continue connecting (yes/no/[fingerprint])?

yes

Warning: Permanently added '123.456.789.10' (ECDSA) to the list of known hosts.

root@123.456.789.10's password:



¿Cómo actualizar nuestro VPS Ubuntu 20.04 LTS?



Actualizamos y upgradeamos todo con el mismo comando, por si hay contenido nuevo:

apt-get update && sudo apt-get upgrade



¿Cómo cambiar el hostname de nuestro VPS Ubuntu 20.04 LTS?



Cambiar el nombre del host
Por defecto, el hostname será el nombre de la máquina de VPS que hayamos contratado, en este caso
nombredelamaquina
Al cambiar el hostname, podremos asignar tanto a la IP como la IPV6.
En nuestro caso "server" para tanto a la IP como a IPV6 server.superdata.dev bajo el dominio "superdata.dev" a la máquina.
Para poder hacer esto, debemos de tener un dominio web previo.
[Mirar programa de partners]

Escribimos
nano /etc/hostname
Esto nos llevará a un editor de texto donde cambiamos:
nombredelamaquina (o lo que tengáis, si os pone nombredelamaquina.webvpscontratado.com lo cambiás por la segunda opción)
Por
server
Segunda opción: nombredelamaquina.webvpscontratado.com

Y ejecutamos el proceso de guardado ya mencionado al principio de la guía.

Vamos al archivo:
nano /etc/hosts
Y nos saldrá un archivo tal que así:

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
  GNU nano 4.8                                /etc/hosts
127.0.1.1 nombredelamaquina.webvpscontratado.com nombredelamaquina
127.0.0.1 localhost

# The following lines are desirable for IPv6 capable hosts
::1 localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Reemplazamos
127.0.1.1 nombredelamaquina.webvpscontratado.com nombredelamaquina
De la primera linea por:
127.0.1.1 server.superdata.dev server
Y añadimos al final del documento:
123.456.789.10 server.superdata.dev server

De tal forma que nos quede así

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
  GNU nano 4.8                                /etc/hosts
127.0.1.1 server.superdata.dev server
127.0.0.1 localhost

# The following lines are desirable for IPv6 capable hosts
::1 localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
123.456.789.10 server.superdata.dev server
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Si nuestro servicio vps cuenta con plantillas en la nube, no se va a cambiar nada de lo que hagamos al reiniciar, es por esto que debemos comprobar si existe ese directorio.
Para ello:

nano /etc/cloud/templates/hosts.debian.tmpl

Y nos saldrá un archivo tal que así:

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
{# The value '{{hostname}}' will be replaced with the local-hostname -#}
127.0.1.1 {{fqdn}} {{hostname}}
127.0.0.1 localhost

# The following lines are desirable for IPv6 capable hosts
::1 localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Y nos debe de quedar así:

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
127.0.1.1 server.superdata.dev server
127.0.0.1 localhost

# The following lines are desirable for IPv6 capable hosts
::1 localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
123.456.789.10 server.superdata.dev server
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Y ejecutamos el proceso de guardado ya mencionado al principio de la guía.



¿Cómo podemos saber si IPV6 está activado en nuestro VPS Ubuntu 20.04 LTS?

Ejecutamos

sudo nano /etc/default/ufw

Y nos saldrá un archivo tal que así:

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
# /etc/default/ufw
#

# Set to yes to apply rules to support IPv6 (no means only IPv6 on loopback
# accepted). You will need to 'disable' and then 'enable' the firewall for
# the changes to take affect.
IPV6=yes

# Mas ajustes abajo pero como no se ha de tocar nada por el momento, no se muestran...
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Y comprobamos en el archivo ufw de la carpeta default en el directorio etc, el apartado de IPV6 esté activado y con la palabra "yes".
IPV6=yes

Está activado y con "yes".

Sino lo está, lo ponemos
Y ejecutamos el proceso de guardado ya mencionado al principio de la guía.

A estas alturas, la consola nos quedaría tal que así:

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
root@nombredelamaquina:~# apt-get update && sudo apt-get upgrade
Get:1 http://security.ubuntu.com/ubuntu focal-security InRelease [114 kB]
Hit:2 http://archive.ubuntu.com/ubuntu focal InRelease
Get:3 http://archive.ubuntu.com/ubuntu focal-updates InRelease [114 kB]
Get:4 http://archive.ubuntu.com/ubuntu focal-backports InRelease [108 kB]
Fetched 336 kB in 1s (459 kB/s)
Reading package lists... Done
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
The following package was automatically installed and is no longer required:
  libfwupdplugin1
Use 'sudo apt autoremove' to remove it.
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
root@nombredelamaquina:~# nano /etc/hostname
root@nombredelamaquina:~# nano /etc/hosts
root@nombredelamaquina:~# nano /etc/hosts
root@nombredelamaquina:~# sudo nano /etc/default/ufw
root@nombredelamaquina:~# nano /etc/cloud/templates/hosts.debian.tmpl
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -



¿Cómo configurar correctamente los puertos de forma segura de nuestro VPS Ubuntu 20.04 LTS?



Ahora configuraremos los puertos, para tener las entradas y salidas óptimas.
Segun speedguide net, no hay nada asignado entre los puertos 25472-25575, lo que pretendemos es evitar ataques automáticos en nuestro servidor, es por esto que cambiaremo el puerto 22 y 22/tcp por un puerto aleatorio, para poder acceder por ssh de forma mas segura y así reducir el número de probabilidades de ser atacado.
Escogeremos un puerto aleatorio entre en el rango 25472-25575, por ejemplo: 25472, evitad a toda costa cojer el puerto 25565, porque si queréis montar un servidor de Minecraft, es el que usa MySQL Standard port Minecraft Dedicated Server (IANA official. (Speed Guide, Inc, 2009)
Aquí abriremos un puerto que es para configurar nuestra TLD de Handshake a través de la guía de La Guía HNS + PowerDNS + Nginx + DANE - Introduction de htools.work de Rithvik Vibhu en español. (Vibhu, 2021)
Así que si estás aquí solo para instalar Ubuntu 20.04 LTS, no abras el puerto 53, te darás cuenta de que es cuando lleguemos, no te preocupes.

Comenzamos a escribir:
ufw allow ssh

La consola nos devuelve esta respuesta que deseamos:
Rules updated
Rules updated (v6)

ufw allow 25472

La consola nos devuelve esta respuesta que deseamos:
Rules updated
Rules updated (v6)

La consola nos devuelve esta respuesta que deseamos:
ufw allow 53
Rules updated
Rules updated (v6)

La consola nos devuelve esta respuesta que deseamos:
ufw allow http
Rules updated
Rules updated (v6)

La consola nos devuelve esta respuesta que deseamos:
ufw allow https
Rules updated
Rules updated (v6)

La consola nos devuelve esta respuesta que deseamos:
ufw enable
Command may disrupt existing ssh connections. Proceed with operation (y|n)? y

Escribimos "y" y le damos a enter.

La consola nos devuelve esta respuesta que deseamos:
Firewall is active and enabled on system startup

Buscamos por numeración los puertos actuales:
ufw status numbered

La consola nos responderá tal que así:

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Status: active

     To                         Action      From
     --                         ------      ----
[ 1] 22/tcp                     ALLOW IN    Anywhere
[ 2] 25472                       ALLOW IN    Anywhere
[ 3] 53                         ALLOW IN    Anywhere
[ 4] 80/tcp                     ALLOW IN    Anywhere
[ 5] 443/tcp                    ALLOW IN    Anywhere
[ 6] 22/tcp (v6)                ALLOW IN    Anywhere (v6)
[ 7] 25472 (v6)                  ALLOW IN    Anywhere (v6)
[ 8] 53 (v6)                    ALLOW IN    Anywhere (v6)
[ 9] 80/tcp (v6)                ALLOW IN    Anywhere (v6)
[10] 443/tcp (v6)               ALLOW IN    Anywhere (v6)
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Ahora lo que vamos a hacer es borrar el puerto 22 porque este es vulnerable a ataques.
Cómo podéis ver, el puerto 22 está en la lista, es el puerto 1, el primer puerto que hay que borrar.

Por lo tanto ejecutaremos:
ufw delete 1

La consola nos devuelve esta respuesta que deseamos:
Deleting:
 allow 22/tcp
Proceed with operation (y|n)?

Escribimos "y" y le damos a enter.
y

La consola nos devuelve esta respuesta que deseamos:
Rule deleted

Volvemos a pedir la lista de puertos cono el comando ejecutado anteriormente:
ufw status numbered

La consola nos responderá tal que así:

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Status: active

     To                         Action      From
     --                         ------      ----
[ 1] 25472                       ALLOW IN    Anywhere
[ 2] 53                         ALLOW IN    Anywhere
[ 3] 80/tcp                     ALLOW IN    Anywhere
[ 4] 443/tcp                    ALLOW IN    Anywhere
[ 5] 22/tcp (v6)                ALLOW IN    Anywhere (v6)
[ 6] 25472 (v6)                  ALLOW IN    Anywhere (v6)
[ 7] 53 (v6)                    ALLOW IN    Anywhere (v6)
[ 8] 80/tcp (v6)                ALLOW IN    Anywhere (v6)
[ 9] 443/tcp (v6)               ALLOW IN    Anywhere (v6)
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Cómo podéis ver, el puerto 22 sigue en la lista, es el puerto 5, el segundo y último puerto que hay que borrar.

Por lo tanto ejecutaremos:
ufw delete 5

La consola nos devuelve esta respuesta que deseamos:
Deleting:
 allow 22/tcp
Proceed with operation (y|n)?

Escribimos "y" y le damos a enter.
y

La consola nos devuelve esta respuesta que deseamos:
Rule deleted (v6)

Volvemos a pedir la lista de puertos cono el comando ejecutado anteriormente:
ufw status numbered

La consola nos responderá tal que así:

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Status: active

     To                         Action      From
     --                         ------      ----
[ 1] 25472                       ALLOW IN    Anywhere
[ 2] 53                         ALLOW IN    Anywhere
[ 3] 80/tcp                     ALLOW IN    Anywhere
[ 4] 443/tcp                    ALLOW IN    Anywhere
[ 5] 25472 (v6)                  ALLOW IN    Anywhere (v6)
[ 6] 53 (v6)                    ALLOW IN    Anywhere (v6)
[ 7] 80/tcp (v6)                ALLOW IN    Anywhere (v6)
[ 8] 443/tcp (v6)               ALLOW IN    Anywhere (v6)
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -



¿Cómo cambiar el puerto de acceso ssh para protegernos de ataques a nuestro VPS Ubuntu 20.04 LTS?



Ahora vamos al archivo de configuración de sshd, para así modificar el puerto de acceso:
nano /etc/ssh/sshd_config
#Port 22
Lo activamos (quitando la hastag o almoadilla)
Y cambiamos el 22 por 25472

Port 25472

Que es en definitva un puerto que hemos abierto antes.

Y ejecutamos el proceso de guardado ya mencionado al principio de la guía.

Reiniciamos el servicio de ssh
service ssh restart

Y reiniciamos el servidor:
reboot

A estas alturas, la consola nos quedaría tal que así:

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
root@nombredelamaquina:~# ufw allow ssh
Rules updated
Rules updated (v6)
root@nombredelamaquina:~# ufw allow 25472
Rules updated
Rules updated (v6)
root@nombredelamaquina:~# ufw allow 53
Rules updated
Rules updated (v6)
root@nombredelamaquina:~# ufw allow http
Rules updated
Rules updated (v6)
root@nombredelamaquina:~# ufw allow https
Rules updated
Rules updated (v6)
root@nombredelamaquina:~# ufw enable
Command may disrupt existing ssh connections. Proceed with operation (y|n)? y
Firewall is active and enabled on system startup
root@nombredelamaquina:~# ufw status numbered
Status: active

     To                         Action      From
     --                         ------      ----
[ 1] 22/tcp                     ALLOW IN    Anywhere
[ 2] 25472                       ALLOW IN    Anywhere
[ 3] 53                         ALLOW IN    Anywhere
[ 4] 80/tcp                     ALLOW IN    Anywhere
[ 5] 443/tcp                    ALLOW IN    Anywhere
[ 6] 22/tcp (v6)                ALLOW IN    Anywhere (v6)
[ 7] 25472 (v6)                  ALLOW IN    Anywhere (v6)
[ 8] 53 (v6)                    ALLOW IN    Anywhere (v6)
[ 9] 80/tcp (v6)                ALLOW IN    Anywhere (v6)
[10] 443/tcp (v6)               ALLOW IN    Anywhere (v6)

root@nombredelamaquina:~# ufw delete 1
Deleting:
 allow 22/tcp
Proceed with operation (y|n)? y
Rule deleted
root@nombredelamaquina:~# ufw status numbered
Status: active

     To                         Action      From
     --                         ------      ----
[ 1] 25472                       ALLOW IN    Anywhere
[ 2] 53                         ALLOW IN    Anywhere
[ 3] 80/tcp                     ALLOW IN    Anywhere
[ 4] 443/tcp                    ALLOW IN    Anywhere
[ 5] 22/tcp (v6)                ALLOW IN    Anywhere (v6)
[ 6] 25472 (v6)                  ALLOW IN    Anywhere (v6)
[ 7] 53 (v6)                    ALLOW IN    Anywhere (v6)
[ 8] 80/tcp (v6)                ALLOW IN    Anywhere (v6)
[ 9] 443/tcp (v6)               ALLOW IN    Anywhere (v6)

root@nombredelamaquina:~# Status: active
     To                         Action      From
     --                         ------      ----
[ 1] 25472                       ALLOW IN    Anywhere
[ 2] 53                         ALLOW IN    Anywhere
[ 3] 80/tcp                     ALLOW IN    Anywhere
[ 4] 443/tcp                    ALLOW IN    Anywhere
[ 5] 22/tcp (v6)                ALLOW IN    Anywhere (v6)
[ 6] 25472 (v6)                  ALLOW IN    Anywhere (v6)
[ 7] 53 (v6)                    ALLOW IN    Anywhere (v6)
[ 8] 80/tcp (v6)                ALLOW IN    Anywhere (v6)
[ 9] 443/tcp (v6)               ALLOW IN    Anywhere (v6)

root@nombredelamaquina:~# ufw status numbered
Status: active

     To                         Action      From
     --                         ------      ----
[ 1] 25472                       ALLOW IN    Anywhere
[ 2] 53                         ALLOW IN    Anywhere
[ 3] 80/tcp                     ALLOW IN    Anywhere
[ 4] 443/tcp                    ALLOW IN    Anywhere
[ 5] 22/tcp (v6)                ALLOW IN    Anywhere (v6)
[ 6] 25472 (v6)                  ALLOW IN    Anywhere (v6)
[ 7] 53 (v6)                    ALLOW IN    Anywhere (v6)
[ 8] 80/tcp (v6)                ALLOW IN    Anywhere (v6)
[ 9] 443/tcp (v6)               ALLOW IN    Anywhere (v6)

root@nombredelamaquina:~# ufw delete 5
Deleting:
 allow 22/tcp
Proceed with operation (y|n)? y
Rule deleted (v6)
root@nombredelamaquina:~# ufw status numbered
Status: active

     To                         Action      From
     --                         ------      ----
[ 1] 25472                       ALLOW IN    Anywhere
[ 2] 53                         ALLOW IN    Anywhere
[ 3] 80/tcp                     ALLOW IN    Anywhere
[ 4] 443/tcp                    ALLOW IN    Anywhere
[ 5] 25472 (v6)                  ALLOW IN    Anywhere (v6)
[ 6] 53 (v6)                    ALLOW IN    Anywhere (v6)
[ 7] 80/tcp (v6)                ALLOW IN    Anywhere (v6)
[ 8] 443/tcp (v6)               ALLOW IN    Anywhere (v6)

root@nombredelamaquina:~# nano /etc/ssh/sshd_config
root@nombredelamaquina:~# service ssh restart
root@nombredelamaquina:~# reboot
root@nombredelamaquina:~# Connection to 123.456.789.10 closed by remote host.
Connection to 123.456.789.10 closed.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -



¿Cómo acceder a través del nuevo puerto en nuestro VPS Ubuntu 20.04 LTS?



Para acceder, ahora deberemos hacerlo desde el puerto designado anteriormente, con la flag -p:
ssh -p 25472 root@123.456.789.10



¿Cómo instalar la interfaz gráfica o tasksel en nuestro VPS Ubuntu 20.04 LTS?



Ahora instalaremos la parte gráfica del sistema operativo.
Por si no te habías dado cuenta... la versión solo cuenta con una consola.
Por lo tanto, ejecutaremos:

apt install tasksel

En un momento dado, nos preguntará si:
After this operation, 309 kB of additional disk space will be used.
Do you want to continue? [Y/n]

Respondemos con "Y" y le damos a enter, para esperar a que termine.
Y

Ejecutamos tasksel escribiendo en la consola:
tasksel


Nos moveremos con las flechas del teclado hacia arriba y hacia abajo.
Le damos a la barra espaciadora para activar las opciones que queremos:
Ejemplo de opción no seleccionada:
  │    [ ] Ubuntu desktop                                                                       │
Ejemplo de opción seleccionada:
  │    [*] Ubuntu desktop                                                                       │


En mi caso, voy a escoger esta configuración:
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Package configuration

  ┌───────────────────────────────────┤ Software selection ├────────────────────────────────────┐
  │ You can choose to install one or more of the following predefined collections of software.  │
  │                                                                                             │
  │ Choose software to install:                                                                 │
  │                                                                                             │
  │    [ ] Kubuntu live CD                                                                      │
  │    [ ] Lubuntu live CD                                                                      │
  │    [ ] Ubuntu Budgie live CD                                                                │
  │    [ ] Ubuntu live CD                                                                       │
  │    [ ] Ubuntu MATE Live CD                                                                  │
  │    [ ] Ubuntu Studio live DVD                                                               │
  │    [ ] Xubuntu live CD                                                                      │
  │    [*] Ubuntu Cloud Image (instance)                                                        │
  │    [ ] DNS server                                                                           │
  │    [ ] Kubuntu desktop                                                                      │
  │    [ ] LAMP server                                                                          │
  │    [ ] Lubuntu Desktop                                                                      │
  │    [ ] Mail server                                                                          │
  │    [ ] PostgreSQL database                                                                  │
  │    [ ] Print server                                                                         │
  │    [ ] Samba file server                                                                    │
  │    [ ] Ubuntu Budgie desktop                                                                │
  │    [*] Ubuntu desktop                                                                       │
  │    [ ] Ubuntu desktop default languages                                                     │
  │    [ ] Ubuntu minimal desktop                                                               │
  │    [ ] Ubuntu minimal desktop default languages                                             │
  │    [ ] Ubuntu MATE minimal                                                                  │
  │    [ ] Ubuntu MATE desktop                                                                  │
  │    [ ] Audio recording and editing suite                                                    │
  │    [ ] Ubuntu Studio desktop                                                                │
  │    [ ] Ubuntu Studio minimal DE installation                                                │
  │    [ ] Large selection of font packages                                                     │
  │    [ ] 2D/3D creation and editing suite                                                     │
  │    [ ] Photograph touchup and editing suite                                                 │
  │    [ ] Publishing applications                                                              │
  │    [ ] Video creation and editing suite                                                     │
  │    [ ] Xubuntu minimal installation                                                         │
  │    [ ] Xubuntu desktop                                                                      │
  │    [*] OpenSSH server                                                                       │
  │    [*] Basic Ubuntu server                                                                  │
  │                                                                                             │
  │                                                                                             │
  │                                           <Ok>                                              │
  │                                                                                             │
  └─────────────────────────────────────────────────────────────────────────────────────────────┘

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Le damos al tabulador para seleccionar "<Ok>" y luego a enter para aceptar y ejecutar.

Package configuration

 ┌───────────────────────────────────┤ Installing packages ├────────────────────────────────────┐
 │ Configuring something                                                                        │
 │                                                                                              │
 │                                                                                              │
 │                                                                                              │
 │                                              66%                                             │
 │                                                                                              │
 └──────────────────────────────────────────────────────────────────────────────────────────────┘

Y nos esperamos. No se toca nada, hasta que acabe.

Reiniciamos
reboot



¿Cómo acceder desde Remote Desktop, instalación y configuración de xrdp en nuestro VPS Ubuntu 20.04 LTS?



Una vez instalado, debemos seguir instalando la interfaz para poder entrar desde herramientas como sería remote desktop.
Para ello añadiremos el puerto 3389

Comenzamos a escribir:
ufw allow 3389

La consola nos devuelve esta respuesta que deseamos:
Rules updated
Rules updated (v6)

Y actualizamos y upgradeamos.

apt-get update && sudo apt-get upgrade

Instalamos xrdp

apt install xrdp

After this operation, 3212 kB of additional disk space will be used.
Do you want to continue? [Y/n]

Respondemos "y" y le damos a enter.
Y

Esperamos a que termine.

Visualizamos el estado de xrdp con el siguiente comando:

systemctl status xrdp

Es muy importante visualizar un piloto o punto verde al lado del servicio:
● xrdp.service - xrdp daemon
     Loaded: loaded (/lib/systemd/system/xrdp.service; enabled; vendor preset: enabled)
     Active: active (running)

Ese punto negro de la guía a de estar en verde.
Si es así, podemos continuar.

Añadimos un usuario en el grupo ss-cert para xrdp:

adduser xrdp ssl-cert

La consola nos devuelve esta respuesta que deseamos:

Adding user `xrdp' to group `ssl-cert' ...
Adding user xrdp to group ssl-cert
Done.

Reiniciamos el proceso xrdp:

systemctl restart xrdp



¿Cómo añadir mas ram a nuestro VPS Ubuntu 20.04 LTS?



Ahora añadiremos mas ram, sí. Este tipo de VPS, por lo general suelen venir con poca ram, 1GB, 2GB o 4GB, incluso si crees que te vas a quedar corto de ram con 8gb, podemos añadir más ram, gracias a nuestra memoria SSD.
Para ello debemos ejecutar:

fallocate -l 16G /ramr
La consola no devolverá respuesta, seguimos escribiendo y dando a enter:

chmod 600 /ramr
La consola no devolverá respuesta, seguimos escribiendo y dando a enter:

mkswap /ramr  && sudo swapon /ramr

La consola responde:
Setting up swapspace version 1, size = 16 GiB (17179865088 bytes)
no label, UUID=abb4d923-02b8-4f59-a391-d0b82bbed1aa

Editamos el archivo fstab para añadir la ram.
nano /etc/fstab

Esto se añade al final del documento:
/ramr swap swap defaults 0 0
Y ejecutamos el proceso de guardado ya mencionado al principio de la guía.

Mostramos las rams que hay:
swapon --show

La consola responde:
NAME  TYPE SIZE USED PRIO
/ramr file  16G   0B   -2

Liberamos la ram:
free -h

La consola responde:
              total        used        free      shared  buff/cache   available
Mem:          7.8Gi       411Mi       6.4Gi       4.0Mi       947Mi       7.1Gi
Swap:          15Gi          0B        15Gi

Reiniciamos
reboot



¿Cómo instalar net-tools en nuestro VPS Ubuntu 20.04 LTS?



apt install net-tools



¿Cómo añadir un nuevo usuario en nuestro VPS Ubuntu 20.04 LTS?



adduser newuser



La consola nos devuelve el siguiente questionario rápido:

root@server:~# adduser newuser
Adding user `newuser' ...
Adding new group `newuser' (1001) ...
Adding new user `newuser' (1001) with group `newuser' ...
Creating home directory `/home/newuser' ...
Copying files from `/etc/skel' ...

Escribimos una contraseña:
New password:

Repetimos la contraseña:
Retype new password:
passwd: password updated successfully

La consola nos dice:
Changing the user information for next
Enter the new value, or press ENTER for the default

Escribe o simplemente dale a enter para continuar (en esta guía se da enter y no se coloca nada.)
        Full Name []:
		
Se da enter y se deja vacío, esperando a la siguiente pregunta del cuestionario:
        Room Number []:

Se da enter y se deja vacío, esperando a la siguiente pregunta del cuestionario:
        Work Phone []:

Se da enter y se deja vacío, esperando a la siguiente pregunta del cuestionario:
        Home Phone []:

Se da enter y se deja vacío, esperando a la siguiente pregunta del cuestionario:
        Other []:

Se da enter y se deja vacío, esperando a la siguiente pregunta del cuestionario:
Is the information correct? [Y/n]

Escribimos "y" y le damos a enter:
Y




¿Cómo dar privilegios y añadir al grupo cualquier usuario de nuestro VPS Ubuntu 20.04 LTS?



usermod -aG sudo newuser



adduser next
La consola no devolverá respuesta, por lo tanto el procedimiento ha sido el correcto.



¿Cómo configurar DNS Server, por Rithvik Vibhu, nuestro VPS Ubuntu 20.04 LTS?

Logeamos con el usuario nuevo, y actuamos siempre con "sudo"
$ ssh -p 2083 next@207.180.198.58

Primero comprobamos la actividad del puerto 53, escrbiendo este comando.

sudo lsof -i :53

Y tras pedirnos la contraseña, la consola nos devolverá esta respuesta.
COMMAND   PID            USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
systemd-r 713 systemd-resolve   12u  IPv4  17878      0t0  UDP localhost:domain
systemd-r 713 systemd-resolve   13u  IPv4  17879      0t0  TCP localhost:domain (LISTEN)

Editamos el archivo, /etc/systemd/resolved.conf

sudo nano /etc/systemd/resolved.conf


Este es el archivo que nos dará la consola:

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
  GNU nano 4.8                                          /etc/systemd/resolved.conf

[Resolve]
#DNS=
#FallbackDNS=
#Domains=
#LLMNR=no
#MulticastDNS=no
#DNSSEC=no
#DNSOverTLS=no
#Cache=no-negative
#DNSStubListener=yes
#ReadEtcHosts=yes
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Y tenemos que añadir:
DNS=1.1.1.1
DNSStubListener=no

Y ejecutamos el proceso de guardado ya mencionado al principio de la guía.

Sincronizamos el archivo para usar el systemd's resolv config.

sudo ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf

Reiniciamos
sudo reboot

Entramos con el mismo usuario de antes y ejecutamos:

sudo lsof -i :53

Si está todo correcto, la consola nos devolverá:
Literalmente nada. Eso significará que podemos proseguir.



¿Cómo instalar PowerDNS siguiendo la guía de Rithvik Vibhu pero con versión 4.7 en nuestro VPS Ubuntu 20.04 LTS?



Añadimos la repo:
sudo sh -c "echo 'deb [arch=amd64] http://repo.powerdns.com/ubuntu focal-auth-47 main' > /etc/apt/sources.list.d/pdns.list"
sudo sh -c "echo 'Package: pdns-*\nPin: origin repo.powerdns.com\nPin-Priority: 600' > /etc/apt/preferences.d/pdns"
curl https://repo.powerdns.com/FD380FBB-pub.asc | sudo apt-key add - &&
sudo apt-get update &&
sudo apt-get install sqlite3 pdns-server pdns-backend-sqlite3 -y

Con todo esto la consola quedará tal que así:
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
next@server:~$ sudo sh -c "echo 'deb [arch=amd64] http://repo.powerdns.com/ubuntu focal-auth-47 main' > /etc/apt/sources.list.d/pdns.list"
[sudo] password for next:
next@server:~$ sudo sh -c "echo 'Package: pdns-*\nPin: origin repo.powerdns.com\nPin-Priority: 600' > /etc/apt/preferences.d/pdns"
next@server:~$ curl https://repo.powerdns.com/FD380FBB-pub.asc | sudo apt-key add - &&
> sudo apt-get update &&
>
next@server:~$ curl https://repo.powerdns.com/FD380FBB-pub.asc | sudo apt-key add - &&
> sudo apt-get update &&
> sudo apt-get install sqlite3 pdns-server pdns-backend-sqlite3 -y
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  1678  100  1678    0     0  16134      0 --:--:-- --:--:-- --:--:-- 16134
OK
Get:1 http://security.ubuntu.com/ubuntu focal-security InRelease [114 kB]
Hit:2 http://archive.ubuntu.com/ubuntu focal InRelease
Get:3 http://repo.powerdns.com/ubuntu focal-auth-47 InRelease [1814 B]
Get:4 http://archive.ubuntu.com/ubuntu focal-updates InRelease [114 kB]
Get:5 http://security.ubuntu.com/ubuntu focal-security/main amd64 Packages [1342 kB]
Get:6 http://security.ubuntu.com/ubuntu focal-security/main Translation-en [233 kB]
Get:7 http://security.ubuntu.com/ubuntu focal-security/main amd64 DEP-11 Metadata [40.7 kB]
Get:8 http://security.ubuntu.com/ubuntu focal-security/main amd64 c-n-f Metadata [9800 B]
Get:9 http://security.ubuntu.com/ubuntu focal-security/restricted amd64 Packages [825 kB]
Get:10 http://security.ubuntu.com/ubuntu focal-security/restricted Translation-en [117 kB]
Get:11 http://security.ubuntu.com/ubuntu focal-security/universe amd64 Packages [692 kB]
Get:12 http://security.ubuntu.com/ubuntu focal-security/universe Translation-en [121 kB]
Get:13 http://security.ubuntu.com/ubuntu focal-security/universe amd64 DEP-11 Metadata [66.5 kB]
Get:14 http://security.ubuntu.com/ubuntu focal-security/universe amd64 c-n-f Metadata [14.1 kB]
Get:15 http://security.ubuntu.com/ubuntu focal-security/multiverse amd64 DEP-11 Metadata [2464 B]
Get:16 http://archive.ubuntu.com/ubuntu focal-backports InRelease [108 kB]
Get:17 http://repo.powerdns.com/ubuntu focal-auth-47/main amd64 Packages [6555 B]
Get:18 http://archive.ubuntu.com/ubuntu focal-updates/main amd64 Packages [1669 kB]
Get:19 http://archive.ubuntu.com/ubuntu focal-updates/main amd64 DEP-11 Metadata [278 kB]
Get:20 http://archive.ubuntu.com/ubuntu focal-updates/main amd64 c-n-f Metadata [14.8 kB]
Get:21 http://archive.ubuntu.com/ubuntu focal-updates/universe amd64 Packages [910 kB]
Get:22 http://archive.ubuntu.com/ubuntu focal-updates/universe Translation-en [202 kB]
Get:23 http://archive.ubuntu.com/ubuntu focal-updates/universe amd64 DEP-11 Metadata [390 kB]
Get:24 http://archive.ubuntu.com/ubuntu focal-updates/universe amd64 c-n-f Metadata [20.3 kB]
Get:25 http://archive.ubuntu.com/ubuntu focal-updates/multiverse amd64 Packages [24.5 kB]
Get:26 http://archive.ubuntu.com/ubuntu focal-updates/multiverse amd64 DEP-11 Metadata [944 B]
Get:27 http://archive.ubuntu.com/ubuntu focal-backports/main amd64 DEP-11 Metadata [7988 B]
Get:28 http://archive.ubuntu.com/ubuntu focal-backports/universe amd64 DEP-11 Metadata [30.8 kB]
Fetched 7356 kB in 3s (2870 kB/s)
Reading package lists... Done
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
  libboost-program-options1.71.0 libluajit-5.1-2 libluajit-5.1-common pdns-backend-bind
Suggested packages:
  sqlite3-doc
The following NEW packages will be installed:
  libboost-program-options1.71.0 libluajit-5.1-2 libluajit-5.1-common pdns-backend-bind pdns-backend-sqlite3 pdns-server sqlite3
0 upgraded, 7 newly installed, 0 to remove and 2 not upgraded.
Need to get 4023 kB of archives.
After this operation, 19.3 MB of additional disk space will be used.
Get:1 http://repo.powerdns.com/ubuntu focal-auth-47/main amd64 pdns-server amd64 4.7.0~alpha1-1pdns.focal [2422 kB]
Get:2 http://archive.ubuntu.com/ubuntu focal/main amd64 libboost-program-options1.71.0 amd64 1.71.0-6ubuntu6 [342 kB]
Get:3 http://repo.powerdns.com/ubuntu focal-auth-47/main amd64 pdns-backend-bind amd64 4.7.0~alpha1-1pdns.focal [100 kB]
Get:4 http://repo.powerdns.com/ubuntu focal-auth-47/main amd64 pdns-backend-sqlite3 amd64 4.7.0~alpha1-1pdns.focal [26.1 kB]
Get:5 http://archive.ubuntu.com/ubuntu focal/universe amd64 libluajit-5.1-common all 2.1.0~beta3+dfsg-5.1build1 [44.3 kB]
Get:6 http://archive.ubuntu.com/ubuntu focal/universe amd64 libluajit-5.1-2 amd64 2.1.0~beta3+dfsg-5.1build1 [228 kB]
Get:7 http://archive.ubuntu.com/ubuntu focal-updates/main amd64 sqlite3 amd64 3.31.1-4ubuntu0.2 [860 kB]
Fetched 4023 kB in 1s (3636 kB/s)
Selecting previously unselected package libboost-program-options1.71.0:amd64.
(Reading database ... 168360 files and directories currently installed.)
Preparing to unpack .../0-libboost-program-options1.71.0_1.71.0-6ubuntu6_amd64.deb ...
Unpacking libboost-program-options1.71.0:amd64 (1.71.0-6ubuntu6) ...
Selecting previously unselected package libluajit-5.1-common.
Preparing to unpack .../1-libluajit-5.1-common_2.1.0~beta3+dfsg-5.1build1_all.deb ...
Unpacking libluajit-5.1-common (2.1.0~beta3+dfsg-5.1build1) ...
Selecting previously unselected package libluajit-5.1-2:amd64.
Preparing to unpack .../2-libluajit-5.1-2_2.1.0~beta3+dfsg-5.1build1_amd64.deb ...
Unpacking libluajit-5.1-2:amd64 (2.1.0~beta3+dfsg-5.1build1) ...
Selecting previously unselected package pdns-server.
Preparing to unpack .../3-pdns-server_4.7.0~alpha1-1pdns.focal_amd64.deb ...
Unpacking pdns-server (4.7.0~alpha1-1pdns.focal) ...
Selecting previously unselected package pdns-backend-bind.
Preparing to unpack .../4-pdns-backend-bind_4.7.0~alpha1-1pdns.focal_amd64.deb ...
Unpacking pdns-backend-bind (4.7.0~alpha1-1pdns.focal) ...
Selecting previously unselected package pdns-backend-sqlite3.
Preparing to unpack .../5-pdns-backend-sqlite3_4.7.0~alpha1-1pdns.focal_amd64.deb ...
Unpacking pdns-backend-sqlite3 (4.7.0~alpha1-1pdns.focal) ...
Selecting previously unselected package sqlite3.
Preparing to unpack .../6-sqlite3_3.31.1-4ubuntu0.2_amd64.deb ...
Unpacking sqlite3 (3.31.1-4ubuntu0.2) ...
Setting up libluajit-5.1-common (2.1.0~beta3+dfsg-5.1build1) ...
Setting up sqlite3 (3.31.1-4ubuntu0.2) ...
Setting up libboost-program-options1.71.0:amd64 (1.71.0-6ubuntu6) ...
Setting up libluajit-5.1-2:amd64 (2.1.0~beta3+dfsg-5.1build1) ...
Setting up pdns-server (4.7.0~alpha1-1pdns.focal) ...
Created symlink /etc/systemd/system/multi-user.target.wants/pdns.service → /lib/systemd/system/pdns.service.
Job for pdns.service failed because the control process exited with error code.
See "systemctl status pdns.service" and "journalctl -xe" for details.
Setting up pdns-backend-sqlite3 (4.7.0~alpha1-1pdns.focal) ...
Setting up pdns-backend-bind (4.7.0~alpha1-1pdns.focal) ...
Processing triggers for man-db (2.9.1-1) ...
Processing triggers for libc-bin (2.31-0ubuntu9.7) ...
Processing triggers for pdns-server (4.7.0~alpha1-1pdns.focal) ...
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Comprobamos el estado de PowerDNS

sudo systemctl status pdns

● pdns.service - PowerDNS Authoritative Server
     Loaded: loaded (/lib/systemd/system/pdns.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2022-03-22 17:49:10 CET; 1min 33s ago

Nos aseguramos de que el piloto es de color verde.

Y editamos la base de datos:

sudo nano /etc/powerdns/pdns.conf

En medio del archivo hay un "launch=" Borradlo!
Y poner estas 3 líneas al principo de todo el archivo.

launch=gsqlite3
gsqlite3-database=/var/lib/powerdns/pdns.sqlite3
gsqlite3-dnssec=yes

Y ejecutamos el proceso de guardado ya mencionado al principio de la guía.

Ejecutamos:

sudo sqlite3 /var/lib/powerdns/pdns.sqlite3 < /usr/share/doc/pdns-backend-sqlite3/schema.sqlite3.sql

Si es correcto, la consola no devolverá nada!

Ejecutamos:

sudo chown -R pdns:pdns /var/lib/powerdns

Si es correcto, la consola no devolverá nada!

Ejecutamos:

sudo systemctl restart pdns

Si es correcto, la consola no devolverá nada!

Comprobamos el estado de pdns:
sudo systemctl status pdns

● pdns.service - PowerDNS Authoritative Server
     Loaded: loaded (/lib/systemd/system/pdns.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2022-03-22 17:55:36 CET; 26s ago

Nos aseguramos de que tenga	 un piloto verde.



¿Cómo creo una zona para nuestra extensión TLDs de Handshake en nuestro VPS Ubuntu 20.04 LTS?



Creo una zona vacía:

sudo -u pdns pdnsutil create-zone cil ns1.cil

Activo el DNSSEC:

sudo -u pdns pdnsutil secure-zone cil

Añado un record TXT:

sudo -u pdns pdnsutil add-record cil. @ TXT '"first-record!"'

Y lo comprobamos:

dig @127.0.0.1 cil TXT +dnssec



Con todo esto la consola quedará tal que así:
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
next@server:/etc/powerdns/pdns.d$ sudo -u pdns pdnsutil create-zone cil ns1.cil
Mar 22 17:59:01 [bindbackend] Done parsing domains, 0 rejected, 0 new, 0 removed
Creating empty zone 'cil'
Also adding one NS record
next@server:/etc/powerdns/pdns.d$ sudo -u pdns pdnsutil secure-zone cil
Mar 22 17:59:05 [bindbackend] Done parsing domains, 0 rejected, 0 new, 0 removed
Securing zone with default key size
Adding CSK (257) with algorithm ecdsa256
Zone cil secured
Adding NSEC ordering information for zone 'cil', 1 updates
next@server:/etc/powerdns/pdns.d$ sudo -u pdns pdnsutil add-record cil. @ TXT '"first-record!"'
Mar 22 17:59:08 [bindbackend] Done parsing domains, 0 rejected, 0 new, 0 removed
New rrset:
cil. 3600 IN TXT "first-record!"
next@server:/etc/powerdns/pdns.d$ dig @127.0.0.1 cil TXT +dnssec

; <<>> DiG 9.16.1-Ubuntu <<>> @127.0.0.1 cil TXT +dnssec
; (1 server found)
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: REFUSED, id: 60344
;; flags: qr rd; QUERY: 1, ANSWER: 0, AUTHORITY: 0, ADDITIONAL: 1
;; WARNING: recursion requested but not available

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags: do; udp: 1232
;; QUESTION SECTION:
;cil.                           IN      TXT

;; Query time: 0 msec
;; SERVER: 127.0.0.1#53(127.0.0.1)
;; WHEN: Tue Mar 22 17:59:15 CET 2022
;; MSG SIZE  rcvd: 32
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -



¿Cómo cómo apunto mi TLDs de Handshake a nuestro VPS Ubuntu 20.04 LTS?



Ejecutamos:

sudo pdnsutil show-zone cil



Con todo esto la consola quedará tal que así:
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
next@server:/etc/powerdns/pdns.d$ sudo pdnsutil show-zone cil
Mar 22 18:01:23 [bindbackend] Done parsing domains, 0 rejected, 0 new, 0 removed
This is a Native zone
Metadata items: None
Zone has NSEC semantics
keys:
ID = 1 (CSK), flags = 257, tag = 8696, algo = 13, bits = 256      Active         Published  ( ECDSAP256SHA256 )
CSK DNSKEY = cil. IN DNSKEY 257 3 13 OoL4Nuox18AMrdsqXjDJeOq05xOotlTiURe4TMJDM7aL6GgE0ALaKDFz6YKS9HWlP9365p4lRb1zEa79FxoxhA== ; ( ECDSAP256SHA256 )
DS = cil. IN DS 8696 13 2 4b4d7e9742c6526ebd2460330a8841de0283470fb48ed4c307e706544096f09d ; ( SHA256 digest )
DS = cil. IN DS 8696 13 4 94402dc0a63495433a97db4e3a7c319d7af6f0bcb86202cfe4eb584b4e85b68789a07d120117aa56d491aeb9ae82e5f3 ; ( SHA-384 digest )
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Nos quedamos con el "13 2"

DS = cil. IN DS 8696 13 2 4b4d7e9742c6526ebd2460330a8841de0283470fb48ed4c307e706544096f09d ; ( SHA256 digest )

Añadimos a Bob Wallet

NS ns1.cil
GLUE ns1.cil. 123.456.789.10
DS 8696 13 2 4b4d7e9742c6526ebd2460330a8841de0283470fb48ed4c307e706544096f09d

Y nos esperamos, podremos comprobar que todo va bien, ejecutando:

dig @123.456.789.10 cil TXT +dnssec



¿Cómo configuro el webserver en nuestro VPS Ubuntu 20.04 LTS?


ejecutamos:

sudo apt install nginx -y

Y esperamos a que se intale todo.

Ejecutamos este código para poder crear una carpeta para la web.

sudo mkdir -p /var/www/cil

Si es correcto, la consola no devolverá nada.

Añadimos contenido básico a la web:

sudo sh -c "echo '<html><body><h1>Web de Handshake</h1></html>' > /var/www/cil/index.html"

Si es correcto, la consola no devolverá nada.

Creamos un archivo y pegamos el texto de a continuación:

sudo nano /etc/nginx/sites-available/cil

server {
  listen 80;
  listen [::]:80;
  root /var/www/cil;
  index index.html;
  server_name cil;
  location / {
    try_files $uri $uri/ =404;
  }
}

Y ejecutamos el proceso de guardado ya mencionado al principio de la guía.

Lo activamos ejecutando el siguiente código:

sudo ln -s /etc/nginx/sites-available/cil /etc/nginx/sites-enabled/cil

Si es correcto, la consola no devolverá nada.

Reiniciamos nginx

sudo systemctl restart nginx

Si es correcto, la consola no devolverá nada.

Comprobamos el estado del mismo nginx:

sudo systemctl status nginx

● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2022-03-22 18:13:16 CET; 10s ago

Es muy importante visualizar un piloto o punto verde al lado del servicio:

Añadimos un record con la ip

sudo -u pdns pdnsutil add-record cil. @ A 207.180.198.58

Mar 22 18:14:39 [bindbackend] Done parsing domains, 0 rejected, 0 new, 0 removed
New rrset:
cil. 3600 IN A 123.456.789.10


¿Cómo añado el canado verde DANE en nuestro VPS Ubuntu 20.04 LTS?

Comenzamos a copiar y a pegar tranquilamente linea a linea:

openssl req -x509 -newkey rsa:4096 -sha256 -days 365 -nodes \
  -keyout cert.key -out cert.crt -extensions ext  -config \
  <(echo "[req]";
    echo distinguished_name=req;
    echo "[ext]";
    echo "keyUsage=critical,digitalSignature,keyEncipherment";
    echo "extendedKeyUsage=serverAuth";
    echo "basicConstraints=critical,CA:FALSE";
    echo "subjectAltName=DNS:cil,DNS:*.cil";
    ) -subj "/CN=*.cil"

La consola responde: "writing new private key to 'cert.key'" Todo es correcto.

Ejecutamos:

echo -n "3 1 1 " && openssl x509 -in cert.crt -pubkey -noout | openssl pkey -pubin -outform der | openssl dgst -sha256 -binary | xxd  -p -u -c 32

Nos responderá con un registro: LO GUARDAMOS PARA EL FUTURO!

3 1 1 A21411F3E824461799400A0B8ECCFDE54F5E1ABDA75CFF78C74484F2E5091135

Movemos de lugar los dos archivos generados:

sudo mv cert.key /etc/ssl/cil.key

Si es correcto, la consola no devolverá nada.

sudo mv cert.crt /etc/ssl/cil.crt

Si es correcto, la consola no devolverá nada.

Añadimos unas lineas a este archivo que inicialmente creamos en la parte anterior de la guía.

sudo nano /etc/nginx/sites-available/cil

Añadimos estas lineas antes del último cierre de código.

  listen 443 ssl;
  ssl_certificate /etc/ssl/cil.crt;
  ssl_certificate_key /etc/ssl/cil.key;

Con todo esto el archivo quedará tal que así:
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
  GNU nano 4.8                                    /etc/nginx/sites-available/cil                                    Modified
server {
  listen 80;
  listen [::]:80;
  root /var/www/cil;
  index index.html;
  server_name cil;
  location / {
    try_files $uri $uri/ =404;
  }
  listen 443 ssl;
  ssl_certificate /etc/ssl/cil.crt;
  ssl_certificate_key /etc/ssl/cil.key;
}
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Y ejecutamos el proceso de guardado ya mencionado al principio de la guía.

Reiniciamos nginx:

sudo nginx -t && sudo systemctl restart nginx

Añadimos el record TLSA con el registro aquel que hemos guardado!

sudo -u pdns pdnsutil add-record cil. _443._tcp TLSA '3 1 1 A21411F3E824461799400A0B8ECCFDE54F5E1ABDA75CFF78C74484F2E5091135'

La consola nos responde:

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Mar 22 18:23:07 [bindbackend] Done parsing domains, 0 rejected, 0 new, 0 removed
New rrset:
_443._tcp.cil. 3600 IN TLSA 3 1 1 a21411f3e824461799400a0b8eccfde54f5e1abda75cff78c74484f2e5091135
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Llamamos para rectificar la zona:

sudo -u pdns pdnsutil rectify-zone cil

La consola nos responde:

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Mar 22 18:24:04 [bindbackend] Done parsing domains, 0 rejected, 0 new, 0 removed
Adding NSEC ordering information for zone 'cil', 3 updates
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -



¿Cómo instalar Docker en nuestro VPS Ubuntu 20.04 LTS?



Primero actualizamos

sudo apt-get update

Nos pedirá la contraseña, la colocamos y damos a enter, esperamos a que termine y seguimos.

sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release

Le damos a enter y esperamos a terminar.

Ponemos el curl deseado, en este caso:

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

Si es correcto, la consola no devolverá nada.

Ponemos este código para pedirle la versión estable:

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  
Si es correcto, la consola no devolverá nada.



¿Cómo instalar Docker Engine en nuestro VPS Ubuntu 20.04 LTS?


Actualizamos:

sudo apt-get update

E incorporamos este código:

sudo apt-get install docker-ce docker-ce-cli containerd.io

Comprobamos que esté bien instalado:

sudo docker run hello-world

La consola nos responde:

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
2db29710123e: Pull complete
Digest: sha256:bfea6278a0a267fad2634554f4f0c6f31981eea41c553fdf5a83e95a41d40c38
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -



¿Cómo actualizar Docker en nuestro VPS Ubuntu 20.04 LTS?



Ejectuamos:

sudo apt-get update

Ejecutamos esto para visualizar que versiones existen:

apt-cache madison docker-ce

La consola nos responde:

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
 docker-ce | 5:20.10.13~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.12~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.11~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.10~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.9~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.8~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.7~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.6~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.5~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.4~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.3~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.2~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.1~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:20.10.0~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.15~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.14~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.13~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.12~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.11~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.10~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
 docker-ce | 5:19.03.9~3-0~ubuntu-focal | https://download.docker.com/linux/ubuntu focal/stable amd64 Packages
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Comprobamos que nuestra versión sea la última:

next@server:~$ docker -v

La consola nos responde:

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Docker version 20.10.13, build a224086
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -



¿Cómo actualizar Docker Engine en nuestro VPS Ubuntu 20.04 LTS?



Ejecutamos:

curl -fsSL https://get.docker.com -o get-docker.sh

Si es correcto, la consola no devolverá nada.

Ejecutamos:

sudo sh get-docker.sh

La consola nos responde:

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
# Executing docker install script, commit: 93d2499759296ac1f9c510605fef85052a2c32be
Warning: the "docker" command appears to already exist on this system.

If you already have Docker installed, this script can cause trouble, which is
why we're displaying this warning and provide the opportunity to cancel the
installation.

If you installed the current Docker package using this script and are using it
again to update Docker, you can safely ignore this message.

You may press Ctrl+C now to abort this script.
+ sleep 20
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Presionamos Control+C para salir, tenemos la última versión.



¿Cómo añadir a Docker en el grupo sudo en nuestro VPS Ubuntu 20.04 LTS?

Ejecutamos:

sudo -id

Esto hará que accedamos a root.

Ejecutamos:

sudo groupadd docker

Si ya existe, la consola nos lo dirá:

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
groupadd: group 'docker' already exists
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Ejecutamos:

sudo usermod -aG docker $USER

Si es correcto, la consola no devolverá nada.

Ejecutamos:

docker run hello-world

Si la consola responde correctamente, sin "sudo" es que has incorporado tu usuario con docker y sudo.

Si ya existe, la consola nos lo dirá:

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -


¿Cómo ejecutar Docker en el inicio de sesión en nuestro VPS Ubuntu 20.04 LTS?



Ejecutamos:

sudo systemctl enable docker.service

La consola nos responde:

Synchronizing state of docker.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable docker

Ejectuamos:

sudo systemctl enable containerd.service

Si es correcto, la consola no devolverá nada.



¿Cómo instalar Docker Compose en nuestro VPS Ubuntu 20.04 LTS?



Ejecutamos:

sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

La consola nos respodnerá:

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   664  100   664    0     0  13551      0 --:--:-- --:--:-- --:--:-- 13551
100 12.1M  100 12.1M    0     0  16.5M      0 --:--:-- --:--:-- --:--:-- 23.4M
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Ejecutamos:

sudo chmod +x /usr/local/bin/docker-compose

Si es correcto, la consola no devolverá nada.

Comprobamos la versión:

docker-compose --version

La consola respodnerá:

docker-compose version 1.29.2, build 5becea4c



¿Cómo instalar Command-line completion para Docker Compose en nuestro VPS Ubuntu 20.04 LTS?



sudo curl \
    -L https://raw.githubusercontent.com/docker/compose/1.29.2/contrib/completion/bash/docker-compose \
    -o /etc/bash_completion.d/docker-compose

La consola nos respodnerá:

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 13500  100 13500    0     0  69230      0 --:--:-- --:--:-- --:--:-- 69587
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Recargamos la terminal:

source ~/.bashrc

Si es correcto, la consola no devolverá nada.



¿Cómo instalar PowerDNS-Admin con Docker en nuestro VPS Ubuntu 20.04 LTS?



Ejecutamos:

docker run -d \
    -e SECRET_KEY='a-very-secret-key' \
    -v pda-data:/data \
    -p 9191:80 \
    ngoduykhanh/powerdns-admin:latest

La consola nos respodnerá:

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Unable to find image 'ngoduykhanh/powerdns-admin:latest' locally
latest: Pulling from ngoduykhanh/powerdns-admin
5758d4e389a3: Pull complete
2c40eb35ff36: Pull complete
b218723011f1: Pull complete
41c45f2f26c3: Pull complete
7f33384f5962: Pull complete
3e4abb6e835a: Pull complete
4b675c72e18e: Pull complete
Digest: sha256:67886c94cf627b351269313ee3564d8bc9d4a0cef6986a15ba5c34459f1aee16
Status: Downloaded newer image for ngoduykhanh/powerdns-admin:latest
d7f7192dfe830e78737e4ac77526b8e440309cf843e5254d1d147d690b0ded61
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

Cojemos nuestro buscador y ponemos nuestra ip seguida de dos puntos con el puerto deseado, en este caso, 9191:

123.456.789.10:9191

Creamos un usuario.

Veremos un recuadro rojo, colocamos los datos correctos:

Vamos al panel de PowerDNS-Admin y en el sidebar veremos un sitio que pone: "API Keys"

Generamos una, la copiamos y la pegamos en API-Key en el paso siguiente:

Vamos a cd /etc/powerdns/pdns.sqlite3

Y pegamos:

webserver-port=8081
api=yes
api-key=LAPEGAMOSAQUI

dkxORDZ1b1JXWDd0WnM3

En el recuadro rojo de PowerDNS-Admin ponemos:

PDNS API URL http://pdns:8081/
PDNS API KEY LAPEGAMOSAQUI
PDNS VERSION 4.7.0

9jA6iTVns/ZXiMNk5L2OHptiiz2tJs8O0Fu3+A8HI5s=
/var/lib/powerdns/pdns.sqlite3
4.7.0-alpha1







¿Qué pasa si me pide autorización en nuestro VPS Ubuntu 20.04 LTS?

next@server:~$ systemctl restart pdns.service
==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
Authentication is required to restart 'pdns.service'.
Multiple identities can be used for authentication:
 1.  Ubuntu (admin)
 2.  ,,, (next)

Le das a Control+C, sales de ese engaño.
Choose identity to authenticate as (1-2): ^C

Nos vamos a root.
next@server:~$ sudo -i

Y entonces ejecutamos sin errores.
root@server:~# systemctl restart pdns.service
