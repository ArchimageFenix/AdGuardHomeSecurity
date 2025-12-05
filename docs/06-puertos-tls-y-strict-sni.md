# Puertos expuestos, TLS estricto y Strict SNI

Uno de los pilares de la seguridad en un VPS público con recursos limitados es **minimizar al máximo la superficie de ataque**.  
En este entorno se tomó una decisión clara:

> Solo exponer los puertos **853 (DoT)** y **443 (DoH/HTTPS)**, ambos siempre bajo cifrado TLS, con validación estricta de SNI (Strict SNI).

Esto implica:

- No hay puerto 80 abierto.
- No hay puerto 53 UDP/TCP abierto directamente a Internet.
- No hay otros servicios accesibles desde el exterior.
- Todo el tráfico útil entra siempre encapsulado en TLS.

---

## 1. Puertos abiertos: 853 (DoT) y 443 (HTTPS/DoH)

### 1.1 Puerto 853 – DNS over TLS (DoT)

- Protocolo: DNS over TLS
- Uso: consultas DNS cifradas
- Ventaja: menor fingerprinting a nivel HTTP, enfoque puro en DNS
- Ideal para: clientes que soportan DoT nativamente

Abrir solo 853 para DoT permite:

- Separar el canal DNS cifrado del resto de servicios.
- Tener una ruta dedicada para clientes avanzados.

---

### 1.2 Puerto 443 – HTTPS / DNS over HTTPS (DoH)

- Protocolo: HTTPS (HTTP/1.1 o HTTP/2) con TLS
- Uso: panel web (si aplica) y/o DNS over HTTPS
- Ventaja: 443 es el puerto más común en Internet, más difícil de bloquear por terceros
- Ideal para: Android (DNS privado), navegadores y sistemas que solo admiten DoH

Exponer únicamente 443 como puerto “web” tiene varios beneficios:

- Compatibilidad máxima con casi cualquier red.
- Menos sospechoso para firewalls y redes intermedias.
- Posibilidad de aprovechar **DNS sobre HTTPS** sin puertos raros.

---

## 2. Por qué NO exponer otros puertos

Se decidió **no exponer**:

- Puerto 22 (SSH) → solo accesible por Tailscale.
- Puerto 80 (HTTP sin cifrar).
- Puerto 53 (DNS tradicional) directamente a Internet.
- Cualquier otro servicio administrativo.

Motivos:

- Reducir el número de vectores explotables.
- Bloquear ataques triviales de fuerza bruta en SSH.
- Evitar que el servidor pueda ser usado como amplificador DNS abierto.
- Forzar que todo tráfico útil pase por una capa de cifrado obligatoria.

---

## 3. Cifrado obligatorio (TLS como primera línea de defensa)

Tanto en 853 como en 443, el tráfico se establece siempre bajo TLS:

- Certificados válidos (por ejemplo, Let’s Encrypt).
- Protocolos modernos (priorizando TLS 1.3).
- Ciphers fuertes (CHACHA20, AES-GCM, etc. según el cliente).

Beneficios:

- Confidencialidad de las consultas DNS.
- Integridad de los datos en tránsito.
- Dificultad adicional para inspección y manipulación por terceros.
- Más resistencia frente a escaneos superficiales.

---

## 4. ¿Qué es Strict SNI y por qué usarlo?

En las conexiones TLS, el cliente envía un campo llamado **SNI (Server Name Indication)** que indica el nombre del host al que quiere conectarse (ejemplo: `dns.midominio.tld`).

**Strict SNI** es una política defensiva que consiste en:

- Rechazar activamente cualquier conexión TLS que:
  - No tenga SNI.
  - Tenga SNI inválido.
  - Tenga SNI que no coincida con los nombres esperados.
- No entregar ningún certificado útil si el SNI no es correcto.
- Cortar la sesión lo antes posible.

En la práctica:

> Si el cliente no sabe qué nombre debe pedir,  
> el servidor no le ofrece nada útil.

---

## 5. Beneficios prácticos de Strict SNI

1. **Filtro temprano de bots y escáneres**  
   La mayoría de herramientas de escaneo masivo:
   - No envían SNI válido.
   - Envian valores genéricos o vacíos.
   - Prueban combinaciones sin sentido.

   Con Strict SNI:

   - La conexión se rechaza antes de llegar a capas más costosas.
   - Se reduce el impacto en CPU.
   - Muchas herramientas automatizadas “ven” el servicio como no útil.

---

2. **Reducción de superficie de fingerprinting**

Sin Strict SNI:

- Un escáner puede establecer handshake completo.
- Ver el certificado.
- Obtener información de:
  - host
  - CA
  - tiempos de expiración
  - comportamientos específicos

Con Strict SNI:

- Si el SNI no coincide, se corta el handshake.
- No se revela certificado útil.
- No se entrega información adicional sobre el servicio.

---

3. **Alineación con un diseño de acceso controlado**

El servidor asume que:

- Solo clientes que conocen el dominio correcto deben poder usar el servicio.
- Cualquier otro intento es considerado sospechoso o irrelevante.

Esto encaja con un modelo de seguridad donde:

- El servicio DNS cifrado **no está pensado para uso público generalizado**.
- Solo debe ser usado por dispositivos configurados explícitamente (por ejemplo, tus propios clientes Android).

---

## 6. Impacto en compatibilidad

La aplicación de Strict SNI tiene consecuencias:

### Ventajas

- Mayor protección frente a escaneo masivo.
- Menor exposición de certificados.
- Filtrado de tráfico basura de forma temprana.
- Ahorro de recursos en el servidor.

### Desventajas

- Clientes muy antiguos o mal implementados que:
  - No envían SNI.
  - O lo hacen de forma incorrecta.
  
  Pueden quedar excluidos.

En este entorno específico (uso personal/hogareño con control de los clientes), esta desventaja es aceptable, porque:

- Los dispositivos usados (móviles, PCs, navegadores modernos) **sí soportan SNI correctamente**.
- No se busca compatibilidad con hardware legacy o entornos desconocidos.

---

## 7. Puertos, TLS y Strict SNI como estrategia combinada

En conjunto, la política es:

- Exponer solo 853 (DoT) y 443 (DoH/HTTPS).
- Obligar siempre a TLS.
- Aplicar Strict SNI.
- Bloquear todo lo demás a nivel de firewall.

Esto ofrece:

- Una superficie de ataque extremadamente reducida.
- Una clara frontera entre:
  - tráfico legítimo (clientes configurados)
  - tráfico hostil o desconocido (todo lo demás)
- Integración perfecta con otras capas defensivas:
  - fail2ban
  - ipset
  - análisis de logs
  - túneles privados (Tailscale para administración)

---

## 8. Relación con el resto de la guía

Esta decisión de diseño no se entiende de forma aislada, sino junto con:

- El modelo de amenazas descrito previamente.
- La arquitectura general (VPS pública + hogar + Tailscale + Unbound).
- La protección de SSH exclusivamente vía túnel y llaves.
- La automatización de análisis de logs y bans.

La exposición minimalista de puertos (853 y 443 únicamente, con TLS y Strict SNI) es uno de los componentes centrales de la estrategia de hardening documentada en este proyecto.
