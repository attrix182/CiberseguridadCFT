
# Ciberseguridad 

#### Resolver máquinas CTF 



  
Debemos pasar por varias etapas para comprometer un sistema estas son:
  
 <ol>  
<li>Etapa de reconocimiento</li>  
<li>Explotación de vulnerabilidades</li>  
<li>Escalada de privilegios</li>  
</ol>

Comencemos con:

<b> 1) Etapa de reconocimiento </b>

Esta consiste en analizar todo lo que se pueda con la IP que se nos otorga, tanto desde el navegador como desde consola, algunos comandos a utilizar son:

Para ello podemos ejecutar los siguientes comandos desde terminal:

A. PING
```
ping -s 1  00.00.00.00 
```

(** 00.00.00.00 ** es la ip de la máquina objetivo) con este comando enviamos una traza icmp, es decir transmitimos y recibimos paquetes para saber si tenemos conexión con la máquina y vemos el TTL*

La salida será similar a:
```
PING 00.00.00.00 (00.00.00.00) 56(84) bytes of data.
64 bytes from 00.00.00.00: icmp\_seq =1 ttl=127 time=76.3ms
```


  > *TTL:  Time To Live, representa el número de saltos que ha dado el paquete de host en host por internet hasta alcanzar su destino. Es probable que veamos que disminuya en 1 unidad
  >   > ttl=128: Windows
  >  >  ttl=64: Linux
