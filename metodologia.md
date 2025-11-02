# 🧩 Metodología de Análisis y Reconocimiento

---

## 1. Objetivo del Proceso

El objetivo de esta metodología es establecer un flujo de trabajo técnico y reproducible para el reconocimiento y enumeración de servicios dentro de un entorno de laboratorio controlado.  
La base de esta metodología es el principio de **"Calidad del Dato"**, asegurando que cada comando ejecutado aporte valor al análisis posterior.

---

## 2. Fases de la Metodología

### 🔹 Fase 1: Descubrimiento de red
**Herramienta:** `nmap` o `arp-scan`  
**Objetivo:** Identificar hosts activos en la red local.

```bash
sudo arp-scan --interface=en0 --localnet | tee evidencia/arp_scan.txt

