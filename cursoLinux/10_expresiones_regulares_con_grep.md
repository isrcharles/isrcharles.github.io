# Expresiones regulares con `grep`

 Las expresiones regulares son una herramienta muy poderosa que permite buscar texto utilizando patrones ahorrando mucho tiempo comparado con hacerlo de forma manual.

> :pencil: _**NOTAS:**_ 
> - Las expresiones regulares es un tema avanzado y complejo, se proveen ejemplos para dar una idea de su funcionamiento y uso básico.
> - En algunos ejemplos se utilizará el comando `egrep` el cual es un alias para `grep -E`

El comando `grep` permite utilizar expresiones regulares. En el día a día se utiliza para filtrar resultados la salida de comandos o buscar patrones dentro de archivos.

### Ejemplos básicos

Filtrar el listado de procesos que pertenecen al usuario israel:

	ps -ef | grep israel

Buscar intentos de acceso con usuarios incorrectos:

	sudo cat  /var/log/auth.log | grep  invalid

Buscar errores en las bitácoras

	sudo grep -Ri error apt/

### Ejemplos más avanzados

- `^`: patrón que concuerde en el inicio de la linea  

		grep ^root /etc/passwd #obtener info sobre el usuario root
		root:x:0:0: root:/root:/bin/bash

- `$`: patrón que concuerde al final de la linea   

		grep sh$ /etc/passwd #que usuarios pueden registrarse en el sistema
		root:x:0:0:root:/root:/bin/bash
		israel:x:1000:1000:israel,,,:/home/israel:/bin/bash

 Una `bracket expression` es una lista de caracteres entre corchetes `[ ]`

- ` [x]`: concuerda con el caracter (o rango de caracteres) de la lista en esa posición.

		egrep "[s]..[h]" /usr/share/dict/words

- ` b*`: concuerda si `b` aparece cero o más veces

		egrep "x*" /usr/share/dict/words

- `b+`: concuerda si `b` aparece una o más veces

		egrep "x+" /usr/share/dict/words

- ` .`: cualquier caracter en esa posición

		egrep "^a..z$" /usr/share/dict/words

- `[^x]`: concuerda con cualquier caracter que no esté en la lista

		egrep "..[^abcdefghijklmnopqrstuvw]$" /usr/share/dict/words # solo lo que termine en x,y o z

- ` a|b`: concuerda con `a` o `b` en esa posición

		egrep "^X|^Z" /usr/share/dict/words

- **Avanzado:** Buscar correos electrónicos que cumplan con el formato estándar `algo@otracosa.loquesea`

		egrep "^[A-Za-z0-9.S_%-]+@[A-Za-z0-9.-]+[.][A-Za-z]+$" correos.txt

### Repeticiones

- **?** El caracter que precede es opcional y concuerda al menos una vez.
- ***** El caracter que precede concuerda cero o más veces.
- **+** El caracter que precede concuerda una o más veces.
- **{n}** El caracter que precede concuerda exactamente `n` veces.
- **{n,}** El caracter que precede concuerda `n` o más veces.
- **{,m}** El caracter que precede concuerda al menos `m` veces.
- **{n,m}** El caracter que precede concuerda al menos `n` veces, pero no más de `m` veces.