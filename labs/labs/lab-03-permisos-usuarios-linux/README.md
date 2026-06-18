Lab 03 — Gestión de Permisos y Usuarios en Linux
Fecha: Junio 2026
Herramientas: chmod, chown, /etc/passwd, Ubuntu (VirtualBox)
Dificultad: Básico

🎯 Objetivo
Auditar usuarios del sistema, identificar cuentas con acceso de shell activo y aplicar permisos correctos a archivos sensibles como parte de un hardening básico.

🔬 Lo que hice
1. Auditoría de usuarios con /etc/passwd
Ejecuté cat /etc/passwd para listar todos los usuarios del sistema.

Formato de cada línea: usuario:contraseña:UID:GID:descripción:directorio:shell Hallazgo clave: Solo dos usuarios tienen /bin/bash al final, lo que significa que pueden iniciar sesión interactiva:

root — administrador del sistema
alex — usuario personal
El resto tiene /usr/sbin/nologin o /bin/false, lo que les impide iniciar sesión. Esto es correcto por diseño.

Riesgo: Si un atacante crea un usuario nuevo con /bin/bash, aparecerá en este archivo. Auditarlo periódicamente es una tarea SOC básica.

2. Gestión de permisos con chmod
Comando	Permisos	Uso seguro
chmod 600 archivo.txt	Solo dueño lee y escribe	Archivos privados, claves SSH
chmod 644 config.php	Dueño escribe, todos leen	Archivos web estándar
chmod 777 script.sh	Todos tienen control total	⚠️ Nunca en producción
3. Cambio de propietario con chown
chown root archivo_critico.log
Transfiere la propiedad al administrador. Nadie más puede modificarlo sin escalar privilegios.

🚨 Vectores de ataque identificados
Hallazgo	Riesgo	Acción recomendada
Usuario con /bin/bash no autorizado	Acceso persistente del atacante	Eliminar o cambiar a nologin
Archivo con chmod 777	Cualquier proceso puede modificarlo	Restringir a 600 o 644
Archivo crítico sin chown root	Usuario no privilegiado puede alterarlo	Transferir propiedad a root
💡 Conclusión
La auditoría de usuarios y permisos es la primera línea de defensa en Linux. Un atacante que logra crear un usuario con shell activo o modificar un archivo con permisos 777 puede mantener acceso persistente sin ser detectado fácilmente.

📸 Evidencia
