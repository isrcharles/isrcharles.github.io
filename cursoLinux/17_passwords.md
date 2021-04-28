# Gestión de password

## Comando `passwd`

Este comando sirve para bloquear o activar cuentas de usuario y asignar o modificar la contraseña de un usuario.

### Asignar o cambiar una contraseña

	$ sudo passwd <usuario>

### Cambiar la contraseña del **usuario actual**

	$ passwd

### Bloquear una cuenta de usuario

	$ sudo passwd -l <usuario>

### Activar una cuenta de usuario

	$ sudo passwd -u <usuario>

## Creación de políticas para los passwords

### Comando `chage`

Sirve para ver o cambiar la información relacionada con la expiración de la contraseña de un usuario.

#### Ver información

	$ chage -l <usuario>
	Último cambio de contraseña                                     : oct 21, 2016
	La contraseña caduca                                            : nunca
	Contraseña inactiva                                             : nunca
	La cuenta caduca                                                : nunca
	Número de días mínimo entre cambio de contraseña                : 0
	Número de días máximo entre cambio de contraseña                : 99999
	Número de días de aviso antes de que caduque la contraseña      : 7

#### Forzar a un usuario a cambiar su contraseña tras su próximo inicio de sesión

	$ sudo chage -d 0 <usuario>

#### Requerir un cambio de contraseña tras N días

	$ sudo chage -M 60 <usuario>

### Política de password seguro e historial de passwords

Definir una política sobre los passwords de los usuarios ayuda a tener un mejor control sobre cuestiones del largo de la contraseña, su complejidad, evitar el uso de contraseñas anteriores, etc.

#### Instalar el módulo de autenticación necesario

	$ sudo apt-get install libpam-cracklib

#### Editar archivo `/etc/pam.d/common-password`

Agregar la siguiente linea debajo del comentario que termina con: **the Primary block**.

	password        required        pam_pwhistory.so        remember=5      use_authok

Con lo anterior, el sistema recordará los últimos 5 passwords del usuario y evitará que sean reutilizados.