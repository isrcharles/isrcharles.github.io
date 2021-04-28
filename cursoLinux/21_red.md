### Definir el nombre del host

El nombre del equipo se define durante la instalación y es importante porque lo diferencia del resto de la red. Es muy recomendable establecer una convención para el nombre de cada equipo de la red. Algunas empresas usan por convención nombres de planetas o satelites, personajes de caricatura, seres mitológicos, entre otros. La decisión final es de cada admin.

Por defecto, si no se cambia el nombre durante la instalación, el equipo usará `ubuntu` como nombre. En caso de necesitar cambiarlo, se debe realizar dicho cambio en el archivo `/etc/hostname`. También es necesario definir el nombre y opcionalmente el dominio al que pertenece en el archivo `/etc/hosts`.

Modificar `hostname`

	sudo cat /etc/hostname #Opcional: ver nombre actual
	  israel-pc
	sudo echo icharles-lap > /etc/hostname # Cambiar nombre al equipo

Adaptar archivo `hosts` editando la linea:

	127.0.1.1	israel-pc

Cambiar por:

	127.0.1.1	icharles-lap

Opcionalmente se puede agregar el dominio:

	127.0.1.1     icharles-lap    icharles-lap.charlesit.mx

Por último, se debe reiniciar el equipo para que los cambios surtan efecto.

### Configuración de interfaz por dhcp

Por default, Ubuntu intenta obtener una dirección ip al arranque del equipo, pero en dado caso que por algún motivo el equipo haya arrancado y no se haya configurado automáticamente la interfaz, es posible asignar una ip (siempre que en la red exista un servidor DHCP) con el siguiente comando:

	sudo dhclient -v enp0s3

Si se desea dejar la ip asignada de la interfaz configurada mediante DHCP, se utiliza el comando:

	sudo dhclient -r enp0s3

Tome en cuenta que si los archivos de configuración no hacen referencia a obtener una ip por DHCP, se tendrá que realizar el comando `dhclient -v enp0s3` cada vez que inicie el equipo.

### Gestión de interfaces de red Ubuntu 18.04 y superior

Si el hardware de red se detectó correctamente, las interfaces disponibles se podrán conocer con el siguiente comando:

	ls /sys/class/net
	enp0s3 lo 

Se puede mostrar la información de las interfaces de red disponibles con el comando:

	ip addr show # Este comando se puede abreviar como ip a

Si se tiene solo una interfaz de red, por default el comando anterior muestra dos interfaces:

- lo: La interfaz loopback o bucle local, es una interfaz virtual que se puede utilizar para acceder a servicios de red que se estén ofreciendo directamente en el mismo equipo. Por ejemplo, para acceder a una página web se pondría en el navegador del mismo equipo :fa-globe:http://127.0.0.1 o el alias :fa-globe:http://localhost
- enp0s3: Anteriormente se seguía la convención eth0 para la primer interfaz, eth1 para la segunda y así sucesivamente. Hoy se utiliza este tipo de convención al nombrar las interfaces debido a que ayuda a tener configuraciones persistentes pues se basa en la ubicación física de la tarjeta de red en la motherboard. En la salida del comando se puede ver información importante de la interfaz tal como:
	- Nombre de la interfaz: `enp0s3`
	- Dirección IP: `192.168.1.147`
	- Dirección MAC: `07:08:27: b0: 86:34`
	- Máscara de red: `255.255.255.0`

Para obtener mayor información sobre una interfaz ejecute el comando:

	ip a show enp0s3 #Adaptar al nombre de interfaz del equipo actual

El comando ip permite gestionar el estado de una interfaz. Se puede dar de baja/desactivar o alta/activar.

	sudo ip link set enp0s3 down # Deshabilitar la interfaz
	sudo ip link set enp0s3 up # Habilitar la interfaz

**Configuración estática**

Asignar una ip estática es primordial para un servidor. En Ubuntu 18.04 se utiliza netplan para la configuración.

