# Shell scripting

## Introducción

Crear scripts de shell ayuda al administrador a realizar tareas complejas con la ayuda de los diferentes comandos del sistema en conjunto con conceptos como `quoting`, `expansiones de shell/comodines`, `expresiones regulares`, etc. En esta introducción se utilizarán diferentes ejemplos que se irán explicando para comprender los conceptos básicos.

## Hola mundo en bash

Cree el archivo `holaMundo.sh` con `vi` y asigne permisos de ejecución con `chmod`. El contenido del archivo es el siguiente:

	#!/bin/bash
	echo "¡Hola mundo!"

Del anterior ejemplo se determina que los script de shell:

- Son archivos de texto que contienen comandos (`echo` en este caso)
- Deben ser ejecutables. Se recomiendan permisos `700` (solo el propietario puede leer/escribir/ejecutar el script)
- Por convención, se utiliza la extensión `.sh` al nombrarlos.
- La primer linea `#!/bin/bash` se le conoce como la linea `shebang`. La combinación de caracteres `#` y `!` es llamada el número mágico. Se utiliza para indicar el shell que se utilizará en el script (`/bin/bash` en este caso). Todo script debe iniciar siempre con esta linea.

Ejecución del script

Método 1

	$ ./holaMundo.sh # en este ejemplo se debe estar en la misma ruta que el script
	$ /ruta/al/script/holaMundo.sh # O se puede ejecutar si se sigue el Path completo.

Método 2

	$ /bin/bash holaMundo.sh

## Script para respaldar el home de un usuario

	#!/bin/bash
	tar -czf /mnt/my-backup.tgz /home/ray

El ejemplo anterior es un script muy sencillo pues solo utiliza un comando, pero es útil cuando dicho comando puede ser complejo y se desea simplificar su uso. El problema está en que su uso está muy limitado, puesto que si se desea respaldar el home de otro usuario, se tendría que cambiar manualmente el valor `/home/ray` y el nombre o la ruta del respaldo nuevo pues de lo contrario, se reemplazaría el anterior. Para dar mayor flexibilidad, se utilizarán `variables de entorno`.

Las variables de entorno muestran información variada. Se puede ver las variables y sus valores al ejecutar el comando `env`. Para ver el contenido de una variable en específico (HOME en este caso) se utiliza `$` precediendo a la variable:

`$ echo $HOME`
`/home/ray`  

	#!/bin/bash
	#Script para respaldar el home de un usuario 
	#V2 usa variables de entorno para respaldar el home del usuario que ejecute el script
	tar -cZf /mnt/my-backup.tgz $HOME 

Todavía se tiene el problema que el nombre del respaldo es estático y se reemplazaría el respaldo anterior cada vez que se ejecute. Para solucionar esto se puede agregar la fecha al nombre del respaldo:

	#!/bin/bash
	#Script para respaldar el home de un usuario 
	#V3 Agregar fecha al nombre del respaldo para poder mantener varios respaldos
	tar -cZf /mnt/my-backup-$(date +%d%m%Y).tgz $HOME 

`$(date +%d%m%Y)` se convierte en el valor de la salida del comando `date` la cual se modifica para que solo muestre el valor del día, mes y año actual.

Si se desea hacer aún más flexible el script, es posible crear variables para definir el directorio de respaldo a utilizar. También será necesario validar que dicho directorio exista. Para esto se utilizarán condicionales mediante `if then fi`.

	#!/bin/bash
	#Script para respaldar el home de un usuario 
	#V4 definir directorio de respaldos y validar su existencia
	DIRTRABAJO=/mnt/respaldos/
	
	if [ ! -d $DIRTRABAJO ]; then # observe el espacio entre la condición y los corchetes
	  echo "Error: el directorio de trabajo no existe"
	  exit 1 # exit 0 sale correctamente. exit 1 sale indicando error.
	fi;
	
	tar -cZf $DIRTRABAJO/my-backup-$(date +%d%m%Y).tgz $HOME 

La condición se lee: “Si no existe el directorio de trabajo, entonces informar del error y salir del script”. Los elementos de condición son:

