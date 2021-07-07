
## Ciberseguridad 

##### Resolver máquinas CTF 



  
Debemos pasar por varias etapas para comprometer un sistema estas son:
  
 <ol>  
<li>Etapa de reconocimiento</li>  
<li>Explotación de vulnerabilidades</li>  
<li>Escalada de privilegios</li>  
</ol>

Comencemos con:

1) Etapa de reconocimiento

Esta consiste en analizar todo lo que se pueda con la IP que se nos otorga, tanto desde el navegador como desde consola, algunos comandos a utilizar son:

Para ello podemos ejecutar los siguientes comandos desde terminal:

```ping -s 1  00.00.00.00```
(** 00.00.00.00** es la ip de la máquina objetivo) con este comando enviamos una traza icmp, es decir transmitimos y recibimos paquetes para saber si tenemos conexión con la máquina y vemos el TTL


