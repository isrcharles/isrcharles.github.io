# Información de cuentas de usuario

## Archivo /etc/passwd

Es una archivo que por cada linea contiene información sobre un usuario. Cada registro separa la información en columnas, las cuales son divididas por `:`. Las columnas son las siguientes en orden de izquierda a derecha:

- Usuario: nombre del usuario con el que se ingresa al sistema
- password: cuando esta columna contiene una “x”, significa que el password está manejado en el archivo /etc/shadow
- id del usuario: Identificador númerico. Se utiliza de manera interna para la mayoría de procesos del sistema operativo.
- id del grupo principal del usuario
- Información adicional del usuario: Comúnmente nombre y apellido
- Ubicación del directorio home para dicho usuario
- shell que el usuario tiene por default.

Ejemplo del archivo

	ray:x:1001:1001:Ray Charles,,,:/home/ray:/bin/bash

## Archivo /etc/shadow

Anteriormente la contraseña de los usuarios se guardaba en el archivo `passwd`, el cual, necesitaba ser accedido (permisos de lectura) por otros programas como `ls` para mapear el `UID` con el nombre de usuario. Esto era un notable fallo de seguridad, es por eso que se optó por usar el archivo `shadow` para guardar los hash de las contraseñas de los usuarios. Este último archivo solo puede ser accedido por root.

El archivo `shadow` también maneja en cada linea información de un usuario y también separa en columnas el tipo de información usando `:`. Las columnas del archivo `shadow` son las siguientes:

- Nombre del usuario
- Password cifrado
- última vez que se cambió el password (en días), tomando como referencia la fecha Unix Epoch (1/01/1970)
- Mínimo de días transcurridos para que al usuario se le permita cambiar el password
- Máximo número de días que el usuario puede conservar el mismo password.
- Días de antelación en los que se le advertirá al usuario que su password está por caducar.
- Número de días tras los que, si el usuario no ha cambiado su password, la cuenta será deshabilitada.
- Expiración: número de días que transcurrirán para que la cuenta sea deshabilitada. Se toma en cuenta la Unix Epoch

Ejemplo del archivo:

	ray:$6$QARtFGG:16809:0:99999:7::

> :pencil: _**NOTAS:**_ 
  > - Es más sencillo ver esta información mediante el uso del comando `passwd -S usuario`
  > - En ambos archivos es posible dejar algunas columnas en blanco.

## Directorio /etc/skel

Se podría tomar como una plantilla en la que se basa la creación del `home` de cada usuario nuevo. Es útil cuando se desea que ciertos archivos o estructura de directorios, se creen para todos los usuarios que se agreguen en un futuro. Para los usuarios ya existentes, se tendría que copiar manualmente los archivos.


