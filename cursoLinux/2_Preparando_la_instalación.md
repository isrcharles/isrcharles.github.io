# Preparando la instalación 

## Introducción

Aunque lo explicado en esta sección puede aplicar para `Ubuntu Desktop` (incluye interfaz gráfica) y `Ubuntu Server` (sin interfaz gráfica), se hablará siempre tomando un enfoque en la versión para servidores.

> :pencil: _**NOTA:**_ Para servidores, siempre se recomienda utilizar la edición de `Ubuntu server` y no instalar interfaz gráfica debido a que:

- Los recursos de un servidor no deben ser mal gastados en procesos adicionales que se ejecutan con la interfaz gráfica.
- Software como network-manager tiende a causar problemas en la configuración de la red para un servidor. NM está optimizado para ayudar en escenarios a los que se enfrenta una laptop (cambios constantes de redes e ips)
- Software adicional aumenta la posibilidad de introducir fallos de seguridad.

### Tipos de versiones

Una nueva versión de Ubuntu se libera cada 6 meses, generalmente los últimos días de los meses de abril y octubre. Se podrían definir las versiones en dos categorías:

- Versión estándar:
	- Soporte oficial (Canonical provee actualizaciones del sistema y de seguridad) por 9 meses.
	- Se enfoca en tener versiones del software actualizadas sin que hayan sido probadas extensamente.
	- Se recomienda para probar los cambios más recientes, pero no para ambientes de producción.
- Versión LTS (Long Term Support)
	- Soporte oficial por 5 años, pudiendo extenderse 2 años adicionales si se ha adquirido (pagado) Ubuntu Advantage.
	- Se libera generalmente cada 2 años en los últimos días de abril.
	- Contiene software estable.
	- Recomendado para ambientes de producción.

### Número de versión

Ejemplo `18.04.1`

- 18: Indica el año.
- 04: Indica el mes.
- .1: Conocido como “point release”, se libera cada 6 meses y contiene paquetes actualizados de la base. Se podría comparar a un “service pack” en windows.

Desde el 2006 las versiones LTS se liberan cada 2 años por lo que la versión actual soportada es 20.04 y la siguiente versión será la 22.04 (Abril del 2022)

### Requerimientos del sistema

| Tipo de instalación | CPU      | RAM     |Sistema base|Todas las tareas instaladas|
| :-------            | :------  | -----   |:-----      |:---------                 |
| Server standard     |  1 GHz   | 512 MB  |1.5 GB      |2.5 GB                     |
| Server mínima	      | 300 MHz  | 256 MB  |1.5 GB      |2.5 GB                     |

### Obtener el archivo ISO

Puedes descargar la versiones de Ubuntu server desde [Link para descargar ISO de ubuntu Server](http://www.ubuntu.com/download/server/download)

En caso de que la versión haya sido liberada recientemente, se recomienda realizar la descarga por torrent (vea la sección de descargas alternas) puesto que el ancho de banda de los servidores para descarga directa puede ser muy limitado.

!> Importante: No te olvides de comprobar la suma de verificación una vez que la descarga se haya completado, esto te ayuda a comprobar que la descarga se realizó sin problemas y aunque no es del todo infalible, te ayuda a comprobar que la imagen viene de la fuente oficial. 

Para hacer esta comprobación se puede ejecutar en una terminal alguno de los siguientes comandos:
```bash
#Opción 1
md5sum archivo.iso
#Opción 2
sha1sum archivo.iso
#Opción 3
sha256sum archivo.iso
```

Una vez que tengas la suma de comprobación, realiza una búsqueda en google con la suma obtenida del comando que elegiste y verifica que la misma se muestre como resultado de las páginas oficiales. Si la suma de verificación no concuerda, **no instales usando ese ISO** pues puede que obtengas fallos en la instalación o incluso, que sea una versión modificada que pueda poner en riesgo la seguridad de tu servidor.

### Grabar el ISO a USB

La opción más sencilla, y que a la vez es multiplataforma, para realizar el grabado a una USB es usar [BalenaEtcher](https://www.balena.io/etcher/)

Recuerda que la usb donde se guardará la imagen será formateada, así que realiza un respaldo previamente. De igual manera la usb se utilizará en su totalidad, por lo que no podrás hacer uso del espacio “sobrante” a menos que vuelvas a formatear la memoria.