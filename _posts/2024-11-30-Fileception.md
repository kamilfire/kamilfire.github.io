---
title: Fileception
author: Ldeluq
date: 2024-12-23 14:10:00 +0800
categories:
  - Ctf
  - DockerLabs
tags:
  - ciber
render_with_liquid:
---
Iniciamos la maquina 

![[Pasted image 20241227113431.png]](/imagenes/Pasted%20image%2020241227113431.png)

escaneamos los puertos `nmap -p- --open -sCSV -vvv -Pn -n --min-rate 5000 172.17.0.2`

![[Pasted image 20241227113558.png]](/imagenes/Pasted%20image%2020241227113558.png)

vemos que podemos entrar por ftp como anonymous y navegamos en los directorios y vemos una archivo jpg que descargamos para luego analizar 

![[Pasted image 20241227113745.png]](/imagenes/Pasted%20image%2020241227113745.png)

abrimos el navegador e inspeccionamos la pagina

![[Pasted image 20241227113841.png]](/imagenes/Pasted%20image%2020241227113841.png)

![[Pasted image 20241227113925.png]](/imagenes/Pasted%20image%2020241227113925.png)

asi que encontramos informacion interesante para analizar la imagen que encontramos anteriormente usamos cyberchef para desifrar la cadena de texto y encontrar una posible contrasena 

![[Pasted image 20241227114237.png]](/imagenes/Pasted%20image%2020241227114237.png)

encontramos un archivo txt y encontramos un contenido particular que luego analizamos con chatgpt

![[Pasted image 20241227114503.png]](/imagenes/Pasted%20image%2020241227114503.png)

![[Pasted image 20241227114656.png]](/imagenes/Pasted%20image%2020241227114656.png)

intentamos con chatgpt desifrar pero no logramos asi que buscamos en la web un sitio donde podamos usar este tipo de codigo Ook
 encontramos https://www.cachesleuth.com/bfook.html donde pasamos el codigo a brainfuck y luego a txt y vemos una posible contrasena 

![[Pasted image 20241227120143.png]](/imagenes/Pasted%20image%2020241227120143.png)

cuando inspeccionamos el sitio encontramos un usuario peter asi que probaremos entrar usando ssh 

![[Pasted image 20241227120326.png]](/imagenes/Pasted%20image%2020241227120326.png)

encontramos una nota que pasamos a ver su contenido 

![[Pasted image 20241227120436.png]](/imagenes/Pasted%20image%2020241227120436.png)

procedemos a ir al directotio tmp y encontramos otro txt y vemos su contenido 

![[Pasted image 20241227120701.png]](/imagenes/Pasted%20image%2020241227120701.png)

descargamos el archivo importante_octopus.odt 

![[Pasted image 20241227120908.png]](/imagenes/Pasted%20image%2020241227120908.png)

abrimos el archivo y vemos que es un archivo comprimido con muchos otros archivos dentro revisando nos centramos en leerme.xml

![[Pasted image 20241227121247.png]](/imagenes/Pasted%20image%2020241227121247.png)

abrimos el archivo y conseguimos credenciales del usuario octopus 

![[Pasted image 20241227121526.png]](/imagenes/Pasted%20image%2020241227121526.png)

hacemos uso nuevamente de cyberchef 

![[Pasted image 20241227121752.png]](/imagenes/Pasted%20image%2020241227121752.png)

escalamos privelegios al usuario octopus y listamos los servicios que este usuario puede ejecutar con `sudo -l`

![[Pasted image 20241227122049.png]](/imagenes/Pasted%20image%2020241227122049.png)

vemos que podemos hacernos root desde octopus ejecutamos `sudo su` y listo somos root verificamos con `whoami` 

![[Pasted image 20241227122327.png]](/imagenes/Pasted%20image%2020241227122327.png)

