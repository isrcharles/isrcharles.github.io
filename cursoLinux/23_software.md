# Gestión de software

## Introducción

Gracias a andriod, hoy en día muchos usuarios conocen lo que es una tienda de aplicaciones como la play store, sin embargo este concepto no es nuevo en el mundo de Linux. Los repositorios son servidores que contienen el software que puede ser instalado, así como las actualizaciones de dicho software. Ubuntu tiene repositorios oficiales a través de los cuales dota a los servidores del software base, sin embargo se pueden agregar nuevos repositorios para ampliar el catálogo y opciones.

El formato por default dentro de Ubuntu Linux son los paquetes `deb` estos suelen tener dependencias (otros paquetes) que necesitan ser instaladas en conjunto con el paquete original a fin de que todas las funcionalidades sean cubiertas.

## Gestores de paquetes

Para instalar o desinstalar paquetes se utilizan los gestores de paquetes. Se pueden catalogar en gestores de bajo nivel (no resuelven dependencias) y de alto nivel (resuelven dependencias de manera automática).

### dpkg

Es un gestor catalogado como bajo nivel, para instalar un paquete todas las dependencias deben estar resueltas.

Instalar paquete

	$ sudo dpkg -i paquete.deb

Desintalar paquete

	$ sudo dpkg -r paquete.deb

Listar paquetes instalados

	$ sudo dpkg -l

Filtrar listado de paquetes instalados en busca de un paquete en específico

	$sudo dpkg -l | grep nombreDelPaquete

### apt-get

Es un gestor de paquetes de alto nivel, pues resuelve dependencias

Instalar paquete

	$ sudo apt-get install paquete

Desintalar paquete

	$ sudo apt-get remove paquete

Buscar un paquete

	$ sudo apt-cache search nombrePaquete

Es posible instalar varios paquetes a la vez

	$ apt-get install paquete1 paquete2 paquete3

Purgar un paquete (ademas de eliminar el paquete, elimina los archivos de configuración)

	$ apt-get purge paquete

Actualizar info de paquetes/actualizaciones disponibles en los repositorios

	$ apt-get update

Actualizar el sistema:

	$ apt-get upgrade

Actualizar el sistema aunque sea necesario desinstalar paquetes

	$ apt-get dist-upgrade

Mostrar más información sobre un paquete

	$ apt-cache show paquete

### apt

Es la nueva versión simplificada de `apt-get` la cual da unas ventajas como un porcentaje de avance. Su uso es prácticamente el mismo:

	$ apt install paquete
	$ apt remove paquete
	$ apt search paquete

Nota: a día de hoy no se recomienda utilizar apt dentro de scripts de shell.

## Gestión de repositorios

Los repositorios se encuentran dentro del archivo `/etc/apt/sources.list` y repositorios adicionales se encuentran en el subdirectorio `/etc/apt/sources.list.d`

Una típica linea se muestra a continuación: 

	deb http://us.archive.ubuntu.com/ubuntu/ xenial main restricted

Donde:

- `deb` hace referencia a un repositorio con binarios. `deb-src` hace referencia a paquetes de código fuente.
- URL de la cual se obtendrán los paquetes. En este caso `http://us.archive.ubuntu.com/ubuntu/`
- `xenial` es el nombre código de la versión de Ubuntu.
- `main` refiere al componente el cual indica si el repositorio contiene software libre o no y si es soportado por canonical. Ejemplos pueden ser `main`, `restricted`, `universe`, `multiverse`.

Es posible agregar nuevos repositorios insertando en el archivo `sources.list` la linea correspondiente. 

!> Importante: Cada vez que se modifique el archivo `sources.list` se debe actualizar la info mediante `sudo apt-get update`.

## Corrigiendo errores

En ocasiones un proceso de instalación no termina bien por diferentes causas como por ejemplo, falta de espacio en disco o un apagado del servidor en medio del proceso de instalación. Para tratar de que apt resuelva este problema se utiliza:

	$ sudo apt-get install -f

Obviamente para que funcione, se debe haber arreglado el problema previo. Ejemplo: liberar espacio en disco.

## Mantenimiento

Si se ha elegido que las actualizaciones de seguridad se apliquen automáticamente, con el tiempo algunos paquetes ya no son requeridos (kernels antiguos principalmente). Para eliminar aquellos paquetes que ya no son necesarios y recuperar espacio en disco se ejecuta el comando

	$ sudo apt-get autoremove







