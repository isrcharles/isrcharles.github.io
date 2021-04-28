# Otros comandos útiles

## rsync

Sincroniza archivos y directorios de forma local y remota. Útil cuando solo se desea copiar lo nuevo de un directorio a otro.

- Crear una copia local del contenido del home de un usuario
```bash
$ rsync -var /home/ray /tmp/ray-new-home
```

Si se modifica un archivo, rsync solo copiará dicho archivo. Con esto se ahorra procesamiento y ancho de banda en caso de que el destino sea remoto.

	$ touch /home/ray/.bashrc
	$ rsync -var /home/ray /tmp/ray-new-home

- Crear una copia remota del contenido del home de un usuario  
```bash
$ rsync -var /home/ray ray@192.168.1.2:/tmp/ray-new-home
```

De igual forma, en caso de que algunos archivos cambien, solo se copiarán estos la próxima vez que se ejecute `rsync`

## pv

Permite monitorear el progreso de los datos que cruzan una pipa `|`

- Crear un archivo con dd

	$ dd if=/dev/zero bs=1M count=512 | pv | dd of=/tmp/512M.file

- Respaldar el home de un usuario u otro directorio

```bash
SIZE=`du -sk /home/ray | cut -f 1`
tar cvf - /home/ray | pv -p -s ${SIZE}k | bzip2 -c > rayshome.tar.bz2
```

## NET-TOOLS

Es un conjunto de herramientas que ofrecen información relacionada con la red.

### route

Imprime la tabla de ruteo.

	$ route 
	Tabla de rutas IP del núcleo
	Destino         Pasarela        Genmask         Indic Métric Ref    Uso Interfaz
	default         nethsec.sinc.la 0.0.0.0         UG    0      0        0 enp0s3
	10.248.96.0     *               255.255.255.0   U     0      0        0 enp0s3

### netstat

Entre otras cosas permite ver los puertos abiertos o en estado de escucha en el servidor

	$ netstat -tulpen
	(No todos los procesos pueden ser identificados, no hay información de propiedad del proceso
	 no se mostrarán, necesita ser superusuario para verlos todos.)
	Conexiones activas de Internet (solo servidores)
	Proto  Recib Enviad Dirección local         Dirección remota       Estado       User       Inode       PID/Program name
	tcp        0      0 0.0.0.0:22              0.0.0.0:*               ESCUCHAR    0          16052       -               
	tcp6       0      0 :::22                   :::*                    ESCUCHAR    0          16054       -               
	udp        0      0 0.0.0.0:68              0.0.0.0:*                           0          13607       -

### BIND-UTILS

#### nslookup

Permiten consultar servidores de nombres de dominio

	$ nslookup youtube.com
	Server:		10.248.96.254
	Address:	10.248.96.254#53
	
	Non-authoritative answer:
	Name:	youtube.com
	Address: 64.233.180.93
	Name:	youtube.com
	Address: 64.233.180.136
	Name:	youtube.com
	Address: 64.233.180.190
	Name:	youtube.com
	Address: 64.233.180.91

#### host

	$ host google.com

#### dig

	$ dig google.com

### telnet

Permite corroborar si un puerto (tcp) está abierto en un dominio/ip

	$ telnet google.com 80
	Trying 172.217.1.238...
	Connected to google.com.
	Escape character is '^]'.
	Nota: Para salir presione ctrl + 5 y después ctrl-d

> :pencil: _**NOTA:**_ Para salir presione ctrl + 5 y después ctrl-d

### nmap

Muestra los puertos abiertos de otro host. 

