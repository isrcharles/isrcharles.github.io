| Comandos básicos|
|------------|
|__ls: Muestra el contenido de un directorio__|
|   Ejemplo: ls /tmp|
| Opciones:|
| -a muestra archivos ocultos|
|-R lista recursiva sobre directorios|
|-r lista en orden inverso|
|-t Ordena en base al último archivo modificado|
|-S Ordena por archivo más pesado|
|-l formato largo (muestra más info de cada archivo/dir)|
|-1 uno por linea|
|-m listado separado por comas|
|__cd: cambia de directorio__|
| Ejemplo: cd /tmp|
| Opciones:|
| - cambia a ubicación anterior|
|.. sube un nivel (directorio anterior)|
| ~ cambia al home del usuariop|
|__cp: copia archivos y directorios__|
| Ejemplo: cp miarchivo.txt /tmp/|
| Ejemplo: cp miarchivo.txt /tmp/otronombre.txt|
| Ejemplo: cp miarchivo.txt micopia.txt|
| Opciones:|
| -r copia directorios|
|-a copia preservando atributos y permisos |
| -v copia mostrando en pantalla el archivo en turno|
|-auv igual que -a + -v pero solo copia los archivos que cambiaron|
|__mv: renombra un archivo o lo mueve de ubicación__|
|Ejemplo: mv archivo.txt fichero.txt|
|Ejemplo: mv archivo.txt /otra/ruta/otro-nombre.txt|
|mkdir: crea directorios|
|Ejemplo: mkdir nuevodir|
|Opciones:|
|-p crea directorios incluyendo todos aquellos niveles superiores que hagan falta|
|touch: actualiza la hora de acceso y modificación de archivos. Si no existe, crea un archivo vacio.|
|Ejemplo: touch archivo.txt|
|__rmdir: borra directorios vacios__|
|Ejemplo: rmdir directorioVacio|
|__rm: borra archivos__|
|Ejemplo: rm archivo.txt|
|-f forza el borrado del archivo|
|-r borra directorios recursivamente. ¡Usar con cuidado!|
|__cat: muestra el contenido de un archivo__|
|__more: muestra el contenido de un archivo usando paginación__|
|__less: igual que more, pero permite avanzar y retroceder en el archivo__|
|__tail: muestra las últimas lineas de un archivo__|
|Opciones:|
|-f muestra lo que se añade al archivo en tiempo real|
|-n en conjunto con -f, especifica cuantas lineas mostrar|
|__head: muestra las primeras lineas de un archivo__|
|__pwd: imprime el directorio de trabajo (actual)__|
|__who: Muestra los usuarios conectados al sistema__|
|__whoami: muestra la cuenta de usuario que se usa actualmente__|
|__date: muestra la fecha y hora del sistema__|
|Opciones:|
|+%H imprime la hora actual del sistema|
|+%M imprime el minuto actual del sistema|
|+%Y imprime el año actual del sistema|
|+%d imprime el día actual del sistema|
|+%m imprime el mes actual del sistema|
|__uptime: Muestra el tiempo que el servidor lleva encendido sin interrupciones__|
|__su: cambia el usuario__|
|Ejemplo: su - otrousuario |
|Opciones:|
|-l USUARIO inicializa el entorno como si el usuario se hubiese logueado directamente|
|-c COMANDO ejecuta el comando especificado|
|__exit: cierra la sesión actual__|
|__find: busca archivos en el arbol de directorios__|
|Ejemplo: find /ruta/de/busqueda -name miarchivo.txt #busca por nombre|
|Ejemplo: find /ruta/de/busqueda -perm 777 #busqueda de archivos con todos los permisos asignados|
|Ejemplo: find /ruta/de/busqueda -size +100MB #busca archivos mayores a 100MB|
|Ejemplo: find /ruta/de/busqueda -size +100MB-exec rm {} \; #borrar archivos mayores a 100MB|
|__man comando: muestra el manual del comando__|


