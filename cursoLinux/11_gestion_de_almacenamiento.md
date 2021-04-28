# Gestión del almacenamiento

## Introducción

Llevar un buen control del almacenamiento de un servidor es importante, pues muchas cosas pueden empezar a fallar si no hay espacio en disco. Linux provee herramientas que nos ayudan a llevar una buena gestión del almacenamiento. Cuando hay problemas de espacio, es necesario saber en dónde se tienen, qué es lo que está consumiendo e incluso, agregar dispositivos adicionales.

## Análisis del almacenamiento 

### Ver uso de disco 

Para mostrar el uso de disco de los sistemas de ficheros se usa el siguiente comando

    df -h

    S.ficheros     Tamaño Usados  Disp Uso% Montado en
    /dev/nvme0n1p4   167G    12G  148G   8% /
    /dev/nvme0n1p1   511M   3.5M  508M   1% /boot/efi
    /dev/sda1        917G   144G  773G  16% /media/israel/ExtraDrive1

Es importante mencionar que el comando muestra una vista general de cada sistema de ficheros. Es útil cuando se quiere ver una lista de los volúmenes montados y cuanto espacio está consumiendo cada uno. La opción `-h` es útil porque sin ella, por defecto se muestra la información en bytes.

Se puede dar el escenario en que una aplicación informe que el disco está lleno aunque haya espacio libre en disco según `df`, en este caso se debe poner atención a los [inodos](https://es.wikipedia.org/wiki/Inodo) disponibles. Este caso puede ocurrir en servidores de correos con un gran núnero de archivos, que han ocasionado que se alcance el límite máximo (muy difícil de alcanzar en escenarios comunes). En este caso es necesario el monitoreo de los inodos mediante el comando:

	df -i

Al hacer el análisis del origen del consumo de discos más a detalle, revisar el peso de los directorios puede ayudar a identificar al responsable… Para esto use el comando

### Peso de directorios

	du -h directorio

Una herramienta que no viene instalada por defecto en Ubuntu, pero que es de gran ayuda es ncdu. Se instala con:

	sudo apt-get install ncdu

Uso:

	ncdu carpeta

    ncdu 1.11 ~ Use the arrow keys to navigate, press ? for help                                                                                                                                  
    /media/israel/ExtraDrive1/Descargas
    --------------------------------------------------------------
    11.1 GiB [##########] /Isos
    1.7 GiB [#         ] /pfs-vpn
    9.6 MiB [          ]  shallalist.tar.gz

### Peso de archivos 

Para ver el tamaño de los archivos dentro de un directorio, ordenados por tamaño en forma descendente, se usa el comando:

	ls -lhS

## Añadir almacenamiento

En ocasiones es necesario añadir almacenamiento adicional a servidores físicos o virtuales, para esto es necesario saber el nombre del nuevo dispositivo, formatearlo y montarlo. Al agregar un dispositivo (disco duro interno, externo o usb) normalmente se identifica como `/dev/sda` para el primer dispositivo, `/dev/sdb` para el segundo y así sucesivamente. En otros casos, al usar discos virtuales, se pueden identificar como `/dev/vda` o /`dev/xda u otros`.

### Ver dispositivos de almacenamiento detectados

Para ver info de los dispositivos se usa el comando:

	sudo fdisk -l

	Disk /dev/sda: 931.5 GiB, 1000204886016 bytes, 1953525168 sectors
	Units: sectors of 1 * 512 = 512 bytes
	Sector size (logical/physical): 512 bytes / 4096 bytes
	I/O size (minimum/optimal): 4096 bytes / 4096 bytes
	Disklabel type: gpt
	Disk identifier: 48D0EDAB-DD0D-4029-9173-D07C5F612551

	Disposit.  Start      Final   Sectores   Size Tipo
	/dev/sda1   2048 1953523711 1953521664 931.5G Linux filesystem

Otro comando que ayuda a determinar los dispositivos en el sistema y no necesita `sudo` es:

	lsblk

	NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
	sda           8:0    0 931.5G  0 disk 
	└─sda1        8:1    0 931.5G  0 part /media/israel/ExtraDrive1

> :pencil: _**NOTA:**_ En este caso se muestra el dispositivo `sda` y su primera partición `sda1`. En caso de que el disco tuviese más particiones, se mostrarían como `sda2`, `sda3`… `sdaN`. Lo mismo pasaría con un segundo disco: sdb1, sdb2… sdbN. El número de particiones primarias para tablas `DOS (MBR)` son 4 y solo puede utilizarse en discos duros de hasta 2TB, `GPT` por el contrario, no tiene estas restricciones, es por eso que se está convirtiendo en el estándar por defecto y es el que se recomienda implementar.

### Particionar el dispositivo

El comando `fdisk` permite también dar formato a un dispositivo. 

!> Se debe tener mucho cuidado al utilizarlo. Si se selecciona el disco erróneo, hay muchas posibilidades que se destruya toda su información.

Su sintaxis es la siguiente:

	sudo fdisk /dev/sdb #Nuevamente se debe revisar que realmente está usando el dispositivo deseado

Los comandos básicos para `fdisk` son:

	p imprime la tabla de particiones
	n crea una nueva partición
	d borra una partición
	q sale sin guardar cambios
	w escribe cambios y sale
	m imprime la ayuda

Los cambios no son escritos hasta ejecutar el comando `w`.

`cfdisk` es otra herramienta para realizar el particionado que es más sencilla de utilizar, por proveer menús que se navegan a través del teclado.

### Dar formato al dispositivo

Dar formato en este caso significa crear un sistema de archivos en la partición del dispositivo. Existen gran cantidad de sistemas de archivos, pero se recomienda utilizar `EXT4` para uso general y `XFS` cuando se manejan archivos individuales de gran tamaño o en servidores de base de datos debido a su buen performance.

Para dar formato a /dev/sdb1 con EXT4 como sistema de archivos use:

	sudo mkfs.ext4 /dev/sdb1

Para hacerlo usando XFS utilice:

	sudo mkfs.xfs /dev/sdb1

### Montar o desmontar el volumen

Para empezar a utilizar el volumen (`sdb1`) es necesario montarlo. En Linux, el directorio para montar es `/mnt`. Es posible crear subcarpetas dentro de este directorio en caso de montar diferentes volúmenes. El directorio debe estar vacío.

	sudo mount /dev/sdb1 /mnt #montado directo sobre mnt
	sudo mount /dev/sdb1 /mnt/misDatos #montado sobre un subidrectorio el cual debe existir previamente.
	sudo mount /dev/sdb2 /mnt/otrosDatos #note que es otra partición y se monta en otro subdirectorio bajo mnt

Para desmontar el volumen, es necesario que este no se esté utilizando por ningún comando y no estar ahí (cd /mnt/mivolumen/).

	sudo umount /mnt
	sudo umount /mnt/misDatos

## FSTAB

`/etc/fstab` se utiliza para especificar los dispositivos que se deben montar (y su punto de montaje) al arranque del sistema. Principalmente se utiliza para montar el sistema de archivos principal (`/,/boot`) por lo que se debe tener mucho cuidado puesto que un error al editarlo, puede causar que el sistema no arranque.


	# <file system> <mount point>   <type>  <options>       <dump>  <pass>
	# / was on /dev/nvme0n1p4 during installation
	UUID=fa46799a-0c2a-49b4-8c7e-1... /               ext4    noatime,errors=remount-ro 0       1
	# /boot/efi was on /dev/nvme0n1p1 during installation
	UUID=B198-...  /boot/efi       vfat    umask=0077      0       1
	# swap was on /dev/nvme0n1p3 during installation
	UUID=682085d8-162a-442a-9e29-... none            swap    sw              0       0

El `UUID` es un identificador que se crea para cada volumen y se prefiere su uso en comparación con el método de definir utilizando la ruta al dispositivo (`/dev/sdX#`), puesto que en un cambio físico en las conexiones (tarjeta madre) esta ruta puede cambiar y generar comportamientos no deseados. Para obtener el `UUID` se usa el comando:

	blkid

Cada linea dentro de `fstab` se compone de diferentes columnas. Se explica cada columna de izquierda a derecha:

1. `UUID` de cada volúmen
2. especifica el punto de montaje del dispositivo. `/` para el sistema de archivos raíz. Swap no tiene punto de montaje (`none`).
3. tipo de sistema de archivos: EXT4, XFS, swap, etc…
4. Opciones: `errors=remount-ro` en caso de error, vuelve a montar en modo solo lectura. `sw` para swap
5. Dump: su valor puede ser `0` o `1` y determina si el sistema puede ser respaldado (1) o no (0). Hoy en día no se usa y se recomienda dejar su valor en 0.
6. Pass: Orden en el cual se revisará el sistema de archivos (comando `fsck`).
	- a. 0: no se revisa.
	- b. 1: Se revisa con prioridad. Opción recomendada para `/`
	- c. 2: Se revisa como segunda prioridad. Opción recomendada para todo lo demás.

Algunas de las opciones útiles usadas en la cuarta columna de `fstab` son:

- `rw`: montar en modo lectura y escritura
- `exec`: permite que los archivos se ejecuten como programas
- `auto:` montar automáticamente al arranque
- `nouser`: solo el usuario root puede montarlo.
- `async`: la salida hacia el dispositivo será asíncrona.