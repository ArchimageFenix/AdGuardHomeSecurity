# Arquitectura general del entorno

Esta sección describe la arquitectura general utilizada a lo largo del tiempo para operar AdGuard Home de forma segura en una VPS pública, separando claramente las zonas públicas de las zonas privadas y reduciendo al máximo la superficie de ataque.

La arquitectura ha ido evolucionando con base en la experiencia real, pero siempre ha mantenido los mismos principios fundamentales:

- Separación de roles
- Exposición mínima
- Accesos administrativos aislados
- Resolución DNS controlada
- Uso de túneles privados cuando es necesario

---

## 1. Componentes principales

La infraestructura está compuesta por los siguientes elementos:

- VPS pública (zona expuesta a Internet)
- AdGuard Home (filtrado, estadísticas y control)
- Servicios DNS cifrados (DoT / DoH)
- Tailscale (túnel privado)
- Unbound (recursor DNS doméstico, en ciertos escenarios)
- Clientes finales (Android, red doméstica, dispositivos personales)

---

## 2. Zona pública (VPS)

La VPS cumple el rol de **frontera expuesta a Internet**. En esta zona se concentran todos los intentos de acceso desde el exterior.

En la VPS se exponen únicamente:

- Servicios DNS cifrados:
  - DNS over TLS (DoT)
  - DNS over HTTPS (DoH)
- Panel web de AdGuard Home (con protección adicional)

No se expone:

- Acceso SSH por Internet
- Servicios administrativos sin cifrado
- Puertos innecesarios

Toda la superficie pública está protegida mediante:

- Firewall del sistema
- Fail2ban
- ipset a nivel de kernel
- Reglas de TLS estrictas

---

## 3. Zona privada (acceso administrativo)

El acceso administrativo a la VPS se realiza exclusivamente mediante:

- SSH
- A través de Tailscale
- Autenticación solo mediante llaves

Esto implica que:

- El puerto 22 no está accesible desde Internet
- Solo dispositivos autenticados dentro de la red Tailscale pueden intentar iniciar sesión
- No existe autenticación por contraseña

De esta forma, el control administrativo queda completamente aislado del tráfico público.

---

## 4. Flujo general del tráfico DNS (modo estándar)

En el escenario más común, el flujo de resolución funciona así:


En este escenario:

- La VPS actúa como punto de entrada público
- Unbound realiza la recursión real
- La red doméstica nunca queda expuesta directamente al Internet público
- La comunicación VPS ↔ hogar viaja cifrada por Tailscale

Este modo fue especialmente útil para maximizar privacidad y control total de la resolución DNS.

---

## 6. Clientes finales

Los clientes acceden al servicio principalmente mediante:

- DNS privado en Android
- Dispositivos personales
- Red local del hogar
- Conectividad móvil

Los clientes siempre resuelven contra la VPS pública, incluso cuando la recursión real se encuentra en el entorno doméstico.

Esto permite:

- Compatibilidad total
- Control centralizado
- Filtrado unificado
- Estadísticas consolidadas

---

## 7. Principios de diseño aplicados

A nivel arquitectónico, todas las decisiones siguen estos principios:

- Exponer solo lo estrictamente necesario
- Separar tráfico público de control administrativo
- Aislar la recursión cuando sea posible
- Priorizar túneles privados sobre exposición directa
- Reducir superficie de ataque en cada capa
- Diseñar siempre asumiendo tráfico hostil por defecto

---

## 8. Evolución de la arquitectura

La arquitectura no ha sido estática. Con el tiempo se han realizado ajustes como:

- Endurecimiento progresivo de TLS
- Integración de ipset
- Automatización de análisis de logs

Cada cambio ha surgido como respuesta directa a eventos reales observados en producción.

