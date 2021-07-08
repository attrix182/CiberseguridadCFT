
# Ciberseguridad (Attrix182)

<img src="https://github.com/attrix182/CiberseguridadCFT/blob/main/assets/portada.gif?raw=true" heigth="350px"></img>

___ 

## Introduccion 
Mi nombre es Luciano, en este documento explicare paso a paso como comenzar a resolver maquinas de captura la bandera, las cuales consisten en acceder a una maquina solo conociendo su IP, para ello se debe utilizar distintas tecnicas de reconocimiento y explotacion de vulnerabilidades.
Soy nuevo en este campo, pero me gusta aportar lo que voy aprendiendo para quienes tambien se interesan en aprender.
Nota: Actualmente lo estoy redactando en español, pero proximamente lo hare en ingles tambien
	
Webs que proveen laboratorios para resolver:
  
<ol>  
<li> <a> https://www.hackthebox.eu/ </a> </li>  
<li> <a> https://tryhackme.com/ </a> </li>  
</ol>

### Preparacion del entorno de trabajo

Recomiendo utilizar una maquina virtual con alguna distribucion de linux, las mas conocidas son Parros OS y Kali Linux, estas ya vienen con las herramientas nececsarias preinstaladas
Links:
<ol>  
<li> <a>https://www.virtualbox.org/</a></li> 
<li> <a>https://www.parrotsec.org/</a></li>  
<li> <a>https://kali.org/</a></li>  
</ol>

___ 
	
### Resolver máquinas CTF 

Debemos pasar por varias etapas para comprometer un sistema estas son:
  
 <ol>  
<li>Etapa de reconocimiento</li>  
<li>Explotación de vulnerabilidades</li>  
<li>Escalada de privilegios</li>  
</ol>



Comencemos con:

> <b> 1) Etapa de reconocimiento </b>

Esta consiste en analizar todo lo que se pueda con la IP que se nos otorga, tanto desde el navegador como desde consola, algunos comandos a utilizar son:

Es muy importante que durante todo el proceso guardemos toda la informacion relevante que vayamos encontrando, desde nombre de usuario, hasta frases, o retorno de comandos que ejecutemos, ya que con esta informacion podemos armar diccionarios propios o usar esos datos para probar ingresar al sistema

Este reconocimiento se puede hacer de forma visual y tambien desde terminal.
Generalmente se suele iniciar con el visual para luego seguir investigando.

> > A. Reconocimiento visual: <br>
 Consiste en ingresar desde el navegador a la IP que se nos ha proporcionado y observar de que tipo de sitio se trata, recomiendo utilizar Chrome e instalar la siguiente extension: [Wappalyzer](https://chrome.google.com/webstore/detail/wappalyzer/gppongmhjkpfnbhagpmjfkannfbllamg?hl=es) 
esta nos retornara las librerias, frameworks y lenguajes que detecte en el sitio.
Tambien si utilizamos Chrome, con click derecho, podemos seleccionar la opcion de ver el codigo fuente, y buscar comentarios en el codigo.
Otro lugar a buscar son las imagenes, a veces tienen de nombres palabras claves


> B.Terminal
> Consiste en utilizar

Para ello podemos ejecutar los siguientes comandos:

A. PING. Ver sistema operativo y verificar conexión
```
ping -s 1  00.00.00.00 
```

( 00.00.00.00  es la ip de la máquina objetivo) con este comando enviamos una traza icmp, es decir transmitimos y recibimos paquetes para saber si tenemos conexión con la máquina y vemos el TTL*

La salida será similar a:
```
PING 00.00.00.00 (00.00.00.00) 56(84) bytes of data.
64 bytes from 00.00.00.00: icmp\_seq =1 ttl=127 time=76.3ms
```


  > *TTL:  Time To Live, representa el número de saltos que ha dado el paquete de host en host por internet hasta alcanzar su destino. Es probable que veamos que disminuya en 1 unidad
  >   > ttl=128: Windows
  >  >  ttl=64: Linux

<br>
<br>

B. NMAP. Buscaremos puertos abiertos en la IP de la máquina entre otras utilidades de NMAP

```
nmap -p- --open -T5 -v -n 00.00.00.00 -oG AllPorts
```
Buscara en todos los puertos cuales están abiertos y exportará la salida en formato grepeable en el archivo AllPorts.

```
nmap -sS --min-rate 5000 -p- --open -vvv -n 00.00.00.00 -oG AllPorts
```
Lo mismo que el comando anterior pero mucho más rápido

**Luego podemos usar extractPorts(Recomendado): [https://pastebin.com/tYpwpauW](https://pastebin.com/tYpwpauW) by s4vitar

```
extractPorts AllPorts
```
 Debemos tener agregada esta función a nivel zshrc, la salida será similar a:
 ```
[ *] Extracting information…
	[*] IP : 00.00.00.00
[*] Open ports: 22,80

[*] Ports copied to clipboard

```
 
 
```
nmap -sc -sV -p80 00.00.00.00 -oN targeted
```
Buscara servicios y versiones en la IP determinada y lo exporta en un archivo de texto

>En este momento podremos ver para donde apuntar el ataque buscando exploits y vulnerabilidades del gestor de archivos o servicio que esté instalado en el servidor



C. WFUZZ O DIRBUSTER
```
wfuzz -c -–hc=404 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```
[http://00.00.00.00/FUZZ] // Esta utilidad basandose en un diccionario reemplazará con esos nombres de rutas comunes, por fuerza bruta, en la palabra FUZZ, y mostrará todos aquellos que retornen una respuesta distinta a 404(Not Found), para que asi podamos saber por que rutas atacar

___ 

> <b> 2) Explotación de vulnerabilidades </b>

Aqui debemos detenernos y ver que es lo que encontramos hasta el momento para ver por donde dirigir el "ataque"

Si con nmap vimos abiertos los siguiente puertos, generalmente significan lo siguiente:
