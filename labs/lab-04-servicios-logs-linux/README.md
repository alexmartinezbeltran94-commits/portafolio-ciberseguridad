# Lab 04 — Análisis de Servicios y Logs en Linux

**Fecha:** Junio 2026  
**Herramientas:** systemctl, /var/log/auth.log, Ubuntu (VirtualBox)  
**Dificultad:** Básico-Intermedio  

---

## 🎯 Objetivo

Identificar servicios activos en el sistema y analizar logs de autenticación
para detectar actividad sospechosa, como un analista SOC en su trabajo diario.

---

## 🔬 Lo que hice

### 1. Auditoría de servicios con systemctl
Ejecuté `systemctl list-units --type=service --state=running` para listar
todos los servicios activos en el sistema.

**Por qué importa en seguridad:**
Un atacante puede instalar un servicio malicioso con nombre inocente
(ej: `system-update.service`) que se ejecuta automáticamente al inicio
con permisos elevados. Auditar la lista de servicios es detección temprana.

### 2. Análisis de logs de autenticación
Ejecuté `cat /var/log/auth.log | tail -50` para revisar los últimos
50 eventos de autenticación del sistema.

**Hallazgos en mi sistema:**

| Evento | Significado | ¿Normal? |
|---|---|---|
| `session opened for user root by CRON` | Tarea automática del sistema | ✅ Normal |
| `alex: Executing command [USER=root]` | Usuario alex usó sudo | ✅ Normal — fui yo con nmap |
| `session opened / session closed` | Mis propios logins | ✅ Normal |

**Hallazgo clave:** Cada vez que se usa `sudo`, el log registra timestamp exacto,
usuario, y comando completo. Esto es evidencia forense en caso de incidente.

---

## 🚨 Cuándo esto se convierte en alerta SOC

| Patrón en el log | Qué significa |
|---|---|
| `Failed password` repetido cientos de veces | Ataque de fuerza bruta en progreso |
| `USER=root` ejecutado por usuario desconocido | Escalación de privilegios |
| `New user added` inesperado | Atacante creó backdoor |
| `session opened` a las 3am desde IP externa | Acceso no autorizado |

---

## 💡 Conclusión

Los logs de autenticación son la memoria del sistema. Un analista SOC
que sabe leerlos puede reconstruir exactamente qué pasó, cuándo, y quién lo hizo.
La diferencia entre un incidente detectado y uno que pasa desapercibido
muchas veces está en si alguien revisó estos logs a tiempo.

---

## 📸 Evidencia

<img width="1869" height="843" alt="image" src="https://github.com/user-attachments/assets/75fbc7a4-7f87-4683-a9db-5824b46e614a" />
