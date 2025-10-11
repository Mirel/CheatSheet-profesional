# CheatSheet-profesional
# 🛠️ Integración Nmap-MSF: Guía Rápida para el Reconocimiento y la Explotación Inicial

***

## 🎯 Objetivo del Recurso y Contexto Profesional

Este documento sirve como un **recurso profesional y guía rápida** diseñado para analistas de seguridad *junior* o equipos que busquen optimizar su flujo de trabajo en las fases iniciales de un *pentest*.

El enfoque principal es demostrar y facilitar la transición metodológica entre la **obtención de datos precisos** mediante Nmap (calidad del dato) y su **utilización eficiente** en el *framework* de explotación Metasploit (MSF).

**Enfoque Profesional:** Este recurso prioriza la **calidad del dato** y la **integración de herramientas** para una explotación precisa, buscando minimizar errores comunes y ahorrar tiempo.

---

## 3. Nmap: El Paso Cero (Calidad del Dato)

Esta sección explica cómo usar Nmap para obtener la información crítica (versión del servicio y SO) que Metasploit necesita. El uso de la opción `-oA` es crucial, ya que genera el archivo **`.gnmap`**, el único formato que Metasploit puede importar directamente a su base de datos.

### 3.1. Comando Maestro y Recolección de Datos

| Comando | Intención Técnica | Resultados Clave para MSF |
| :--- | :--- | :--- |
| `nmap -sS -sV -O -p- -T4 --open <IP> -oA fullscan` | **Escaneo Agresivo y Versátil:** Escaneo SYN (`-sS`), Detección de Versión (`-sV`), Detección de SO (`-O`), todos los puertos (`-p-`). Genera salidas en formato *grepable* (`.gnmap`). | **Versión de Servicios** (ej. *PostgreSQL 9.3*), **Versión de SO** (ej. *Linux Kernel 3.x*). |

### 3.2. Scripts Esenciales para Enumeración Previa

Se ejecutan *scripts* específicos para obtener información de *login* o detalles de configuración antes de la explotación:

* **`--script ftp-anon`**: Prueba si el servidor FTP abierto permite el acceso **anónimo** (Relevante para Ejercicio 7).
* **`--script pgsql-info` / `pgsql-brute`**: Obtiene información detallada del servidor PostgreSQL y prueba credenciales comunes (Ejercicio 8).
* **`--script ssh-enumusers`**: Enumeración de usuarios a través del protocolo SSH (Relevante para Ejercicio 6 y 7).

---

## 4. Flujo de Trabajo y Transición (Nmap → Metasploit)
![Diagrama de flujo (ruta)](CheatSheet-profesional/Flujo Visual del Proceso.png)

Esta sección detalla el procedimiento para importar los datos de Nmap a la base de datos de Metasploit, automatizando el proceso de *targeting*.

### 4.1. Integración de la Base de Datos

* **Razón:** Evitar errores de *typing* y acelerar la configuración de *exploits*.
* **Pasos en `msfconsole`:**
    1.  **Iniciar Base de Datos:** `msfdb init` (Asegura que la base de datos PostgreSQL de MSF esté lista).
    2.  **Crear Workspace:** `workspace -a [Nombre_Proyecto]` (Procedimiento para aislar los datos del proyecto).
    3.  **Importar Resultados:** `db_import /ruta/a/fullscan.gnmap` (Carga el host, puertos y versiones detectadas por Nmap en la DB de MSF).

### 4.2. Flujo Visual del Proceso
### 5.1. Búsqueda Inteligente de Módulos

Utilizo la indexación de la base de datos de MSF para realizar búsquedas específicas y ahorrar tiempo, en lugar de revisar una lista kilométrica:

* `search type:exploit platform:linux name:postgres` (Busca solo *exploits* para PostgreSQL en Linux).
* `search type:auxiliary name:smb_enum` (Busca módulos auxiliares para enumeración SMB).

### 5.2. Post-Explotación (Meterpreter)

El *payload* preferido es **Meterpreter** porque no es una *shell* simple, sino un entorno modular para realizar tareas de **post-explotación** sin salir de la sesión:

* `getsystem`: Intento automatizado de escalada de privilegios.
* `migrate`: Migrar el proceso a otro más estable (ej. `explorer.exe`) para evitar caídas.
* `upload` / `download`: Transferencia de archivos.

### 5.3. Limitaciones y Criterio del Analista

* **Limitación:** Nmap puede fallar en la detección de versión si hay un *firewall* o un *load balancer*. En estos casos, el **criterio técnico exige** que la **inspección manual** (usando `telnet` o `netcat`) para leer los *banners* de servicio sea obligatoria.
* **Criterio del *Handler*:** Si Nmap detecta una versión de servicio y Metasploit no tiene un *exploit* directo, demuestro mi capacidad para usar la herramienta como *listener* avanzado: se utiliza **`exploit/multi/handler`** en MSF para configurar un *listener* y luego se lanzan *exploits* manuales de **Exploit-DB** o de terceros, manteniendo la sesión de *post-explotación* dentro del *framework*.
