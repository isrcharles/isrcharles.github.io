# Gestión de trabajos

## Introducción

La mayoría de comandos que se utilizan en el día a día corren en primer plano (foreground), es decir, nosotros podemos ver su ejecución, sin embargo, esto nos limita a poder ejecutar un comando en la terminal, si deseamos ejecutar otro, es necesario esperar a que el comando actual termine. Una alternativa para ejecutar varios comandos es hacerlo en diferentes terminales, pero también se puede hacer uso del segundo plano (background).

Para ver un ejemplo de mandar un proceso a segundo plano, ejecute lo siguiente:

	$ sleep 60 # una vez que se ejecute el comando, estará ocupando la terminal por 60 segundos.


Con la terminal ocupada, presione `ctrl+z`

	$ sleep 60
	^Z
	[1]+  Detenido                sleep 60

Al presionar `ctrl+z` el proceso actual se va al background y la terminal está lista para usarse. Note que se nos informa que el proceso se ha detenido. En caso de que se requiera que el comando se siga ejecutando en `background` utilice el comando `bg`. A continuación se muestra un ejemplo:

	$ sleep 30
	^Z
	[1]+  Detenido                sleep 30
	$ bg
	[1]+ sleep 30 &
	$ jobs
	[1]+  Ejecutando              sleep 30 &

Para recuperar nuevamente el proceso se utiliza el comando fg (foreground).

	$ fg
	sleep 60

Para ver una lista de los trabajos que se tienen en la sesión actual y su estado, utilice el comando jobs

	$ jobs
	[1]+  Detenido                sleep 60

También es posible tener varios comandos en background y que jobs los muestre

	$ jobs
	[1]-  Detenido                sleep 60
	[2]+  Detenido                sleep 160

Para recuperar un proceso en específico se utiliza fg mas el ID obtenido mediante jobs

	$ fg 1 #recuperaría sleep 60

Para correr en background un proceso en específico utilice `bg` más el ID obtenido mediante `jobs`

	$ bg 2 #Ejecuta el trabajo 2 en background

En la práctica, es posible que este conocimiento de la gestión de trabajos (jobs) sea útil en escenarios en los que abrir una terminal extra sea el camino largo, por ejemplo cuando se está administrando un servidor por `ssh` y tanto la ip/dominio como el usuario/password son extensos. En este caso si se está editando algún archivo mediante `vi` y se desea consultar la página del manual, solo será necesario presionar `ctrl+z` (`importante`: no estando en modo edición), después se consulta el manual necesario y se vuelve a la edición mediante `fg`.

Otra forma de mandar un proceso a background es mediante el uso de &. Siguiendo el ejemplo con el comando sleep sería:

	$ sleep 60 &

De la misma forma se recupera con `fg`.

Por último, cabe mencionar que en caso de que la terminal se cierre, todos los procesos en background terminarían. Para que los procesos sigan en ejecución, después de haber mandado el trabajo a background y activarlo mediante `bg`, el siguiente comando

	$ disown %1 #Donde 1 es el número de trabajo que muestra jobs

Si se planea dejar un trabajo corriendo en background y cerrar la conexión, se puede usar el comando:

	$ nohup sleep 60 & # cambie el comando sleep por el que se desee 

## Matar procesos/trabajos

En caso de que se desee terminar un proceso que esté consumiendo muchos recursos o que simplemente quedó trabado, se puede hacer uso del comando `kill`. Este comando recibe como argumento en `PID` que se obtiene mediante `ps` o `pidof`.

	$ kill 12345

La opción anterior mataría el proceso 12345, pero de forma “educada”, es decir, dando oportunidad a que el proceso se cierre de forma ordenada (SIGTERM 15). En caso de que un proceso se rehúse a cerrarse, se utiliza la opción `-9` (SIGKILL 9). Nota: use esta opción como último recurso pues en algunos casos puede dar como resultado archivos corrompidos, problemas con la memoria, entre otros.

Para más información acerca de las señales pasadas a los procesos consulte `man 7 signal`

Una variante más segura que `kill` es `killall`, y es más segura porque evita que se mate el proceso equivocado, pues toma como parámetro el nombre del comando.

	$ killall sleep
	$ killall -9 sleep
