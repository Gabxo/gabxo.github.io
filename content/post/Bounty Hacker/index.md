---
title: Bounty hacker Writeup
description: 
date: 2024-08-29 00:00:00+0000
image: featured1.png
categories:
  - TryHackMe
  - easy THM
tags: ["easy THM"]
weight: 3       # You can add weight to some posts to override the default sorting (date descending)
---


#### nmap

Dentro del escaneo, encontramos el puerto 21 FTP abierto, lo que parece interesante.

<img src="featured2.png" alt="Featured Image">


---



En los otros puertos no encontramos nada. Accedamos a través de FTP.

¡Oh, muy interesante! Descarguemos esos archivos y veamos qué encontramos

<img src="featured3.png" alt="Featured Image">


---


#### archivos

Usaremos cat en los archivos para ver lo que contienen y potencialmente, encontrar credenciales

encontramos un posible usuario llamado lin 

<img src="featured4.png" alt="Featured Image">

---




#### hydra

Usaremos Hydra para un ataque de fuerza bruta, y aquí está el comando:

```
hydra 10.10.41.104 ssh -s 22 -l lin -P locks.txt -f -vV

```

<img src="featured5.png" alt="Featured Image">


---



#### Credenciales

Obtuvimos las credenciales. Ahora podemos conectarnos a través de SSH

<img src="featured6.png" alt="Featured Image">

---

#### ssh

Muy bien, ahora que estamos conectados a través de SSH, es hora de encontrar las banderas de usuario y root.

<img src="featured7.png" alt="Featured Image">
---


#### root user

Obtuvimos la bandera de usuario. Ahora, procedamos a encontrar la bandera raíz.

<img src="featured8.png" alt="Featured Image">

---

#### SUID

Usamos este comando para revisar los permisos de SUID.

```
find / -perm -u=s -type f 2>/dev/null

```
<img src="featured9.png" alt="Featured Image">

---


#### ROOT SUID

Usamos sudo -l para ver los permisos que tiene root. Después de examinarlos, intentemos abusar de los permisos SUID.

<img src="featured10.png" alt="Featured Image">


---


#### abuso de SUID

Este es el comando que usaremos para abusar de los permisos de SUID y escalar privilegios:

```
sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh

```
<img src="featured11.png" alt="Featured Image">

<img src="featured12.png" alt="Featured Image">
