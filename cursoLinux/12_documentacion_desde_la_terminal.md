# Documentación desde la terminal 

## Introducción 

Es muy difícil recordar siempre la syntaxis exacta y las opciones de cualquier comando de la terminal. En ocasiones solo queremos recordar esa opción que nos facilita el uso del comando en turno. Para esto siempre es recomendable revisar la documentación que se incluye por default en la terminal.

Casi todos los comandos tienen su documentación bajo alguna o varias de las opciones que aquí se mencionan, pero en caso de que no sea así, se puede consultar en Internet.

## Sintaxis general

Todo comando utiliza la siguiente sintaxis

	COMANDO [OPCIÓN1 | OPCIÓN2 ]... [ARGUMENTOS]... OTROS

Donde:

- `COMANDO` es el comando que se está consultando: `ls cp mv rm date cd`
- `[]` indica que lo que está dentro es opcional
- `|` indica que se debe usar la opción uno o dos, no ambas.
- `…` indica que se pueden incluir varias opciones: `-l -h -a -v` o `argumentos: archivo1 archivo2 archivo3 dir1 dir2 dir3`
- `OTROS` todo lo que va escrito fuera de corchetes, debe incluirse de forma obligada.

## Opciones para obtener documentación o ayuda

### Opción -h o --help

Es una ayuda básica sobre el funcionamiento de un comando, algunos comandos muestran información con ambas opciones y otros solo reconocen una de ellas.

	comando -h
	comando --help

Ejemplo

	cp --help

	Modo de empleo: cp [OPCIÓN]... ORIGEN DESTINO
 	 o bien:  cp [OPCIÓN]... ORIGEN... DIRECTORIO
	  o bien:  cp [OPCIÓN]... -t DIRECTORIO ORIGEN...
	Copia ORIGEN a DESTINO, o varios ORIGEN(es) a DIRECTORIO.
	
	Los argumentos obligatorios para las opciones largas son también obligatorios
	para las opciones cortas.
	  -a, --archive                lo mismo que -dR --preserve=all
	      --attributes-only        no copia los datos del fichero, solamente los
	                               atributos
	      --backup[=CONTROL]       crea una copia de seguridad de cada fichero de
	                               destino que exista
	  -b                           como --backup pero no acepta ningún argumento
	      --copy-contents          copia el contenido de los ficheros especiales
	                               cuando opera recursivamente
	  -d                           lo mismo que --no-dereference --preserve=link
	  -f, --force                  si un fichero de destino no se puede abrir, lo
                                   borra y lo intenta de nuevo (no se tiene en
                                   cuenta si se utiliza también la opción -n)

### Páginas del manual

Es documentación extensa que se divide en secciones como comandos, librerías, archivos de configuración, entre otros.

Las secciones son:

	1   Programas ejecutables y guiones del intérprete de órdenes
	2   Llamadas del sistema (funciones servidas por el núcleo)
	3   Llamadas de la biblioteca (funciones contenidas en las bibliotecas del sistema)
	4   Ficheros especiales (se encuentran generalmente en /dev)
	5   Formato de ficheros y convenios p.ej. I/etc/passwd
	6   Juegos
	7   Paquetes de macros y convenios p.ej. man(7), groff(7).
	8   Órdenes de admistración del sistema (generalmente solo son para root)
	9   Rutinas del núcleo [No es estándar]
	n   nuevo [obsoleto]
	l   local [obsoleto]
	p   público [obsoleto]
	o   old [obsoleto]

Ejemplo:

	man cp #muestra la página del manual del comando cp
	man -k copy #busca en las páginas del manual el patrón copy y devuelve registros que coincidan con dicho patrón

Para ver una sección especifica se utiliza:

	man passwd # manual del comando para cambiar la contraseña de un usuario
	man 5 passwd #manual del archivo /etc/passwd

### Paquetes de documentación

Muchos programas incluyen paquetes de documentación especial, que incluye cosas como la licencia, ejemplos, información sobre la instalación, entre otros.

Para buscar si existen paquetes de documentación de un programa en especifico, se puede hacer uso de un gestor de paquetes como `apt`

	apt search doc$ | grep postgres

La documentación de los paquetes normalmente se encuentra en el directorio `/usr/share/doc/PAQUETE` y para acceder a la misma se utiliza el comando `less` o, si viene comprimida con `gz`, `zless` 