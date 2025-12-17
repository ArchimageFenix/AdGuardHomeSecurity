# AdGuardHomeSecurity
AdGuard Home Security Backend


# 🛡️ Guía de Seguridad para AdGuard Home Público (DoT/DoH)
## Endurecimiento basado en TLS, Análisis de Logs y Mitigación Automática

Esta guía describe un modelo de seguridad avanzado para **AdGuard Home expuesto en Internet** como servidor de DNS Cifrado (DNS-over-TLS / DNS-over-HTTPS). Se entiende que existen herramientas mas avanzadas en el mercado para protección de servidores sin embargo es bien sabido que no todos pueden costear WAF o soluciones cloud. Por tanto esto es mas que todo guia, no reemplazo de otras capas de seguridad del mercado.

La arquitectura está basada en:
- **TLS 1.3 estricto**
- **Strict-SNI**
- **Análisis de logs TLS**
- **Fail2ban con reglas específicas**
- **ipset a nivel kernel**
- **Rate-limit**
- **Allowlist geográfico**
- **Reporte automático a AbuseIPDB**
- **Acceso administrativo seguro**


<img width="1536" height="1024" alt="core design" src="https://github.com/user-attachments/assets/4e05b8b7-50ca-4bc0-bdbe-318e611e9763" />



Funciona en producción con junto a Unbound DNS con excelentes resultados:  
entre **120, 170 hasta 200 IP hostile    s bloqueadas** regularmente, con 0 falsos positivos.
Además de Recursión autónoma con baja latencia

---

# 1. Puertos Mínimos Expuestos

Solo abrir:
| Puerto | Descripción |
|--------|------------|
| 443/tcp | DNS-over-HTTPS |
| 853/tcp | DNS-over-TLS |

Nota: NO ABRIR BAJO NINGUNA CIRCUNSTANCIA EL PUERTO 53 en UDP para evitar ataques de amplificacion o usos como Open Resolver.
Todo lo demás:
DROP


Esto elimina:
- fingerprinting
- exploración masiva
- escaneos automatizados

---

# 2. Seguridad TLS Estricta

**Solo TLS 1.3**  
Sin compatibilidad con TLS 1.2 o inferiores.

### Cipher suites recomendadas:
- TLS_CHACHA20_POLY1305_SHA256
- TLS_AES_256_GCM_SHA384
- TLS_AES_128_GCM_SHA256

Beneficios:
- forward secrecy obligatoria
- rechazo a implementaciones obsoletas
- incompatibilidad con librerías antiguas usadas por bots

---

# 3. Strict-SNI Activado

Si el cliente no envía un SNI válido:
- **el handshake se aborta**
- no se negocia certificado

Los bots, scanners y fuzzers suelen omitir este campo → son detectados automáticamente.

---

# 4. Backend TLS en Go

AdGuard Home usa la librería `crypto/tls`, que valida:
- ClientHello
- certificados
- firmas
- curvas
- extensiones TLS
- estructura del handshake

Cuando el cliente presenta anomalías, aparecen logs claros como:
- `tls_signature error`
- `tls bad certificate`
- `doesn't look like a TLS handshake`

Estos logs **no los generan clientes legítimos**.  
Son huellas directas de bots, scanners, malware o librerías rotas.

---

# 5. Acceso Administrativo Seguro

### Panel web:
- Contraseña fuerte generada por keepassxc (>15 caracteres)
- Ban al primer intento con fail2ban
- Cooldown  interno de AdGuard Home bloqueara aquellos intentos repetidos de otras ip que no sean baneadas al instante por fail2ban

### SSH:
- No está expuesto a Internet
- Acceso solo por VPN privada (Tailscale)
- Autenticación por llave SSH (sin contraseña)

Resultado:
**Imposible atacar por fuerza bruta.**

---

# 6. Fail2ban como IDS basado en TLS

Las reglas NO se basan en intentos de login.  
Se basan en **comportamiento criptográfico anómalo**.

Eventos disparadores:
- handshake inválido
- certificados corruptos
- SNI faltante
- firmas incompatibles
- datos no-TLS en puertos TLS

Ejemplos:

