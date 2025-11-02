
# 🖥️ Análisis Técnico de la Máquina Víctima (Debian 13.1)

---

## 1. Información general del sistema

| Elemento | Valor |
|-----------|--------|
| Hostname | debian |
| IP | 192.168.0.31 |
| Usuario principal | mire |
| Arquitectura | arm64 |
| Servicios activos | SSH (puerto 22) |

---

## 2. Servicios Detectados con Nmap

**Comando:**
```bash
sudo nmap -sS -sV -O -p- 192.168.0.31 -oN evidencia/puertos_tcp_all.txt
