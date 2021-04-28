# Uso de comillas

### Introducción

El shell contiene caracteres especiales (`* @ # ? - $ ! 0 _`) que se interpretan al ejecutar comandos. Puede resultar complejo tratar de utilizar estos caracteres cuando forman parte del nombre de argumentos. Es posible deshabilitar su tratado especial utilizando varias formas, de las cuales el entrecomillar, es una de ellas y tal vez la más recomendable.

> :pencil: _**NOTA:**_ En este documento, si una linea empieza con el signo `$`, significa que esa linea representa el comando a ejecutar. Las demás lineas son la salida de dicho comando.

### Formas de escapar caracteres especiales

Se toman como referencia los siguientes archivos: `!superNoticia.txt`, `Los 10 mandamientos.txt, israel@charlesit.mx`

### Diagonal inversa

Es posible utilizar `\` para escapar cada caracter especial 

```bash
#Marca error por los espacios en blanco
$ cp Los 10 mandamientos.txt /tmp
cp: no se puede efectuar 'stat' sobre 'Los': No existe el archivo o el directorio
cp: no se puede efectuar 'stat' sobre '10': No existe el archivo o el directorio
cp: no se puede efectuar 'stat' sobre 'mandamientos.txt': No existe el archivo o el directorio
#Escapando con diagonal inversa
$ cp Los\ 10\ mandamientos.txt /tmp
$ ls /tmp/*.txt
/tmp/Los 10 mandamientos.txt
```

### Comillas simples

Este es el método recomendado. Puesto que ningún caracter se expande o es tratado de forma especial.

	$ cp 'israel@charlesit.mx' '!superNoticia.txt' /tmp/

### Comillas dobles
Quita el significado especial de casi cualquier caracter con excepción de: `$`, \`, `\`, `!`

	$ cp "israel@charlesit.mx" "!superNoticia.txt" /tmp/
	bash: !superNoticia.txt: event not found