Para configurar los parámetros de red edite el archivo `/etc/netplan/01-netcfg.yaml` (en algunos casos el archivo puede llamarse `50-cloud-init.yaml`)

Así se muestra el archivo original:

	# This file describes the network interfaces available on your system
	# For more information, see netplan(5).
	network:
	  version: 2
	  renderer: networkd
	  ethernets:
	    enp0s3:
	      dhcp4: yes

Guarde el archivo y ejecute el siguiente comando para aplicar cambios:

	sudo netplan apply

**Configuración de red temporal de forma manual**

Para asignar de manera temporal una ip a una interfaz se utiliza el comando

	sudo ip addr add 192.168.1.12/24 dev enp0s3 #modifique la interfaz, ip y su máscara a sus necesidades

Para borrar la ip asignada utilice el comando:

	sudo ip addr del 192.168.1.12/24 dev enp0s3

Definir la puerta de enlace:

	sudo ip route add default via 192.168.1.254

Eliminar puerta de enlace:

	sudo ip route del default

**Net-tools y el comando ifconfig para versiones anteriores de Ubuntu**

El comando `ifconfig` es parte del paquete `net-tools` y permite configurar y ver información de las interfaces de red. Si bien aún se incluye en la instalación estándar de Ubuntu, algunas distribuciones como Debian en su versión 9, han dejado de incluirla y está siendo reemplazada por el comando `ip` (utilería de la suite `iproute2`). En caso de querer utilizarlo y que no esté instalado, se puede instalar con:

	sudo apt-get install net-tools

**Para obtener información de las interfaces ejecute:**

`ifconfig`
`/sbin/ifconfig #solo si la opción anterior falla`

	enp0s3    Link encap:Ethernet  direcciónHW 07:08:27:b0:86:34  
	          Direc. inet:192.168.1.147  Difus.:192.168.1.255  Másc:255.255.255.0
	          ACTIVO DIFUSIÓN FUNCIONANDO MULTICAST  MTU:1500  Métrica:1
	          Paquetes RX:810 errores:0 perdidos:0 overruns:0 frame:0
	          Paquetes TX:383 errores:0 perdidos:0 overruns:0 carrier:0
	          colisiones:0 long.colaTX:1000 
	          Bytes RX:915447 (915.4 KB)  TX bytes:31814 (31.8 KB)
	
	lo        Link encap:Bucle local  
	          Direc. inet:127.0.0.1  Másc:255.0.0.0
	          ACTIVO BUCLE FUNCIONANDO  MTU:65536  Métrica:1
	          Paquetes RX:160 errores:0 perdidos:0 overruns:0 frame:0
	          Paquetes TX:160 errores:0 perdidos:0 overruns:0 carrier:0
	          colisiones:0 long.colaTX:1 
	          Bytes RX:11840 (11.8 KB)  TX bytes:11840 (11.8 KB)

El comando `ifconfig` también permite habilitar y deshabilitar una interfaz:

	sudo ifconfig enp0s3 down #deshabilitar
	sudo ifconfig enp0s3 up #habilitar

**Configurar parámetros de red temporales de forma manual**

Para asignar la ip y máscara de red a una interfaz se utiliza el comando:

	sudo ifconfig enp0s3 192.168.1.1 netmask 255.255.255.0
	sudo ifconfig enp0s3 192.168.1.1/24 #Usando formato CIDR

Para quitar la dirección ip a una interfaz:

	sudo ifconfig enp0s3 0

Por último, se debe definir el servidor dns y opcionalmente el dominio a utilizar. Para esto se edita el archivo `/etc/resolv.conf`. El archivo configurado quedaría de la siguiente manera (adapte los valores a su escenario):

	# Dynamic resolv.conf(5) file for glibc resolver(3) generated by resolvconf(8)
	#     DO NOT EDIT THIS FILE BY HAND -- YOUR CHANGES WILL BE OVERWRITTEN
	nameserver 8.8.8.8
	search midominio.lan