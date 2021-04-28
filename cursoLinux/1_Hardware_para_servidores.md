# Hardware para servidores

### Finalidad del servidor

La siguientes reglas son muy generales y solo tienen como objetivo dar una idea base de lo que se debe tomar en cuenta en escenarios simples.

### Escenarios

- Respaldos: Puedes comprar discos estándar (7.2K), pero de gran tamaño. Si no se tiene bien definido la cantidad total que se necesitará a futuro, se puede particionar con LVM para tener flexibilidad a la hora de redimensionar las particiones.
- Base de datos: Se recomiendas discos rápidos y desplegar el servidor en RAID 10 (al menos 4 discos) para obtener un mejor rendimiento y mejorar la velocidad de lectura/escritura.
- Virtualización: Se recomiendan procesadores con multiples cores en base al número de máquinas virtuales que se vayan a desplegar.
- Firewall: Se necesita al menos 2 tarjetas de red para poder dividir la red local de la red externa (Internet). En caso de querer desplegar una DMZ o separar la red inalámbrica, se tendrá que instalar una tarjeta adicional para cada escenario.

### Recomendaciones generales

- Siempre que sea posible desplegar memoria RAM ECC.
- Dependiendo de la importancia del servicio que ofrezca el servidor, se debe agregar una fuente de poder redundante y UPS.
- Usar hardware de marca. Dell y System76 son marcas que tienen muy buena compatibilidad con Ubuntu y Linux en general.

### Procesadores

La decisión por el tipo de procesador a utilizar se basa principalmente en que:

- A mayor número de cores, se pueden atender un mayor número de usuarios
- Cores más rápidos son recomendados cuando hay procesos pesados, como funciones o querys complejos.

### Memoria RAM

Para un servidor de base de datos, normalmente se traduce el tener más RAM a que se obtiene un mejor performance, sobre todo si se hacen uso de índices. Los servidores deben llevar memoria ram ECC (Error-Correcting Code) la cual sirve para garantizar la integridad de los datos.

### Discos Duros

Existen principalmente 2 tipos: SAS y SATA.

**SAS:** Ofrece ventajas de velocidad y performance. También soporta características avanzadas de las cuales el sistema operativo puede sacar ventaja.

- Velocidad: 10K a 15K rpm.
- Tamaño promedio: desde 75GB hasta 1TB
- Costo por GB elevado

**SATA:** Son más asequibles.

- Velocidad: 7.2K a 10K rpm.
- Tamaño promedio: 2 TB
- Costo por GB menor

### Manejo de errores

El reporte de errores normalmente se realiza por medio de dos mecanismos:

- Códigos de error reportados durante las operaciones de entrada y salida
- El protocolo SMART. El cual da información sobre el disco duro como la temperatura y el resultado de los test de diagnostico.

Los discos SATA comerciales tiene una política agresiva de auto corregir errores. Esto está bien cuando hay una sola copia de la información y se usan en un sistema de escritorio, pues el tiempo que se pierde tratando de auto corregir el error, impacta solo a un usuario. Pero en servidores, este comportamiento aunado a una controladora de arreglos, dificulta el trabajo a esta última.

Los discos SAS que son usados normalmente con un sistema RAID, reportan el error y la información se obtiene de una copia alterna.

Los discos SATA empresariales o marcados como `RAID edition` corrigen ese comportamiento, sin embargo la diferencia en precio entre estos y los discos SAS, se reduce considerablemente.

### Firmware y RAID

Es importante que la versión del firmare de los discos de un arreglo coincidan para obtener un performance confiable. Los discos SATA comerciales suelen tener grandes cambios en los firmware de los discos sin tener mucho control sobre dichos cambios. Esto lleva a que en muchos casos no se pueda realizar un downgrade de la versión del firmware o incluso que aunque el modelo del disco siga en el mercado, tenga una versión diferente.