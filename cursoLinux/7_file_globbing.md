# File globbing

### Introducción

Se le conoce al término `globbing` a la operación que realiza el shell (bash en el caso de Ubuntu) de expandir un patrón a través de comodines (`*`,`?`,`[]`) a resultados que concuerden con dicho patrón. Los comodines son útiles a la hora de trabajar con diversos archivos. Para entender de manera sencilla el concepto se explicará con ejemplos cada uno de ellos.

### `*` asterisco

Se utiliza para hacer referencia a cualquier cadena de caracteres, incluso vacía. 

```bash
echo *.jpg #Lista todos los archivos que terminen con extensión jpg
echo respaldoJu*2017 #Muestra los archivos respaldoJulio2017 y respaldoJunio2017
echo p* # Muestra los archivos prueba1 y prueba2
```

### `?` signo de interrogación

Hace referencia a cualquier coincidencia, pero solo de un caracter

```bash
    ls
    file1.txt file2.txt file3.txt
    ls file?.txt
    file1.txt file2.txt file3.txt
```

### `[]` Corchetes

Muestra coincidencias múltiples por caracter. 

```bash
ls #mostrar todo el contenido
a1.txt  A2.txt  A3.txt  A4.txt  a6.txt  b2.txt  B3.txt  B4.txt  b6.txt  c2.txt  c4.txt  c5.txt  c6.txt  d2.txt 4.txt  D5.txt  D6.txt
a2.txt  a3.txt  a4.txt  a5.txt  b1.txt  b3.txt  b4.txt  b5.txt  c1.txt  c3.txt  C4.txt  C5.txt  d1.txt  d3.txt d5.txt  d6.txt
echo [abc][123].txt #mostrar solo los archivos texto que empiecen con a,b o c y terminen con 1,2 o 3.
a1.txt a2.txt a3.txt b1.txt b2.txt b3.txt c1.txt c2.txt c3.txt
echo [Acd][36].txt #mostrar solo los archivos texto que empiecen con A,c o d y terminen con 3 0 6.
A3.txt c3.txt c6.txt d3.txt d6.txt
```

### Rangos

Los rangos se utilizan junto con los corchetes. 

```bash
echo [a-z][0-9].txt
 a1.txt a2.txt a3.txt a4.txt a5.txt a6.txt b1.txt b2.txt b3.txt b4.txt b5.txt b6.txt c1.txt c2.txt c3.txt c4.txt c5.txt c6.txt d1.txt d2.txt d3.txt d4.txt d5.txt d6.txt
echo [b-d][4-6].txt
  b4.txt b5.txt b6.txt c4.txt c5.txt c6.txt d4.txt d5.txt d6.txt
```

### Negaciones

Es posible mostrar todo lo que no coincida con el patron que se le ofrece.

```bash
echo [!b-d][!4-6].txt
a1.txt a2.txt A2.txt a3.txt A3.txt
```

Por último, note que es posible combinar estos comodines. 

```bash
echo [aACd]?.* 
a1.txt a2.txt A2.txt a3.txt A3.txt a4.txt A4.txt a5.txt a6.txt C4.txt C5.txt d1.txt d2.txt d3.txt d4.txt d5.txt d6.txt
```
