# Teoría de permisos

Los permisos básicos para cada archivo o directorio son: Lectura (`r`), escritura (`w`) y ejecución (`x`). Para ver los permisos y otra información de los archivos en un directorio utilice el comando:

    ls -lh
	-rw------- 1 ray ray       20K oct 12 12:12 privado.txt
	-rw-r--r-- 1 ray ray       41K oct 12 12:13 publico.txt
	-rw-rw-r-- 1 ray ventas   154K oct 12 12:13 reporte_ventas.txt
	-rwxr-x--- 1 ray ray       39K oct 12 12:13 script.sh
	-rw-r----- 1 ray sistemas  23K oct 12 12:14 servidores.txt

Cada linea está conformada por diferentes columnas.

- Tipo de objeto, permisos del usuario, grupo y otros. Ejemplo: `-rw-rw-r–`
- Número de links que apuntan al objeto. Ejemplo: `1`
- Propietario del archivo/directorio: Ejemplo: `ray`
- Grupo del archivo/directorio. Ejemplo: `ventas`
- Tamaño del archivo en bytes si no se usa la opción `-h`. Ejemplo `154K`
- Fecha y hora de última modificación. Ejemplo: `oct 12 12:13`
- Nombre del archivo. Ejemplo: `reporte_ventas.txt`

Los tipos de objetos son:

- `-`: archivo normal
- `d`: directorio
- `b`: dispositivo de bloque
- `c`: dispositivo de caracteres
- `l`: Enlace simbólico (liga/link)
- `p`: tubería nombrada [fifo](http://www.tldp.org/LDP/lpg/node16.html#SECTION00731000000000000000)). Archivo que se usa como un dispositivo especial para intercomunicación de procesos.
- `s`: [socket](https://es.wikipedia.org/wiki/Socket_Unix)

<table>
  <tr>
    <th></th>
    <th colspan="3" align="center" >Permisos para</th>
    <th colspan="4"></th> 
 </tr> 
 <tr>
    <th>Tipo de objeto</th>
    <th>Usuario</th>
    <th>grupo</th>
    <th>otros</th>
    <th>Descripción</th>
 </tr>
 <tr>
    <td>- (archivo texto)</td>
    <td>rw-</td>
    <td>rw-</td>
    <td>rw-</td>
    <td>Todos pueden leer/escribir</td>
 </tr> 
 <tr>
    <td>-</td>
    <td>rwx</td>
    <td>rwx</td>
     <td>rwx</td>
     <td>Permisos de ejecución, lectura y escritura para todos</td>
 </tr>
 <tr>
    <td>-</td>
    <td>rw</td>
    <td>rw-</td>
    <td>—</td>
    <td>El propietario y el grupo pueden leer/escribir en el archivo/td>
 </tr> 
 <tr>
    <td>-</td>
    <td>rw-</td>
    <td>—</td>
    <td>—</td>
    <td>Solo el propietario puede leer/escribir el archivo</td>
 </tr>
 <tr>
    <td>-</td>
    <td>rw-</td>
    <td>r–</td>
    <td>r–</td>
    <td>Todos puede leer, pero solo el propietario puede escribir en el archivo</td>
 </tr>
</table>

!> Importante: Normalmente es fácil entender que un archivo pueda ser leído, escrito o incluso que se pueda ejecutar, sin embargo cuando estos permisos aplican para directorios, el sentido varía un poco.

- `r`: Permite ver el contenido de un directorio. Ejemplo: `ls /directorio`
- `w`: Permite agregar o quitar archivos o directorios a un directorio. Ejemplo `cp a.txt /ruta/`, `rm /ruta/archivo.txt`
- `x`: Permite situarse en el directorio. Ejemplo: `cd /directorio`

### Cambiar permisos

Cada permiso tiene un valor entero: x=1, w=2 y r=4. Para asignar permisos se utiliza el comando `chmod` al que se le pasa como argumento la suma de los valores enteros que se desea otorgar. En caso de no otorgar ningún permiso, se pasa el valor 0 (cero). Esto se hace para el usuario, grupo y otros.

	chmod 644 abc.txt #Otorgar permiso de lectura a todos y de escritura solo al propietario.
	chmod 600 abc.txt #Solo el propietario puede leer/escribir
	chmod 750 script.sh #Otorga control total al propietario, el grupo puede ejecutar y leer el archivo, los demás no tienen ningún permiso.
	chmod 700 miDirectorio # Asignar permisos a un directorio.

Es posible dar permisos de forma recursiva a un directorio utilizando el comando `chmod -R 700 miDirectorio`, sin embargo, por la diferencia que existe en otorgar permisos a directorios y archivos, se podría otorgar erróneamente privilegios de ejecución a archivos que no deberían tenerlos. Para evitar eso se usarán los siguientes comandos:

	find /ruta/al/dir -type f -exec chmod 644 {} + #asignar permisos solo a archivos
	find /ruta/al/dir -type d -exec chmod 755 {} + #asignar permisos solo a directorios

### Cambiar propietario y grupo

Cuando se desea cambiar el propietario o el grupo a un archivo se utiliza el comando:

	sudo chown nuevoPropietario.NuevoGrupo archivo.txt
	sudo chown -R nuevoPropietario.NuevoGrupo directorio # Cambia el propietario/grupo del directorio y su contenido

### En caso de que solo se desee cambiar el grupo se puede usar el comando:

	sudo chgrp nuevoGrupo archivo
	sudo chgrp nuevoGrupo -R directorio

> :pencil: _**NOTA:**_ Por defecto, por cada usuario creado, se crea automáticamente un grupo con el mismo nombre




