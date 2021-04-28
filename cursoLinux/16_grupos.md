# Gestión de grupos

## Introducción

?> Pendiente de desarrollar

## Archivo /etc/group

?> Pendiente de desarrollar

## Gestionar grupos

!> En los ejemplos siguientes, reemplace <usuario> y <grupo> por los valores reales que esté usando.

### Crear grupos

	$ sudo groupadd <grupo>

### Borrar gruposBorrar grupos

	$ sudo groupdel <grupo>

### Añadir usuarios a un grupo

	$ sudo usermod -aG <grupo> <usuario>

### Cambiar el grupo principal de un usuario

	$ sudo usermod -g <grupo> <usuario>

### Quitar un usuario de un grupo

	$ sudo gpasswd -d <usuario> <grupo-del-que-será-removido>

### gpasswd como alternativa para añadir usuarios a un grupo

	$ gpasswd -a <usuario> <grupo>