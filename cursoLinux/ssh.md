### Introducción

Tarde que temprano como administrador de sistemas se tiene que empezar a administrar diversos servidores, los cuales pueden estar a unos cuantos metros o en otro país o continente. `SSH` permite acceder a un servidor Linux de forma remota y ejecutar comandos tal y como si se estuviese en frente del equipo. La ventaja de conectarse por `SSH` es que toda la información que fluya entre ambos equipos, estará cifrada.

`SSH` se divide en 2 partes, un servicio corriendo en el equipo al que se desea acceder (servidor o incluso un pc/lap) y un cliente instalado en la pc o lap del administrador.

### Instalación

Durante la instalación de `Ubuntu server`, existe la opción para instalar el servidor `SSH` por medio de tasksel, pero en caso de no haber seleccionado dicha opción, se puede instalar con el comando:

	sudo apt-get install openssh-server

Este paquete solo se deberá instalar en aquellos equipos a los que se desee acceder de forma remota. Normalmente el cliente ya viene instalado por default en `Ubuntu Desktop` y otras distribuciones, pero en caso de que no sea así, se puede instalar mediante el comando:

	sudo apt-get install openssh-client

### Uso

Para conectar a un servidor remoto es necesario ejecutar el comando:

	ssh usuario@ip #usando una ip en forma 192.168.X.Y
	ssh usuario@equipo-remoto #usando un nombre de dominio

Si es la primera vez que se conecta a dicho equipo, la salida del comando será parecida a la siguiente:

	The authenticity of host 'equipo-remoto (::1)' can't be established.
	ECDSA key fingerprint is 4a:22:11:33:44:4f:66:55:56:e4:f0:db:99:aa:bb:cc.
	Are you sure you want to continue connecting (yes/no)?

Lo anterior es debido a que no se puede garantizar que el equipo sea quien dice ser (vea ataques :fa-globe:[MITM](https://es.wikipedia.org/wiki/Ataque_de_intermediario)), es por eso que SSH deja la decisión al usuario (contestar `yes/no`). Una forma sencilla de corroborar que el equipo es realmente aquel al que se desea conectar es ejecutando de forma local en dicho equipo el comando:

	ssh localhost

Si el `fingerprint` es igual al que se presenta al intentar conectar de forma remota, se puede confiar en conectarse a dicho equipo.

Una vez que se confirma el deseo de continuar, el sistema remoto pide la contraseña del usuario con el que se pretende autenticar.

	usuario@ip-equipo-remoto's password:

Al ingresar correctamente el password, se puede observar que el prompt de la terminal cambia:

	usuario@equipo-remoto ~ $

### SCP

SCP forma parte de la suite de utilerías de `SSH` y permite copiar archivos entre distintos equipos de forma segura. Su uso es el siguiente:

	scp /ruta/a/archivo/origen usuario@ip:/ruta/de/destino #enviar un archivo desde equipo local a otro
	scp usuario@ip:/ruta/de/origen /ruta/a/archivo/destino #jalar un archivo desde un equipo remoto al local

Al igual que con SSH, es necesario haber instalado el `openssh-server` previo a intentar copiar archivos a dicho equipo. De igual forma, si es la primera vez que se intenta conectar al equipo, se debe corroborar el `fingerprint` y aceptar la conexión.

Como variante, al intentar copiar un directorio completo, es necesario utilizar la opción `-r`:

	scp -r miDirectorio usuario@ip:/ruta/destino

