# Volúmenes lógicos

Gestionar nuestros servidores con volúmenes lógicos, en lugar de particiones estándar, da una flexibilidad en el manejo de espacio de cada partición. En caso de necesitar más espacio, es posible agregar nuevos discos duros, agregarlos al grupo de volúmenes y fusionar el espacio de los discos.

!> Una de las excepciones para usar volúmenes lógicos es la partición de boot, debido a que algunas versiones viejas de grub no entienden la partición LVM y el sistema por tanto no inicia.

## Conceptos básicos

- Volumen físico: discos duros completos o particiones de disco duro con formato del tipo LVM.
- Grupo de volúmenes: Está conformado por uno o más volúmenes físicos. Trabaja como un disco virtual que puede crecer de tamaño conforme se agreguen más volúmenes físicos.
- Volumen lógico: Similar a una partición, se le puede dar formato con un sistema de ficheros (ext3/4, xfs, etc).

## Configuración con discos nuevos

En este ejemplo, se asume que se tiene un servidor ya instalado. A este servidor se le agregaron 2 discos duros que crearán un volumen lógico en donde se guardarán respaldos.

Los discos extra serán:

- `/dev/sdb`
- `/dev/sdc`

### Crear el volumen físico

	$ sudo pvcreate /dev/sdb
	$ sudo pvcreate /dev/sdc

### Crear el grupo de volúmenes

Se recomienda que siempre inicie por vg y que esté seguido por una descripción que ayude a entender la finalidad del grupo de volúmenes.

	$ vgcreate vg-respaldo /dev/sdb

### Crear el volumen lógico

> :pencil: _**NOTA:**_ Como una característica de los volúmenes lógicos es poder crecer el mismo con relativa facilidad, y a fin de que este ejercicio muestre dicha característica, no se usa en principio el 100% de la capacidad del disco.

	$ lvcreate -n respaldos -L 2g vg-respaldo

#### Descripción de parámetros
- `-n`: indica el nombre del volumen
- `-L`: indica el tamaño en gigas, megas, etc
- `vg-respaldo`: grupo de volúmenes al cual será asignado este volumen lógico.

#### Dar formato al volumen

Para esto hay que localizar el nombre completo del volumen.

	$ ls /dev/mapper

Los volúmenes son listados dentro de `/dev/mapper/`. En este caso el volumen estará en `/dev/mapper/vg–respaldo-respaldos` este valor se usará en el siguiente paso.

#### Agregar un sistema de ficheros al volumen.

	$ sudo mkfs.ext4 /dev/mapper/vg--respaldo-respaldos

#### Montar el nuevo volumen

	$ mount /dev/mapper/vg--respaldo-respaldos /mnt/

## Trabajando con volúmenes existentes

### Volumen Lógico

#### Extender el volumen lógico

Actualmente se tiene un volumen de `2GB`, sin embargo este espacio no es suficiente en la mayoría de escenarios de hoy en día. Para extender un volumen se usa el siguiente comando:

	$ sudo lvextend -n /dev/vg-respaldo/respaldos -l 100%FREE

En este caso se extiende al `100%` del espacio disponible del grupo de volúmenes. Se puede hacer uso de la opción `-L` para especificar por megas o gigas.

#### Extender el sistema de archivos

!> Importante: Aunque se ha extendido el volumen lógico, esto no se verá reflejado en el sistema (se puede comprobar con un `df -h`) hasta que también se extienda el sistema de archivos. `EXT3` y `EXT4` en teoría se pueden extender sin la necesidad de ser desmontado, sin embargo es importante evaluar la situación para saber si esto es conveniente o no.

> :pencil: _**NOTA:**_ para un sistema ext3/4 se usa el comando **resize2fs**, sin embargo para sistemas con `XFS` se usa `xfs_growfs`

	$ resize2fs /dev/mapper/vg--respaldo-respaldos

### Grupo de volúmenes

#### Extender un grupo de volúmenes

Siguiendo el escenario propuesto, se usa el disco restante para obtener su capacidad extra.

	$ sudo vgextend vg-respaldo /dev/sdc

#### Extender el volumen lógico

Por poner una variante, se extiende solo a `10Gigas`

	$ sudo lvextend -L +10g /dev/vg-respaldo/respaldos

## Tips

### Reducir partición `Swap` que está sobre un volúmen lógico con `LVM2`

El particionado automático de Ubuntu Server crea una partición para swap del mismo tamaño de la memoria RAM. Cuando se tiene mucha ram y poco espacio en disco, esto no es lo más conveniente pues se dedica mucho espacio del disco duro a la partición de Swap.

Asumiendo que el volumen a reducir es `/dev/VolGroup00/LogVol01`, se siguen los siguientes pasos.

#### Apagar la swap del volumen lógico

	$ swapoff -v /dev/VolGroup00/LogVol01

#### Reducir el volumen lógico 512MB

	$ lvm lvreduce /dev/VolGroup00/LogVol01 -L -512M

#### Formatear el nuevo espacio para swap

	$ mkswap /dev/VolGroup00/LogVol01

#### Habilitar el nuevo volumen lógico

	$ swapon -va

#### Revisar que el volumen se haya reducido correctamente

	$ cat /proc/swaps