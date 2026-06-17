# Lab 01 — Análisis de Tráfico de Red con Wireshark

**Fecha:** Junio 2026  
**Herramientas:** Wireshark 4.2.2, Ubuntu 24 (VirtualBox)  
**Dificultad:** Básico  

---

## 🎯 Objetivo

Capturar y analizar tráfico de red real para identificar conversaciones DNS y TCP, y comprender cómo se ven estos protocolos desde la perspectiva de un analista de seguridad.

---

## 🔬 Lo que hice

### 1. Captura de tráfico DNS
Ejecuté `ping -c 3 google.com` mientras capturaba tráfico en la interfaz `enp0s3`.  
Filtré con `dns` en Wireshark y observé el ciclo completo de consulta y respuesta.

**Hallazgo:** Los paquetes de consulta salen desde `10.0.2.15` hacia `9.9.9.9` (servidor DNS).  
La respuesta incluye las IPs reales de Google: `142.251.0.100`, `142.251.0.138`.

### 2. Análisis del TCP Handshake
Ejecuté `curl https://google.com` y filtré con `tcp`.  
Identifiqué los 3 paquetes del handshake:

| Paquete | Dirección | Flag | Significado |
|---|---|---|---|
| 22 | Mi PC → Servidor | SYN | "¿Podemos hablar?" |
| 23 | Servidor → Mi PC | SYN-ACK | "Sí, ¿tú también estás listo?" |
| 24 | Mi PC → Servidor | ACK | "Listo, empecemos." |

### 3. Puerto 80 vs Puerto 443
Observé tráfico TLSv1.3 en el puerto 443, confirmando cifrado activo.

---

## 🚨 Vectores de ataque identificados

| Protocolo | Ataque | Impacto |
|---|---|---|
| DNS | DNS Spoofing | Redirige usuarios a sitios maliciosos falsos |
| TCP | SYN Flood | Colapsa el servidor agotando su memoria |
| HTTP (puerto 80) | Sniffing | Credenciales viajan en texto plano, legibles |

---

## 💡 Conclusión

Un servidor web escuchando en puerto 80 representa un riesgo crítico en cualquier auditoría: el tráfico viaja sin cifrado y cualquier interceptación expone credenciales en texto plano.  
**Recomendación:** Migrar a HTTPS puerto 443 con TLS 1.2 mínimo.

---

## 📸 Evidencia

<img width="1866" height="899" alt="image" src="https://github.com/user-attachments/assets/bd705365-3bb4-4208-bbde-95c48042f87b" />
<img width="1874" height="901" alt="image" src="https://github.com/user-attachments/assets/457be01f-cc03-4cb7-aadc-b08604d897d9" />


