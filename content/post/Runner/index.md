---
title: Runner Writeup
description: 
date: 2024-08-28 00:00:00+0000
image: featured1.png
categories:
  - HackTheBox
  - mediun HTB
tags: ["Mediun HTB"]
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---


#### Nmap 


realizamos un escaneo con nmap y encontramos estos puertos abiertos

doge come pone

<img src="featured2.png" alt="Featured Image">

---

#### fuzzing 
accedemos a la web, Nada en los puertos 80, 8000

<img src="featured3.png" alt="Featured Image">


---

encontramos el subdominio **TeamCity.runner.htb** lo agregaremos al /etc/hosts

<img src="featured4.png" alt="Featured Image">

---


esa es una página de inicio de sesión. Entonces, intenté buscar las credenciales predeterminadas, pero no tuvimos éxito

<img src="featured5.png" alt="Featured Image">

---

#### exploit

obtuve un exploit para la versión 2023.11.4 de TeamCity. TeamCity Admin Account Creation CVE-2024-27198


[exploit](https://github.com/Chocapikk/CVE-2024-27198/blob/main/exploit.py)

<img src="featured6.png" alt="Featured Image">

---


usamos el exploit de la siguiente manera

```
python exploit.py -u http://teamcity.runner.htb --add-user

```

<img src="featured7.png" alt="Featured Image">

consguimos estas credenciales

<img src="featured8.png" alt="Featured Image">

---


vamos al panel de login anteriormente descubierto y iniciamos sesion con las crendenciales que conseguimos


<img src="featured9.png" alt="Featured Image">

---


nos moveremos a **backup** y descargaremos el **.zip** 

<img src="featured10.png" alt="Featured Image">

---

#### id_rsa

En la enumeración, encontramos que hay id_rsa archivo en la carpeta de copia de seguridad.

<img src="featured11.png" alt="Featured Image"> 



<img src="featured12.png" alt="Featured Image">

---

#### usuarios

También encontramos usuarios y hashes en la misma carpeta.

<img src="featured13.png" alt="Featured Image">

solo se descifra el hash de Methew 


<img src="featured14.png" alt="Featured Image">

Hasta ahora tenemos un archivo id_rsa, dos usuarios (Methew, jhon), contraseña para Methew.


---

#### ssh

Con id_rsa archivo podemos iniciar sesión con éxito como JHON. Hench obtuvo el acceso inicial

<img src="featured15.png" alt="Featured Image">

<img src="featured16.png" alt="Featured Image">

---

conseguimos la flag de user 

<img src="featured17.png" alt="Featured Image">

---

Al visitar http://127.0.0.1:9000 hay una página de inicio de sesión. En el que podemos iniciar sesión con la contraseña de usuario de Methews

<img src="featured18.png" alt="Featured Image">

---

#### portainer

Nota: Portainer es un software de gestión de contenedores para implementar, solucionar problemas y proteger aplicaciones en casos de uso de nube, centros de datos e IoT industrial.

usaremos las credenciales previamente conseguidas 

```
user matthew password .piper123

```

<img src="featured19.png" alt="Featured Image">

---

#### exploit

<img src="featured20.png" alt="Featured Image">

También hay 2 imágenes de Docker -[ubuntu:latest, teamcity:latest]

<img src="featured21.png" alt="Featured Image">

[exploit](https://nitroc.org/en/posts/cve-2024-21626-illustrated/?source=post_page-----103250a9acd3--------------------------------#why-runc-decides-to-use-openat22)

Según esta publicación, tenemos que crear un directorio de trabajo de contenedor para esta ruta → /proc/self/fd/8

---

#### contenedor


Ahora cree un contenedor usando teamcity:latest.

<img src="featured22.png" alt="Featured Image">

<img src="featured23.png" alt="Featured Image">

<img src="featured24.png" alt="Featured Image">

---

#### consola

Como usuario root, obtenga acceso a la consola

<img src="featured25.png" alt="Featured Image">

---

#### root

```
cd /mnt
cd root 
cat root.txt

```

<img src="featured26.png" alt="Featured Image">
