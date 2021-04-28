# Redireccionamientos, pipas y flujos.

## File descriptors

El kernel maneja todo el I/O (input/output) (archivos, sockets, pipas) utilizando un mecanismo llamado `file descriptor`. Existen varios `file descriptors`, pero este documento se centra en 3 que aplican al uso de los comandos: `0 (stdin), 1 (stdout) y 2 (stderr).`

Normalmente `stdin` proviene del teclado y del ratón, `stdout` y `stderr` son enviados al monitor, pero pueden ir a otros lados como a una impresora o un archivo.

Un ejemplo de `stdout` 

	ls /sys/class/net
	enp2s0f1  lo  vboxnet0  wlp3s0

Un ejemplo de `stderr `(ejecutando el comando como un usuario normal):

	ls /root
	ls: no se puede abrir el directorio '/root': Permiso denegado

## Flujos

Los flujos de información que tiene cada comando se pueden representar de la siguiente forma:

	stdin > COMANDO > stdout  
				v
    		  stderr

## Redireccionamientos

Un redireccionamiento se da cuando un `file descriptor` es asignado a un destino diferente (normalmente la pantalla). Esto es útil cuando se desea guardar el resultado de `stdout` o `stderr` hacia una bitácora para tener referencia del resultado del comando.

### Redireccionar stdin

	wc < archivo.txt

Lo anterior toma el contenido de `archivo.txt `y lo pasa al comando `wc`, el cual regresa el número de lineas, palabras y el conteo de bytes que contiene `archivo.txt`.

### Redireccionar stdout

	echo "Hola mundo" > log.txt

Este comando redirecciona `Hola mundo` para que sea guardado en el archivo `log.txt`. 

!> Importante: El caracter (mayor que) `>` creará el archivo `log.txt` en caso de que no exista, pero en caso de existir, utilizar un solo `>` ocasionará que el contenido del archivo sea sobrescrito por la salida del comando utilizado, (`echo` en este ejemplo).

### Redireccionamiento con apertura del destino

	echo "Hola mundo" >> log.txt

Lo anterior, creará el archivo` log.txt` con el contenido `Hola mundo`, en caso de que no exista; pero si existe, `aperturará` el archivo para poner `Hola mundo` en la última linea.

### Redirección selectiva

	gcc ejemplo.c 2> error

En este ejemplo solo se guarda en el archivo `error` aquellos errores que resulten del proceso de compilación. Por default se usa `>` para redireccionar `stdout `y `2>` para el `stderr`

	find /etc -name "*.conf" > encontrados.txt 2> /dev/null

El comando anterior guarda los resultados en el archivo `encontrados.txt`. Si existen errores en el proceso, estos son redirigidos a `/dev/null`, el cual se utiliza para descartar los errores.

	find /etc -name "*.conf" &> log.txt

Guarda `stdout` y `stderr` en el archivo `log.txt`

	find /etc -name "*.conf" > log.txt 2>&1

Guarda `stdout` hacia `log.txt` y manda stderr hacia donde `stdout` está apuntando (`log.txt` en este caso).

## Pipas

El uso de pipas `|` permite redirigir la salida de un comando hacia otro comando, con esto es posible combinar varios comandos, facilitando así el conseguir algunos objetivos.

La sintaxis general es la siguiente:

	COMANDO | COMANDO2 | COMANDO3...

	ls /etc | less

Se usa el paginador `less` para poder mostrar de manera cómoda, la salida de ls sobre el directorio `/etc`, el cual contiene muchos archivos. De esta manera evitamos tener que crear un archivo para poder conseguir un resultado similar. Mismo ejemplo sin el uso de pipas.

	ls /etc > /tmp/x
	less /tmp/x

### Ejemplos

Usando un editor de textos como `vi `o el comando `echo` y `» nombres.txt` cree el archivo `nombres.txt `con el siguiente contenido:

```txt
Merari
Fernanda
Yanira
Laura
Josefina
Gabriela
Jaime
Magdalena
Enedina
Juana
Yanira
Jaime
Yolanda
Israel
Benito
Merari
Claudia
Carlos
Alberto
Israel
Francisco
Martha
Laura 
```

 Como se puede ver, el contenido del archivo está desordenado. Es posible ordernarlo mediante:

	cat /tmp/nombres.txt | sort

Se puede ver que el archivo tiene nombres duplicados, podemos omitir los duplicados con:

	cat /tmp/nombres.txt | sort | uniq  

 Una vez que el listado de nombre es único, se puede pasar esta última salida al comando `wc` para saber el número de nombres únicos.

	cat /tmp/nombres.txt | sort | uniq | wc

Para saber cuantos caracteres tiene un archivo, por ejemplo` lorem.ipsump`, se usa:

	cat lorem.ipsum | wc -c

 Saber cuantas palabras:

	cat lorem.ipsum | wc -w

Obtener un listado de los 10 archivos mas pesados de un directorio:

	ls -lhS /var/log | head -10

Respaldar carpetas:

	tar -cv /etc/ | gzip > /tmp/etc.tar.gz