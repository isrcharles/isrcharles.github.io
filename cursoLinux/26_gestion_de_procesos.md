# Gestion de procesos

## Introducción

Al administrar un servidor es necesario saber los procesos que se están ejecutando para validar que realmente se esté ofreciendo los servicios necesarios. Un programa es el binario que está guardado en file system. Un proceso es el resultado de ejecutar un programa.

`ps` muestra los procesos del usuario que lo ejecuta.

	$ ps
	  PID TTY          TIME CMD
	21372 pts/3    00:00:00 bash
	21423 pts/3    00:00:00 ps


- `PID`: Es el identificador del proceso que lo diferencia de otros procesos. Es importante ver que aunque para nosotros es más sencillo hablar de procesos por el nombre del comando que se ejecuta (`vim,top,sleep`), el sistema utiliza estos números para su gestión.
- `TTY`: Muestra la terminal en la que se está ejecutando el proceso.
- `TIME`: Tiempo de cpu que se ha utilizado para este proceso.
- `CMD`: Comando relacionado al PID

Nota: Por default, solo muestran los procesos en la misma terminal. Para ver los procesos ejecutados por el usuario en cualquier terminal, se utiliza la opción `-a`.

Otra forma de encontrar el `PID` de un proceso es con el comando `pidof`. Para esto debemos pasar como argumento el nombre del programa involucrado en el proceso.

	$ pidof vim
	22410

Otras opciones del comando `ps` son:

	$ ps a # note que no lleva el guión antes de la opción a
	PID TTY      STAT   TIME COMMAND
	1225 tty1     Ss     0:00 /bin/login --
	1533 tty1     S+     0:00 -bash
	1630 pts/0    Ss     0:00 -bash
	30571 pts/0    R+     0:00 ps a

Esta opción agrega la columna `STAT` la cual muestra el estado del proceso. En primer instancia uno se puede llegar a confundir, sin embargo se puede consultar la página del manual de `ps` en la sección `PROCESS STATE CODES` para más información.

	$ ps aux
	syslog     918  0.0  0.1 256396  3044 ?        Ssl  21:21   0:00 /usr/sbin/rsyslogd -n
	message+   923  0.0  0.2  42900  3656 ?        Ss   21:21   0:00 /usr/bin/dbus-daemon --system --	address=systemd: --nofork --nopidfile --systemd-activation
	root       932  0.0  0.0   4400  1260 ?        Ss   21:21   0:00 /usr/sbin/acpid
	root       935  0.0  0.1  31524  2688 ?        Ss   21:21   0:00 /usr/sbin/cron -f
	root       938  0.0  1.3 211164 22332 ?        Ssl  21:21   0:00 /usr/lib/snapd/snapd
	daemon     939  0.0  0.1  26044  2000 ?        Ss   21:21   0:00 /usr/sbin/atd -f
	root       946  0.0  0.0  95368  1452 ?        Ssl  21:21   0:00 /usr/bin/lxcfs /var/lib/lxcfs/
	root       951  0.0  0.3 278592  6080 ?        Ssl  21:21   0:00 /usr/lib/accountsservice/accounts-daemon
	root      1008  0.0  0.0  13376   168 ?        Ss   21:21   0:00 /sbin/mdadm --monitor --pid-file /run/mdadm/monitor.pid --daemonise --scan --syslog
	root      1015  0.0  0.4 277180  7688 ?        Ssl  21:21   0:00 /usr/lib/policykit-1/polkitd --no-debug
	systemd+  1070  0.0  0.1  96092  1972 ?        Ssl  21:21   0:00 /lib/systemd/systemd-timesyncd
	root      1074  0.0  0.0  16124   864 ?        Ss   21:21   0:00 /sbin/dhclient -1 -v -pf /run/dhclient.enp0s3.pid -lf /var/lib/dhcp/dhclient.enp0s3.leases -I -df /va
	root      1145  0.0  0.3  65520  5296 ?        Ss   21:21   0:00 /usr/sbin/sshd -D
	root      1157  0.0  0.0   5224   160 ?        Ss   21:21   0:00 /sbin/iscsid
	root      1158  0.0  0.2   5724  3524 ?        S<Ls 21:21   0:00 /sbin/iscsid
	root      1246  0.0  0.0 124972  1436 ?        Ss   21:21   0:00 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
	www-data  1247  0.0  0.1 125332  3028 ?        S    21:21   0:00 nginx: worker process
	ray       1525  0.0  0.2  45248  4424 ?        Ss   21:21   0:00 /lib/systemd/systemd --user

Con las opciones `aux` agregadas, se pueden ver procesos de cualquier terminal incluyendo otros usuarios. Este listado siempre es largo pues incluso se incluyen procesos de nivel de sistema. Es común que este detalle de salida se utilice en conjunto de `pipas` y `grep` para filtrar por el proceso que se está buscando.

	$ ps aux | grep nginx

## Otras variantes

	$ ps aux --sort=-pcpu #muestra el listado de procesos por consumo de cpu.
	$ ps aux --sort=-pcpu | head -n 5 #Variante para solo mostrar los 5 procesos con más consumo de CPU
	$ ps aux --sort=-pmem | head -n 5 #Muestra los 5 procesos con más consumo de memoria






