# Introducción a la terminal

### Login o registro en el sistema

Al terminar el proceso de arranque de Ubuntu server el sistema solicita un login (usuario y contraseña) para otorgar acceso. La pantalla inicial es similar a lo siguiente:

```bash
Ubuntu 20.04 LTS miequipo tty1
    
miequipo login:
```


> :pencil: _**NOTAS:**_ 
> - El sistema mostrará lo que se capture en login, es decir, el usuario, pero no mostrará ninguna salida (incluyendo asteriscos) cuando se teclee la contraseña, sin embargo, tenga la certeza que el sistema reconoce y evalúa la misma. 
> - TTY o Terminal es un dispositivo de entrada y salida de datos. En Linux se puede acceder a diferentes terminales utilizando la combinación de teclas `Alt-F1` hasta `Alt-F6`. Son útiles cuando se desea correr diferentes procesos al mismo tiempo, como por ejemplo, ejecutar una consulta de base de datos y en otra terminal, revisar el consumo de recursos mediante top.
> - El término consola hace referencia a una terminal física.

Ejemplo de un `prompt` real:

	israel@ich-gazelle:/tmp/PruebasDeDesarrollo $

Descripción del `prompt`

```bash
                            DIRECTORIO
                                DE
USUARIO                      TRABAJO   
  v                             v
israel@ich-gazelle:/tmp/PruebasDeDesarrollo $
            ^                               ^   
       NOMBRE DEL                    TIPO DE USUARIO
         EQUIPO                      $ = USUARIO NORMAL
                                     # = SUPERUSUARIO
```

Note que el `prompt` puede variar entre distribuciones de Linux o incluso al cambiar de shell (`zshell` por ejemplo), con lo que se perdería esta información a simple vista. Para obtener la información antes mencionada utilice los siguientes comandos:

```bash
#Obtener usuario actual 
whoami 
#Obtener nombre del equipo 
uname -n 
#Opción 2 para obtener nombre del equipo 
cat /etc/hostname 
#Obtener directorio de trabajo 
pwd 
```