# Descripción de directorios

La estructura del árbol de directorios de Linux empieza en la raiz /. Conocer algunos de los subdirectorios y su función, facilita la interacción con las tareas del día a día.

- `/bin`: comandos usados por un usuario normal.
- `/boot`: archivos requeridos para el arranque del sistema operativo. El kernel se encuentra en este lugar.
- `/cdrom`: punto de montaje para el cd/dvd
- `/dev`: dispositivos o archivos especiales como por ejemplo, dispositivos de almacenamiento interno (discos duros) o externo (memorias usb)
- `/etc`: Contiene archivos de configuración de los servicios y scripts de arranque.
- `/home`: contiene los archivos personales de los usuarios del sistema con excepción del usuario root. Es el equivalmente a “Mis documentos” en windows.
- `/lib`: contiene librerías del sistema
- `/media`: Directorio de montaje para dispositivos externos como memorias usb.
- `/`: Directorio de instalación para software de terceros (no incluidos por default en los repositorios oficiales). Por ejemplo: skype, google chrome, kingsoft office, sublime text, entre otros.
- `/proc`: Contiene archivos que muestran información del kernel y de los procesos que están en ejecución.
- `/root`: Es el home del administrador
- `/sbin`: Contiene comandos usados por el administrador
- `/usr`: Contiene programas secundarios, librerías y documentación acerca de programas enfocados al usuario.
- `/var`: Contiene datos variables como las bitácoras del sistema, info de bases de datos, servidores web y similares.
- `/tmp`: Archivos temporales como sockets o descargas parciales. Todo su contenido se borra al reiniciar/apagar el equipo. Útil para guardar información “de paso”.

[Aquí](https://www.pathname.com/fhs/) se puede obtener más información acerca del estándar del sistema de ficheros.