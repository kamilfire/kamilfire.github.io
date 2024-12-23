---
title: DockHackLab
author: Ldeluq
date: 2024-12-20 14:10:00 +0800
categories:
  - Ctf
  - DockerLabs
tags:
  - ciber
render_with_liquid:
---
Iniciamos la maquina 

![[Pasted image 20241220154112.png]](/imagenes/Pasted%20image%2020241220154112.png)

escaneamos los puertos `nmap -p- --open -sCSV -vvv -Pn -n --min-rate 5000 172.17.0.2` 

![[Pasted image 20241220154354.png]](/imagenes/Pasted%20image%2020241220154354.png)

en el navegador encontramos un pagina de apache 

![[Pasted image 20241220154538.png]](/imagenes/Pasted%20image%2020241220154538.png)

realizamos fuzzing web con gobuster `gobuster dir -u http://172.17.0.2 -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -t 50 -x txt,html,php,py,js,png,jpg`  


![[Pasted image 20241220154804.png]](/imagenes/Pasted%20image%2020241220154804.png)

en la nueva dirrecion encontramos 

![[Pasted image 20241220154911.png]](/imagenes/Pasted%20image%2020241220154911.png)

al intentar subir una revershell tenemos este mensaje 

![[Pasted image 20241220155045.png]](/imagenes/Pasted%20image%2020241220155045.png)

vemos que nuestro archivo es modificado al subirlo asi que debemos descubrir que nombre se le asigna para poder entrar a la direccion asi que hacemos uso de un script para generar un diccionario que combine letras y numeros para encontrar el nombre correcto                     

![[Pasted image 20241220162252.png]](/imagenes/Pasted%20image%2020241220162252.png)

luego usamos fuzzing web con el nuevo diccionario de datos   y encontramos nuestro archivo                                                   `gobuster dir -u http://172.17.0.2/hackademy/ -w /home/test/Documents/labs/dockhacklab/diccionario_rever-shell1.txt -t 50 -x txt,html,php,py,js,png,jpg` 

![[Pasted image 20241220163611.png]](/imagenes/Pasted%20image%2020241220163611.png)


desde nuestra maquina atacante nos colocamos a la escucha en el puerto que definimos en la rever shell y nos dirigimos a la dirrecion encontrada 

![[Pasted image 20241220163831.png]](/imagenes/Pasted%20image%2020241220163831.png)

tenemos la rever shell y le damos tratamiento a la tty damos tratamiento de la tty 

`script /dev/null -c bash`  
`control +z`
`stty raw -echo; fg`
`reset xterm`
`export TERM=xterm`
`export SHELL=bash`
y usamos `sudo -l`  

![[Pasted image 20241220164736.png]](/imagenes/Pasted%20image%2020241220164736.png)

buscamos en gtfobins como escalar privilegios en nano 


![[Pasted image 20241220164923.png]](/imagenes/Pasted%20image%2020241220164923.png)

hacemos uso de nano en el usuario firtshacking 
`sudo -u firsthacking /usr/bin/nano` 
`Presiona Ctrl+R` 
`Presiona Ctrl+X`  
`Escribe: reset; bash 1>&0 2>&0` 
`Presiona Enter`

![[Pasted image 20241220165704.png]](/imagenes/Pasted%20image%2020241220165704.png)

y llisto somos el usuario firsthacking 

![[Pasted image 20241220170135.png]](/imagenes/Pasted%20image%2020241220170135.png)

usamos `sudo -l` 

![[Pasted image 20241220170302.png]](/imagenes/Pasted%20image%2020241220170302.png)


buscamos en gtfobins como escalar con este comando  `sudo docker run -v /:/mnt --rm -it alpine chroot /mnt sh`  pero nos sale este error



![[Pasted image 20241220170554.png]](/imagenes/Pasted%20image%2020241220170554.png)

intentando otras cosas y buscando en los dirrectorios encuentro esto

![[Pasted image 20241220171557.png]](/imagenes/Pasted%20image%2020241220171557.png)

voy a la funcion y encuentro esto  

![[Pasted image 20241220171753.png]](/imagenes/Pasted%20image%2020241220171753.png)

investigamos y podrian ser puertos que estan cerrados asi que usamos knock para ver si tenemos alguna respuesta                          `knock 172.17.0.2 12345 54321 24680 13579 -v`

![[Pasted image 20241220172128.png]](/imagenes/Pasted%20image%2020241220172128.png)

volvemos a intentar con el comando que encontramos en gtfobins   `sudo docker run -v /:/mnt --rm -it alpine chroot /mnt sh` para ver si cambio algo y conseguimos ser root

![[Pasted image 20241220172425.png]](/imagenes/Pasted%20image%2020241220172425.png)

