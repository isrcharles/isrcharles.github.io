# Usuarios

## Creación de usuarios

En Ubuntu se tienen dos comandos para crear usuarios, el comando `useradd` y `adduser`. La diferencia principal es que el comando `adduser` es un script hecho en `perl` que facilita mucho la creación de los usuarios, pero lamentablemente no se encuentra disponible en todas las distribuciones. Por el contrario, `useradd` es la utilería estándar dentro de las distribuciones Linux y conocerla permite realizar el trabajo sin importar la variante de Linux que se esté utilizando.

### Crear un usuario: `useradd`

	$ sudo useradd -s /bin/bash -d /home/ray -m ray

#### Parámetros

- -d: sirve para especificar el directorio home del usuario que se está creando. `Importante`: Esta opción no crea el directorio, solo especifica la ruta (que puede o no existir).
- -m: Crea el directorio `home` del usuario en caso de que no exista.
- -s: especifica el shell que usará el usuario

#### Asignar un password al usuario creado

	$ sudo passwd ray

> :pencil: _**NOTA:**_ **Siempre** se debe especificar el usuario al que se desea cambiar el password, de lo contrario, se estará cambiando el password del usuario que ejecuta el comando `passwd`. Ejemplo:

	# Esto cambia el password del usuario 
	$ sudo passwd usuario
	
	#Esto cambia el password del usuario root *Usar con cuidado*
	$ sudo passwd

### Crear un usuario: `adduser`

```bash
	$ sudo adduser ray
	Adding user `ray' ...
	Adding new group `ray' (1006) ...
	Adding new user `ray' (1006) with group `ray' ...
	Creating home directory `/home/ray' ...
	Copying files from `/etc/skel' ...
	Enter new UNIX password:
	Retype new UNIX password:
	passwd: password updated successfully
	Changing the user information for ray
	Enter the new value, or press ENTER for the default
	Full Name []: 
	Room Number []:
	Work Phone []: 
	Home Phone []: 
	Other []: 
	Is the information correct? [Y/n] y
```

Como se puede observar, el comando `adduser` facilita la creación del usuario al crear automáticamente el directorio home, el grupo, copiando archivos de `/etc/skel` y asignar un password a la nueva cuenta; incluso, pide información adicional que puede ser útil en algunos casos. En este ejemplo se dejan los valores por omisión en la mayoría de las opciones.

## Borrar usuarios

Para borrar cuentas de usuario se utiliza el comando `userdel`, el proceso es sencillo, lo único que se podría complicar se sale del ambiente técnico y es, el decidir si al eliminar el usuario también se debe eliminar todos sus documentos o conservarlos pues el usuario pudo dejar trabajo importante que será necesario en un futuro.

### Borrar un usuario y su directorio personal

	$ sudo userdel -r ray

### Borrar solamente la cuenta del usuario

	$ sudo userdel ray

#### Alternativa para borrar el directorio personal del usuario

	$ sudo rm -rf /home/ray

!> Importante: El comando `rm -rf` borra **todo** lo que se le indique de manera recursiva, es decir, incluyendo subcarpetas. Si no se tiene cuidado, puede llegar a borrar archivos importantes que pueden dejar su sistema inutilizable.

## Cambiar de usuario

Existen 2 maneras para cambiar de usuario y en ambas es necesario proporcionar el password del usuario al que se desea cambiar:

	su - usuario # opción 1, recomendada
	su usuario # opción 2

La diferencia radica en que al utilizar la primera opción, todas las variables de entorno se inicializan como si el usuario se hubiese logueado directamente en el sistema. Aunque es rara la ocasión, en algunos casos se obtienen comportamientos extraños si se usa la segunda opción.

El usuario `root` (o un usuario con privilegios de `sudo`) puede utilizar los comandos anteriormente descritos y al ser administrador, no se le solicitará el password del usuario.