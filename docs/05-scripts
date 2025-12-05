# Correlación de logs por IP para auditar bans de fail2ban

Una de las necesidades más importantes cuando se opera un servidor expuesto a Internet es entender **exactamente qué hizo una IP antes de ser bloqueada**.  
No basta con saber que *fail2ban* baneó una dirección: es crucial poder ver:

- Qué servicios tocó
- Qué errores generó
- En qué momento
- Qué línea de log coincidió con la regla de *fail2ban*

Esta sección describe un enfoque práctico para **correlacionar logs por IP** usando `journalctl` y un script en Bash, de forma que sea posible auditar y depurar bans en un VPS con recursos limitados.

---

## Objetivo del script

El script tiene como finalidad:

- Pedir una IP al usuario.
- Opcionalmente, filtrar por rango de fechas.
- Consultar `journalctl` en orden inverso (de lo más reciente a lo más antiguo).
- Filtrar todas las líneas donde aparezca esa IP.
- Mostrar:
  - Fecha y hora
  - Servicio
  - Mensaje de log
- Ayudar a identificar:
  - Comportamiento hostil repetido
  - Errores TLS, HTTP, etc.
  - La línea exacta que activó el ban de *fail2ban*.

Este enfoque es especialmente útil para:

- Validar que una IP fue correctamente baneada.
- Detectar falsos positivos.
- Afinar regex y jails de *fail2ban*.
- Entender mejor los patrones de ataque.

---

## Script de ejemplo: análisis de logs por IP
Este script completo lo pueden hallar en la carpeta script llamado "informeipsh"

