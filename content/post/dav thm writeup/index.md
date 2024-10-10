---
title: dav thm Writeup
description: 
date: 2024-08-28 00:00:00+0000
image: featured1.png
categories:
  - TryHackMe
  - easy THM
tags: ["easy THM"]
weight: 3       # You can add weight to some posts to override the default sorting (date descending)
---


#### nmap 

En el escaneo realizado de nmap encontramos este puerto abierto 

<img src="featured2.png" alt="Featured Image">

---


Dentro del Puerto 80 encontramos este sitio Web, no encontramos nada interesante 

<img src="featured3.png" alt="Featured Image">

---


#### fuzzing

Vamos a realizar fuzzing en la web, apuntando a subdominios, ya que no encontramos nada dentro del sitio web.

<img src="featured4.png" alt="Featured Image">

---

#### subdominio

Dentro del subdominio, encontramos este panel de inicio de sesión. Busquemos en Google las credenciales predeterminadas para DAV.

<img src="featured5.png" alt="Featured Image">

---

#### credenciales

Después de investigar un poco, encontramos estas credenciales. Vamos a acceder a ellos.

Usuario : WAMPP PSWORD : Xampp

<img src="featured6.png" alt="Featured Image">

Logramos acceder, pero lo que tenemos aquí no sirve. Intentemos otro enfoque.

<img src="featured7.png" alt="Featured Image">

---


 #### uso 

Creamos una shell y en otra terminal nos colocamos en usando netcat escucha.

Donde dice "ubicación", tomaremos esa URL y la pegaremos nuevamente en el navegador. Aparecerá el panel de inicio de sesión y volveremos a usar las credenciales. Pero recuerde estar escuchando en el puerto asignado, que por ahora es 1234.

```
sudo curl --user "wamp":xamp" http://ip/webdav/ --upload-file revershell.php -v 

```

<img src="featured8.png" alt="Featured Image">

---

#### shell

conseguimos una shell 

<img src="featured9.png" alt="Featured Image">


---

conseguimos la user root 

<img src="featured10.png" alt="Featured Image">


---


#### root flag

Abusamos de los permisos SUID con

```
sudo /bin/cat /root/root.txt

``` 
<img src="featured11.png" alt="Featured Image">

---

