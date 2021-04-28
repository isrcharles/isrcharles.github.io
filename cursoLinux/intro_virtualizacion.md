# Introducción a la virtualización

## Conceptos de virtualización

**Virtualización**: Tecnología que permite abstraer el hardware de un equipo (computadora) y el software (SO y aplicaciones) emulando el hardware a través del software, permitiendo crear ambientes que corran múltiples sistemas independientes al mismo tiempo.

**Máquina virtual**: Instancia de un sistema operativo que permite ofrecer recursos lógicos al usuario como cpu, ram, almacenamiento entre otros.

**Hipervisor**: Software que permite al usuario correr de forma simultánea uno o más sistemas operativos (máquinas virtuales). Se dividen en dos tipos:

 - **Tipo 1**: corre directamente sobre el hardware. (Oracle VM server, Microsoft Hyper-V, VMware ESX/ESXi y RHEV)

<center>

![imagen1](https://raw.githubusercontent.com/Ialdape/Imagenes-wiki/master/Tipo1.png)

</center>

 - **Tipo 2**: Corre sobre un sistema operativo (VirtualBox, Microsoft Virtual PC, VMware Server y Workstation, parallels).

<center>

![imagen2](https://raw.githubusercontent.com/Ialdape/Imagenes-wiki/master/Tipo2.png)

</center>

## ¿Por qué utilizar virtualización?

En los cursos que realizamos se utiliza porque facilita el crear un ambiente de pruebas para los ejercicios que se realizan, pero otras opciones son:

-   **Consolidación de servidores**: Evita comprar más y más servidores físicos
-   **Reduce la huella en el medio ambiente**  que dejan los centros de datos.
-   **Acelera**  el aprovisionamiento de servidores
-   **Reduce los gastos**  por consumo de energía (servers y sistemas de enfriamiento) y de espacio de almacenamiento.

## Virtualbox

Hipervisor del tipo 2

-   Desarrollado en un inicio por Sun Microsystems, hoy en día mantenido por Oracle.  
-   Robusto 
-   Alto rendimiento  
-   Multiplataforma
-   Gratuito

## Otras definiciones

-   **Host  OS**: Computadora física con un sistema operativo donde se corre el hipervisor tipo 2.
-   **Guest  OS**: Sistema operativo que se instala y ejecuta dentro de una máquina virtual (VM)    
-   **Guest additions**: Paquetes de software que se instalan en VirtualBox para dotar a los Guest  OS  de mejor rendimiento y características extras.