tls_signature error
tls bad certificate
doesn't look like a TLS handshake


Se crean expresiones que:
- detectan textos exactos
- capturan solo eventos TLS hostiles reales
- reducen falsos positivos casi a cero

Todas probadas con: DROP no con REJECT para evitar consumo de CPU en respuestas a los BOT


---

# 8. Doble Defensa: Auth Interno + Bloqueo Criptográfico

Cuando hay intentos al panel, AdGuard Home desde su modulo "auth" puede emitir:
> `blocked for 60 min`

Pero aunque no muestre la IP ahí,
si el cliente produjo anomalías TLS:
→ **Fail2ban lo banea igualmente**

Esto significa que el bloqueo ocurre:
- antes del login,
- antes de HTTP,
- incluso antes de finalizar handshake,

exactamente como lo hacen WAFs avanzados:
- Cloudflare
- Akamai
- Imperva

Es seguridad **por protocolo**, no por URL.

---

# 9. Ban inmediato

Debido a la naturaleza inequívoca de estos logs:
- `maxretry = 1`
- bans largos de 3 días o más

No hay razón para permitir segundas oportunidades a clientes que ni siquiera logran un handshake TLS correcto.

---

# 10. ipset: bloqueo a nivel kernel

Fail2ban envía las IP directamente a `ipset`.

Ventajas:
- mínimo consumo de recursos
- bloqueo sin negociación
- cero impacto en CPU

El servidor no malgasta tiempo lidiando varias veces con el mismo bot.

---

# 11. Rate-limit y geo-allowlist

### Rate Limit:
~22 consultas por segundo

### Allowlist:
solo rangos confiables del país

Esto reduce hasta un **90% del ruido hostil**.

---

# 12. Reporte automático a AbuseIPDB (opcional)

Cada IP hostil:
1. se banea
2. se reporta

Esto:
- contribuye a la reputación global
- degrada utilidad de IPs botnet
- favorece a otros servidores afectados

---

# 13. Realidad actual en IPv4

AdGuard Home público en IPv4:
- siempre será detectado
- siempre será escaneado
- siempre será probado como objetivo

Porque IPv4:
- es pequeño
- se puede mapear completo
- es blanco estadístico

Por eso es normal ver:
**120–170 IPs bloqueadas**  
de forma recurrente.

---

# 14. El Futuro con IPv6

Cuando IPv6 se adopte de forma total:

## El escaneo global dejará de ser viable
El espacio IPv6 es tan enorme que:
- no se puede enumerar
- no se pueden barrer rangos completos
- AdGuard Home sería “invisible” para bots

Pero ojo:

### Los ataques que sí ocurran serán dirigidos
Eso significa:
- el atacante ya conoce nuestra IP
- los intentos serán sofisticados
- habrá análisis más profundo
- existirá mayor preparación

**La amenaza será menor en volumen  
pero mayor en calidad y peligro.**

Por eso la estrategia actual debe mantenerse:
- detección criptográfica
- Strict-SNI
- TLS 1.3
- fail2ban aplicado a logs TLS

**IPv6 no elimina el riesgo: cambia su forma.**

---

# 15. Conclusión

Este modelo convierte AdGuard Home en un DNS cifrado público:
- difícil de mapear
- duro de escanear
- resistente a bots
- auto-defensivo
- protegido por protocolo

Mientras IPv4 siga vigente:
- somos blanco fácil y detectable,
- pero este diseño mitiga y responde con precisión.

Cuando IPv6 domine:
- desaparecerá el ruido masivo,
- pero los ataques serán más dirigidos,
- y estas defensas serán todavía más importantes.

Si aplicamos este enfoque probado por dos años:
- AdGuard Home sería un objetivo más duro en todo el mundo
- las botnets verían reducida su eficacia
- la comunidad subiría el nivel base de seguridad

> **Somos un blanco visible hoy,  
pero podemos ser un blanco blindado mañana.**

About This Guide

This hardening guide is based on over 2 years of real-world experience running AdGuard Home on a public VPS exposed to the Internet.
All recommendations are derived from observed attack patterns, scanning behavior, abuse attempts, and operational incidents, not from theoretical assumptions.
