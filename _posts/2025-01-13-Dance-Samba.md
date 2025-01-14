---
title: Dance-Samba
author: Ldeluq
date: 2025-01-13 14:10:00 +0800
categories:
  - Ctf
  - DockerLabs
tags:
  - ciber
render_with_liquid:false
---
Iniciamos la Maquina 

![[Pasted image 20250113160613.png]](/imagenes/Pasted%20image%2020250113160613.png)

escaneamos los puertos `nmap -p- --open -sCSV -vvv -Pn -n --min-rate 5000 172.17.0.2`

![[Pasted image 20250113161127.png]](/imagenes/Pasted%20image%2020250113161127.png)

observamos que podemos acceder mediante ftp como Anomymous `ftp "anonymous"@172.17.0.2`  listamos los archivos y encontramos un archivo llamado nota.txt que nos descargamos 

![[Pasted image 20250113161526.png]](/imagenes/Pasted%20image%2020250113161526.png)

cuando leemos el archivo encontramos informacion interesante de posibles ususarios

![[Pasted image 20250113161830.png]](/imagenes/Pasted%20image%2020250113161830.png)


asi que usamos rpcclient para ver si tenemos suerte y encontramos dos posibles usuarios y encontramos macarena  `rpcclient -U "" -N 172.17.0.2

![[Pasted image 20250113162502.png]](/imagenes/Pasted%20image%2020250113162502.png)

intentamos usar como password donald y usuario macarena con base a la informacion que estaba en la nota y eureka tuvimos suerte accedemos `smbmap -H 172.17.0.2 -u 'macarena' -p 'donald'`

![[Pasted image 20250113163026.png]](/imagenes/Pasted%20image%2020250113163026.png)

tenemos permisos de lectura y escritura en macarena asi que ingresamos `smbclient -U 'macarena' //172.17.0.2/macarena` y listamos los archivos y encontramos un archivo user.txt que descargamos 

![[Pasted image 20250113163411.png]](/imagenes/Pasted%20image%2020250113163411.png)

leemos la informacion del archivo y encontramos un tipo de hash 

![[Pasted image 20250113163527.png]](/imagenes/Pasted%20image%2020250113163527.png)

vamos a cyberchef para tratar de decifrar pero no logramos nada asi que probamos si se puede subir una llave SSH con puttygen la creamos  `puttygen -t rsa -b 2048 -O private-openssh -o kamil`       y le damos permisos con `chmod 600 kamil` que fue nuestro archivo creado luego extraemos la clave publica `/usr/bin/puttygen kamil -o authorized_keys -O public-openssh`

![[Pasted image 20250113165529.png]](/imagenes/Pasted%20image%2020250113165529.png)

creamos una carpeta llamada .ssh y subimos el archivo authorized_keys con `mkdir .ssh` 

![[Pasted image 20250113165951.png]](/imagenes/Pasted%20image%2020250113165951.png)

`ssh -i kamil macarena@172.17.0.2`    entramos por ssh 

![[Pasted image 20250113170404.png]](/imagenes/Pasted%20image%2020250113170404.png)

vemos que no podemos listar los servicios porque no tenemos el password

![[Pasted image 20250113170557.png]](/imagenes/Pasted%20image%2020250113170557.png)

navegamos por los directorios y encontramos una carpeta llamada secret y encontramos un archivo llamado hash 

![[Pasted image 20250113170829.png]](/imagenes/Pasted%20image%2020250113170829.png)

vamos a cyberchef para decifrar el hash 

![[Pasted image 20250113171010.png]](/imagenes/Pasted%20image%2020250113171010.png)

ya con la contrasena listamos los servicios y buscamos gtfobins para escalar privilegios en file 

![[Pasted image 20250113171422.png]](/imagenes/Pasted%20image%2020250113171422.png)

![[Pasted image 20250113171330.png]](/imagenes/Pasted%20image%2020250113171330.png)

encontramos en el directorio opt un archivo txt llamado password y hacemos uso de el file para leer el archivo `sudo -u root /usr/bin/file -f /opt/password.txt`

![[Pasted image 20250113171929.png]](/imagenes/Pasted%20image%2020250113171929.png)

con el password encontrado nos hacemos root y verificamos con `whoami` 

![[Pasted image 20250113172134.png]](/imagenes/Pasted%20image%2020250113172134.png)
