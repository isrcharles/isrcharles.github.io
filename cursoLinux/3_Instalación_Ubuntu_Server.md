# Instalación de Ubuntu Server

## Introducción

En esta guía se verá la instalación de Ubuntu server 16.04.

## Pasos para la instalación

1. Para descargar el ISO y grabar la imagen a una USB ve a la sección [Preparando la instalación](/cursoLinux/2_Preparando_la_instalación)
2. Selecciona el idioma de instalación.
3. El menú de arranque trae diferentes opciones como:
	* Instalación de Ubuntu server básica: Inicia el proceso de instalación que se verá en este documento
	* Revisar el cd por defectos: comprueba que la información contenida en el cd esté integra (se haya grabado bien)
	* Revisar la ram: Realiza varias pruebas a la memoria ram con el fin de encontrar defectos. En caso de encontrarlos, se recomienda ampliamente cambiar físicamente la memoria para evitar errores en el sistema.
	* Arrancar desde el primer disco duro: Omite el proceso de instalación e inicia el sistema operativo instalado en el disco duro.
	* Rescatar un sistema dañado: Inicia en memoria un sistema mínimo, el cual nos permite utilizar comandos para poder realizar las tareas necesarias corregir los errores en el sistema o dar mantenimiento.
4. Selecciona el lenguaje que será usado para la instalación así como la locación actual.
5. Selecciona la distribución del teclado: esta decisión es importante dado que elegir erróneamente este dato, complicará encontrar caracteres especiales usados en los comandos.
6. El instalador configurará las interfaces buscando un servidor `DHCP` en la red. En caso de no encontrarlo, se podrá realizar la configuración manual.
7. Seleccione el nombre del host. Solo se permiten números, letras (sin acento o ñ) y el guión medio (-). Si es necesario, también se puede definir el dominio en este paso usando el formato: `equipo.dominio.xyz`
8. El usuario que se introduzca durante el proceso de instalación, tendrá privilegios de adminitrador (root) a través del comando `sudo`.
9. Cifrar el home del usuario: solo es recomendable cifrar la información cuando la misma es muy sensible. En caso de un fallo en el proceso de arranque del sistema, recuperar la información de un sistema cifrado puede ser complicado (aún con la contraseña).
10. Definir la zona horaria: Este paso es importante pues el sistema configura la hora en base a lo seleccionado.
11. Particionado del disco: Este es uno de los pasos con más variantes. Se debe saber bien la finalidad del servidor para poder elegir un particionado correcto. En caso de duda, es mejor seleccionar la opción de particionado automático. **Importante**: este paso borrará toda la información del disco duro.
	* El particionado automático creará 2 particiones `/` y `swap`.
	* El particionado automático con `LVM` facilita aumentar el espacio de particiones añadiendo más discos duros, pero introduce una capa de software adicional en la administración del servidor.
	* No se recomienda cifrar el disco si no se cuenta con suficiente experiencia con las tecnologías utilizadas.
12. El sistema iniciara la instalación una vez confirmados los cambios en el disco.
13. Actualizaciones del sistema:
	* Manual: Recomendado cuando se quiere tener mayor control sobre los cambios en el servidor, sin embargo, si hay un fallo de seguridad, el sistema quedará expuesto hasta que alguien aplique las actualizaciones.
	* Instalar actualizaciones de seguridad automáticamente: Si es un sistema que estará expuesto a Internet, y no se le dará mantenimiento en periodos cortos, se recomienda activar esta opción.
	* Administrar el sistema con landscape: Opción de paga, no se verá en esta guía.
14. Tasksel: Elegir tareas de paquetes es una opción que ofrece el instalador para que sea este el que facilite la instalación de software adicional que normalmente se ocupa en diferentes roles de la base de datos. Si se desea tener mayor control del software a instalar, se puede omitir este paso y hacer la instalación de forma manual.
15. Instalación de `grub`: si es el único sistema operativo a instalar en el equipo (es lo común y recomendado en servidores), puede seleccionar que grub se instale en el sector de arranque del primer disco duro.
16. Una vez que termine la instalación seleccione la opción de reiniciar. No olvide de extraer el cd o usb antes del próximo arranque del equipo.