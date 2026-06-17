# Lab 02 — Auditoría de Puertos y Servicios en Linux

**Fecha:** Junio 2026  
**Herramientas:** ss, nmap, Ubuntu (VirtualBox)  
**Dificultad:** Básico  

---

## 🎯 Objetivo

Identificar qué puertos y servicios están expuestos en un sistema Linux,
distinguir niveles de exposición y detectar servicios potencialmente peligrosos.

---

## 🔬 Lo que hice

### 1. Auditoría de puertos con ss
Ejecuté `ss -tuln` para listar todos los puertos en escucha.

**Flags utilizados:**
- `-t` → tráfico TCP
- `-u` → tráfico UDP
- `-l` → solo puertos en escucha (listening)
- `-n` → formato numérico, sin resolución DNS

### 2. Niveles de exposición identificados

| Dirección | Significado | Riesgo |
|---|---|---|
| `127.0.0.1:puerto` | Solo accesible localmente | Bajo — aislado del exterior |
| `0.0.0.0:puerto` | Expuesto en todas las interfaces | Alto — visible en la red |

### 3. Escaneo con Nmap
Ejecuté `nmap -sV 127.0.0.1` para detectar versiones exactas de servicios.

**Estado `filtered`:** El firewall descartó los paquetes de prueba,
impidiendo determinar si el puerto está abierto o cerrado.

---

## 🚨 Vectores de ataque identificados

| Hallazgo | Riesgo | Acción recomendada |
|---|---|---|
| Servicio en `0.0.0.0` innecesario | Superficie de ataque expuesta | Restringir a `127.0.0.1` o cerrar |
| Versión de servicio desactualizada | CVE explotable conocido | Actualizar o parchear |
| Puerto 80 abierto | Tráfico sin cifrado | Migrar a 443 con TLS |

---

## 💡 Conclusión

Un atacante con acceso a la red puede escanear estos mismos puertos con Nmap
y buscar versiones vulnerables en bases de datos públicas como CVE o Exploit-DB.
La auditoría periódica de puertos es una tarea básica de hardening en cualquier servidor.

---

## 📸 Evidencia

<img width="1876" height="846" alt="image" src="https://github.com/user-attachments/assets/766698c6-96c5-455f-b829-81ad693c5c18" />
<img width="1863" height="843" alt="image" src="https://github.com/user-attachments/assets/f6700562-faeb-4299-9e2b-28c65ae3af24" />

