# Raid por software con mdadm

## Introducción

Raid (arreglo de discos) es una tecnología muy utilizada en ambientes empresariales para reducir el “downtime” de un servidor por algún fallo en el disco duro. Aunque existen varios niveles de arreglos, para despliegues de base de datos se recomienda utilizar Raid 10 (requiere al menos 4 discos duros) o mínimo Raid 1 (requiere al menos 2 discos duros). Consulta [Wikipedia](https://es.wikipedia.org/wiki/RAID) para información sobre los diferentes niveles.

En estas guías se usará RAID por software utilizando Linux. Esta solución es asequible y confiable, sin embargo, soluciones basadas en hardware mejoran el rendimiento del sistema y facilitan tareas de mantenimiento como el reemplazar un disco, pero tienen un costo considerablemente elevado.

!> Importante: Raid no debe ser considerado como una tecnología de respaldos, puesto que existen diversos escenarios contra los que la información se puede perder en todos los discos. Ejemplos:

- Robo del equipo
- Desastres naturales
- Error en el software o el hardware que gestionan el arreglo.

**Siempre** se deben sacar respaldos de la información y tenerlos en un lugar seguro.

## Guías

### Raid 1 por software

#### Introducción 
En esta guía se muestra como crear un RAID1 sin herramientas gráficas u opciones durante la instalación.

El escenario que se usa para está guía se tomó de un caso real al configurar un servidor en OVH que tiene 2 discos SSD (480GB) y 2 discos SATA (4TB). OVH por default toma los discos SSD para instalar el sistema operativo, por lo que el ejemplo se hará usando los discos SATA de 4TB (sdc y sdd)

#### Instalar mdadm  

	sudo apt-get update && apt-get install mdadm

#### Configurar discos y crear RAID1 

En este ejemplo se tuvo que usar la herramienta `cfdisk` para preparar los discos duros, ya que es necesario crear una tabla de particiones `GPT` por el tamaño de las particiones a usar.

```bash
sudo cfdisk /dev/sdc
```
Una vez dentro de la utilería `cfdisk` los pasos a seguir son:

1. Crear una nueva tabla de particiones (GPT)
2. Crear una nueva partición de máximo el 90% del tamaño total del DD. El tamaño de la partición puede diferir en base al uso que se le quiera dar a los discos. Es posible inclusive, crear un esquema de particionado tan complejo como sea necesario, sin embargo en este caso se maneja el escenario más sencillo: 1 disco duro > 1 partición.
3. Definir el tipo de la partición como "Linux Raid"
4. Escribir los cambios.
5. REPETIR LOS PASOS PARA /dev/sdd

#### Limpiar configuraciones previas de RAID (en caso de existir) 

	sudo mdadm --zero-superblock /dev/sdc /dev/sdd

#### Crear el RAID1 usando `sdc1` y `sdd1` 

!> Importante: Se debe especificar manualmente el número de dispositivo `md` a utilizar. Si ya están creados dispositivos `md` (ejemplo  `md1` y `md5`), use `md` más el número siguiente (en el ejemplo sería md6).

```bash
mdadm --create /dev/md6 --level=1 --raid-devices=2 /dev/sdc1 /dev/sdd1
```

#### Dar formato al nuevo dispositivo 

	sudo mkfs.ext3 /dev/md6

#### Montar el dispositivo 

```bash
sudo mkdir /media/raid1
sudo mount /dev/md6 /media/raid1
df -H
```

#### Actualizar el archivo `/etc/mdadm.conf`

Obtener el string de configuración para nuestro dispositivo `md6`:

```bash
mdadm --detail --scan

ARRAY /dev/md1 metadata=0.90 UUID=549613d1:6924cc28:a4d2adc2:26fd5302
ARRAY /dev/md5 metadata=0.90 UUID=4356ee61:049d6ba1:a4d2adc2:26fd5302
ARRAY /dev/md6 metadata=1.2 name=pve-sc:6 UUID=048f9c30:b1ad975f:ab59b74b:b3c068c6  # Copiar esta linea al final del archivo mdadm.conf
```

Posteriormente se debe:
1. Editar el archivo.
2. Agregar la linea correspondiente al dispositivo `md` que esté configurando (`md6` en este ejemplo).
3. Guardar y salir del archivo.

#### Editar fstab para el montado automático al arranque. 
Agregue la siguiente linea al archivo `/etc/fstab` (no olvide ajustar el número de dispositivo y punto de montaje):

	/dev/md6 /media/raid1 ext3 noatime,rw 0 0

!> Importante: Siempre es peligroso modificar el archivo `fstab` puesto que una sintaxis erronea, puede ocasionar que el servidor no arranque. Siempre hay que revisar **detenidamente** la sintaxis y si es posible, en vez de reiniciar el equipo, ejecutar el comando `sudo mount -a`

> :pencil: _**NOTA:**_ En caso de que el sistema no actualice los cambios al particionar el disco (detectar las nuevas particiones), será necesario correr el siguiente comando: `sudo partprobe`

### Reemplazar un disco defectuoso

#### Introducción 

En esta guía se muestra como reemplazar un disco defectuoso que forma parte de un Raid 1 (software).

Se usará el siguiente escenario:
- `/dev/sda1` y `/dev/sdb1` crean el arreglo `/dev/md0` (RAID1).
- `/dev/sda2` y `/dev/sdb2` crean el arreglo `/dev/md1` (RAID1).
- `/dev/sdb` ha fallado y se reemplazará.

#### Cómo saber que ha fallado un disco? 

Es posible revisar el estatus de los dispositivos md revisando el fichero mdstat con el siguiente comando: `sudo cat /proc/mdstat`

Si al final de una de las lineas se ve `[U_]` o `[_U]` en vez de `[UU]`, quiere decir que ha fallado un disco. Probablemente la falla puede ser física, pero en algunos casos también puede ser lógica. En este último caso, el fallo se puede solucionar volviendo a añadir, a través de comandos, el dispositivo que marca la falla.

#### Remover el disco con falla 

Para remover el disco (`sdb` en este caso) es necesario marcar `sdb1` y `sdb2` como defectuosos, esto se hace con:
	
	mdadm --manage /dev/md0 --fail /dev/sdb1

Posteriormente se remueve `sdb1` de `md0`:

	mdadm --manage /dev/md0 --remove /dev/sdb1

Se repiten los pasos para `sdb2` que pertenece a `md1`.

	mdadm --manage /dev/md1 --fail /dev/sdb2
	mdadm --manage /dev/md1 --remove /dev/sdb2
  
Una vez ejecutado lo anterior, se debe apagar el sistema:
  
	sudo poweroff

Se reemplazaría físicamente el disco defectuoso por uno nuevo. El nuevo disco debe ser _de al menos_ el mismo tamaño que el anterior. Si el disco es mayor, el espacio sobrante no se usará.

#### Añadir el nuevo disco 

Una vez se haya reemplazado el disco, encienda el equipo.

Se debe crear el mismo esquema de particionado que tiene sda. Esto se logra con este comando:
  
	sfdisk -d /dev/sda | sfdisk /dev/sdb

Se puede corroborar que tengan el mismo esquema usando:

	fdisk -l
  
Para añadir cada partición a sus dispositivos se ejecuta lo siguiente:

	mdadm --manage /dev/md0 --add /dev/sdb1
	mdadm --manage /dev/md1 --add /dev/sdb2

Para ver el proceso de sincronización entre ambos discos ejecute:
  
	watch -n1 cat /proc/mdstat

Presione `ctrl+c` para cancelar el monitoreo.

Durante la sincronización el archivo `mdstat` se verá similar a esta salida:
```bash
Personalities : [linear] [multipath] [raid0] [raid1] [raid5] [raid4] [raid6] [raid10]
md0 : active raid1 sda1 sdb1
24418688 blocks [2/1] [U_]
[=>...................] recovery = 9.9% (2423168/24418688) finish=2.8min speed=127535K/sec

md1 : active raid1 sda2 sdb2
24418688 blocks [2/1] [U_]
[=>...................] recovery = 6.4% (1572096/24418688) finish=1.9min speed=196512K/sec

unused devices: <none>
```

Una vez terminada la sincronización, el archivo se verá similar a:
```bash
Personalities : [linear] [multipath] [raid0] [raid1] [raid5] [raid4] [raid6] [raid10]
md0 : active raid1 sda1 sdb1
24418688 blocks [2/2] [UU]

md1 : active raid1 sda2 sdb2
24418688 blocks [2/2] [UU]

unused devices: <none>
```