- `if`: indica el inicio de un bloque con una condición, el cual debe cumplirse para ejecutar el código de las lineas siguientes hasta encontrar su correspondiente `fi`.
- `[ ! -d $DIRTRABAJO ]`: es la condición.
	- Siempre debe existir un espacio entre `[`, la condición y `]`.
	- `!`: indica la negación de la condición.
	- `-d`: revisa que el directorio exista. El `!` en este caso cambia a revisar el que no exista `DIRTRABAJO`.
	- `then`: sintaxis para indicar lo que pasará al cumplirse la condición.
	- `fi`: cierra el bloque condicional.

Existen variantes como:

`if .. then .. else.. fi:`

	#!/bin/bash
	if [ "foo" = "foo" ]; then
	  echo "Es foo"
	else
	  echo "Es otra cosa"
	fi

`if .. then .. else if.. else .. fi:`

	#!/bin/bash
	if [ "foo" = "foo" ]; then
	  echo "Es foo"
	elif [ "bar" = "bar" ]; then
	  echo "Es bar"
	else
	  echo "Es algo mas"
	fi

## Ciclos

Los ciclos permiten realizar acciones sobre un conjunto de objetos como por ejemplo:

- El contenido de un directorio  
- Una rango de números  
- Cada linea de un archivo
- Valores

```bash
#Mostrar los números del 1 al 10
$ for i in {1..10}; do echo $i; done
1
2
3
4
5
6
7
8
9
10  
```

	#Las tablas de multiplicar del 2 al 5
	for i in {2..5}; do for a in {1..10}; do echo "$i x $a = $(($i*$a))"; done; done
	2 x 1 = 2
	2 x 2 = 4
	2 x 3 = 6
	2 x 4 = 8
	2 x 5 = 10
	2 x 6 = 12
	2 x 7 = 14
	2 x 8 = 16
	2 x 9 = 18
	2 x 10 = 20
	3 x 1 = 3
	3 x 2 = 6
	3 x 3 = 9
	3 x 4 = 12
	3 x 5 = 15
	3 x 6 = 18
	3 x 7 = 21
	3 x 8 = 24
	3 x 9 = 27
	3 x 10 = 30
	4 x 1 = 4
	4 x 2 = 8
	4 x 3 = 12
	4 x 4 = 16
	4 x 5 = 20
	4 x 6 = 24
	4 x 7 = 28
	4 x 8 = 32
	4 x 9 = 36
	4 x 10 = 40
	5 x 1 = 5
	5 x 2 = 10
	5 x 3 = 15
	5 x 4 = 20
	5 x 5 = 25
	5 x 6 = 30
	5 x 7 = 35
	5 x 8 = 40
	5 x 9 = 45
	5 x 10 = 50

## Funciones

Las funciones son bloques de código que pueden ser utilizados con frecuencia para facilitar una tarea y optimizar el código.

	#!/bin/bash
	function quit { 
	  exit  
	} 
	function hello { 
	  echo Hello!
	} 
	hello
	quit
	echo foo

## Otros

Obtener datos del usuario

	#!/bin/bash 
	echo Cómo te llamas?
	read NOMBRE
	echo Hola $NOMBRE

Operaciones matemáticas

	$ echo 1+1
	1+1
	$ echo $((1+1))
	2
	#Para fracciones utilice bc
	$ echo 7/6 | bc -l
	1.16666666666666666666
	#O para limitar las decimales...
	echo "scale=2; 7/6" | bc -l
	1.16

Evaluar el último comando

A veces es necesario saber si el comando anterior se ejecutó correctamente o no, este valor se guarda en una variable especial: `$?`. Si dicha variable es igual a cero, quiere decir que el último comando se ejecutó sin problema, cualquier otro valor indica algún tipo de error.

	#!/bin/bash
	$ cd /emoh &> /dev/null #Directorio que no existe>error
	$ echo $? # resultado 1
	$ cd $(pwd) &> /dev/null
	$ echo $? # resultado 0