!> Importante: escanear host sin autorización puede considerarse una forma de ataque, por lo que está expuesto a entrar en lista negra.

	$ nmap 10.248.96.148
	Starting Nmap 7.01 ( https://nmap.org ) at 2017-11-24 21:16 CST
	Nmap scan report for u16rtest.sinc.lan (10.248.96.148)
	Host is up (0.00055s latency).
	Not shown: 999 closed ports
	PORT   STATE SERVICE
	22/tcp open  ssh

	Nmap done: 1 IP address (1 host up) scanned in 0.06 seconds

### wget

Permite descargar archivos desde servidores web

Obtener mi ip pública por terminal:

	$ wget -qO- http://ipecho.net/plain ; echo

Descargar el iso de Ubuntu server:

	$ wget http://releases.ubuntu.com/16.04.3/ubuntu-16.04.3-server-amd64.iso?_ga=2.151137977.1803712038.1511580006-199111890.1504028981

### links

Es un navegador web por terminal.

	$ links www.duckduckgo.com

Para salir presiona la letra q

### htop

Permite monitorear procesos y ofrece información útil sobre los recursos del sistema tales como:

- Uso del procesador
- Uso de la memoria ram
- Uso de la memoria swap
- Carga promedio
- Tareas
- Uptime  

		$ htop

Para salir presione `q`

### iotop

Permite ver la velocidad de lectura/escritura a disco (i/o) en tiempo real.

	$ sudo iotop

Ejemplo de la salida de iotop

	Total DISK READ :       6.70 M/s | Total DISK WRITE :       2.78 M/s
	Actual DISK READ:       6.70 M/s | Actual DISK WRITE:     109.13 K/s
	PID  PRIO  USER     DISK READ  DISK WRITE  SWAPIN     IO>    COMMAND                                                
	1053 be/4 postgres    0.00 B/s   77.95 K/s  0.00 % 17.94 % postgres: wal writer process
	23232 be/4 postgres 1122.51 K/s    0.00 B/s  0.00 % 11.71 % postgres: ray otrabd_irm [local] COPY
	23171 be/4 postgres  873.06 K/s  802.91 K/s  0.00 %  7.54 % postgres: autovacuum worker process   cliente201711_irm
	23151 be/4 root        3.29 M/s    0.00 B/s  0.00 %  1.39 % tar cvf - ray
	23166 be/4 root        0.00 B/s    0.00 B/s  0.00 %  0.01 % [kworker/u2:2]
	23153 be/4 root        0.00 B/s  413.15 K/s  0.00 %  0.00 % bzip2 -c
	23197 be/4 postgres  748.34 K/s  779.52 K/s  0.00 %  0.00 % postgres: autovacuum worker process   demo_jun2017
	23231 be/4 ray        0.00 B/s   27.28 K/s  0.00 %  0.00 % bzip2
	23241 be/4 postgres  748.34 K/s  748.34 K/s  0.00 %  0.00 % postgres: autovacuum worker process   somedb

### iftop

Permiten ver datos sobre el tráfico de red en tiempo real. Si se instala en el gateway de la red, puede ayudar a encontrar el "porque está lento el Internet".

	$ sudo iftop 

## screen

Permite crear emuladores de terminal. Útil para dejar corriendo procesos en equipos remotos y si se cae la conexión, el proceso seguirá funcionando.

Para crear una terminal nueva

	$ screen

Para salir de la terminal use `ctrl-a` y después `ctrl-d`

Para volver a entrar a la terminal en donde se dejó corriendo el proceso:

	$ screen -r

Nota: en caso de que se tenga más de una terminal emulada, screen mostrará los procesos y se tendrá que seleccionar al que se desea entrar:

	$ screen -r proceso

## lsof

Muestra los archivos abiertos filtrando por diferentes opciones.

Encontrar procesos que tienen abierto un archivo:

	$ lsof /ruta/al/archivo

Encontrar procesos que tienen abierto un puerto local:

	$ lsof -i :puerto

Mostrar solo el id del proceso

	$ lsof -t /ruta/al/archivo

Mostrar archivos abiertos por un usuario:

	$ lsof -u usuario

Mostrar archivos abiertos por un proceso o comando:

	$ lsof -c proceso|comando
