---
title: Escolares
author: Ldeluq
date: 2024-12-23 14:10:00 +0800
categories:
  - Ctf
  - DockerLabs
tags:
  - ciber
render_with_liquid:
---
Iniciamos la Maquina 

![[Pasted image 20241223145801.png]](/imagenes/Pasted%20image%2020241223145801.png)

escaneamos los puertos `nmap -p- --open -sCSV -vvv -Pn -n --min-rate 5000 172.17.0.2`

![[Pasted image 20241223145946.png]](/imagenes/Pasted%20image%2020241223145946.png)


entramos en el navegador 

![[Pasted image 20241223150132.png]](/imagenes/Pasted%20image%2020241223150132.png)

inspecciono la pagina y encuentro una direccion interesante 

![[Pasted image 20241223150306.png]](/imagenes/Pasted%20image%2020241223150306.png)

vamos a la nueva direccion encontramos un posible usuario admin worpress

![[Pasted image 20241223150535.png]](/imagenes/Pasted%20image%2020241223150535.png)

hago fuzzing web en busca de nuevas direcciones
`gobuster dir -u http://172.17.0.2/ -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -t 50 -x txt,html,php,py,js,png,jpg`  

![[Pasted image 20241223150923.png]](/imagenes/Pasted%20image%2020241223150923.png)

intentamos entrar al phpmyadmin pero no funciona tenemos un problema para conectarnos con el mysql

![[Pasted image 20241223151223.png]](/imagenes/Pasted%20image%2020241223151223.png)

asi que probamos entrar en la direccion worpress pero al dar click vemos que debemos anadir la direccion al etc/host usamos el comando `sudo nano /etc/hosts`

![[Pasted image 20241223151552.png]](/imagenes/Pasted%20image%2020241223151552.png)


entramos nuevamente usando la nueva direccion 

![[Pasted image 20241223151859.png]](/imagenes/Pasted%20image%2020241223151859.png)

hacemos fuzzing web 
`gobuster dir -u http://escolares.dl/wordpress/ -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -t 50 -x txt,html,php,py,js,png,jpg` 

![[Pasted image 20241223152133.png]](/imagenes/Pasted%20image%2020241223152133.png)

entramos al login del wordpress y vemos que luisillo es un usuario 

![[Pasted image 20241223152312.png]](/imagenes/Pasted%20image%2020241223152312.png)

buscamos alguna vurnerabilidad usando wpscan y confirmamos el usuario luisillo

![[Pasted image 20241223152748.png]](/imagenes/Pasted%20image%2020241223152748.png)

intentamos hacer fuerza bruta para conseguir la contrasena `wpscan --url http://escolares.dl/wordpress/wp-login.php -U luisillo -P /usr/share/wordlists/rockyou.txt` espero un tiempo pero no pude encontrar nada asi que recuerdo que en la pagina de profesores tenemos un poco de informacion asi que intentamos crear un diccionario personalizado para luisillo.
intentamos con crunch pero no logramos conseguir nada luego de un tiempo pensando y preguntar a chatgpt alternativas para crunch y descubrimos cupp 


![[Pasted image 20241223153455.png]](/imagenes/Pasted%20image%2020241223153455.png)


usamos cupp y creamos un diccionario personalizado 

![[Pasted image 20241223153625.png]](/imagenes/Pasted%20image%2020241223153625.png)

![[Pasted image 20241223153708.png]](/imagenes/Pasted%20image%2020241223153708.png)

volvemos ah lanzar el ataque con el nuevo diccionario `wpscan --url http://escolares.dl/wordpress/wp-login.php -U luisillo -P /home/test/Documents/labs/escolares/luis.txt` y encontramos la contrasena 

![[Pasted image 20241223153936.png]](/imagenes/Pasted%20image%2020241223153936.png)

entramos en el panel del login y vemos que podemos abusar del WP file Manager para subir archivos asi que intentaremos subir una rever-shell

![[Pasted image 20241223154105.png]](/imagenes/Pasted%20image%2020241223154105.png)

![[Pasted image 20241223154455.png]](/imagenes/Pasted%20image%2020241223154455.png)

nos colocamos a la escucha en la maquina atacante y nos vamos a la direccion donde se encuntra nuestra rever shell http://escolares.dl/wordpress/wp-content/themes/twentytwentyfour/

![[Pasted image 20241223154937.png]](/imagenes/Pasted%20image%2020241223154937.png)

le damos tratamiento a la tty 
`script /dev/null -c bash`  
`control +z`
`stty raw -echo; fg`
`reset xterm`
`export TERM=xterm`
`export SHELL=bash`

![[Pasted image 20241223155120.png]](/imagenes/Pasted%20image%2020241223155120.png)

navegamos por los directorios y encontramos una posible contrasena para luisillo asi que pasamos hacernos el usuario luisillo 

![[Pasted image 20241223155604.png]](/imagenes/Pasted%20image%2020241223155604.png)


ya somos usuario luisillo listamos los servicios con `sudo -l` y buscamos en gtfobins como escalar privilegios 

![[Pasted image 20241223155853.png]](/imagenes/Pasted%20image%2020241223155853.png)

![[Pasted image 20241223160001.png]](/imagenes/Pasted%20image%2020241223160001.png)

usamos el comando encontrado y listo somos root verificamos con `whoami` que usuario somos 

![[Pasted image 20241223160150.png]](/imagenes/Pasted%20image%2020241223160150.png)
