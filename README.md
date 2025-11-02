# 🧠 CheatSheet Profesional: Integración Nmap-Metasploit para Reconocimiento y Explotación Inicial

---

## 👩‍💻 Autor
**Nombre:** Mirelle Candida Silva 
**Proyecto:** El Arsenal de un Analista  
**Rol:** Analista Junior de Seguridad  
**Entorno:** macOS (host) + Debian (VM en UTM)

---

## 🎯 Objetivo del Recurso y Contexto Profesional

Este documento sirve como **guía técnica profesional y práctica** diseñada para analistas de seguridad que busquen optimizar su flujo de trabajo entre las fases de **reconocimiento** y **explotación inicial**.

El objetivo principal es mostrar cómo integrar **Nmap** (para recolección precisa de datos) con **Metasploit Framework (MSF)**, transformando información técnica en acciones efectivas dentro de un entorno realista de pentesting.

> 💡 En el mundo profesional, la calidad del reconocimiento inicial define el éxito de todo el proceso de explotación.

---

## ⚙️ Entorno de Laboratorio

| Elemento | Descripción |
|-----------|-------------|
| **Host (atacante)** | macOS con Nmap 7.98 |
| **Máquina virtual víctima** | Debian 13.1 en UTM |
| **Red** | NAT compartida (192.168.0.0/24) |
| **IP Host** | 192.168.0.26 |
| **IP VM (víctima)** | 192.168.0.31 |

---

## 🧩 Flujo Metodológico General

![Flujo de trabajo](CheatSheet-profesional/Flujo%20Visual%20del%20Proceso.png)

**Fases principales:**
1. Identificación de IP (Descubrimiento de red)  
2. Escaneo y reconocimiento con Nmap  
3. Consolidación de datos e importación en Metasploit  
4. Búsqueda de exploits específicos  
5. Ejecución de payloads y post-explotación  
6. Análisis final y documentación  

---

## 🛰️ 1. Descubrimiento de Red

**Comando ejecutado en macOS (host atacante):**
```bash
sudo arp-scan --interface=en0 --localnet | tee evidencia/arp_scan.txt
