Muchos bots en internet usan librerías o stack muy viejos o mal configurados y pretenden hacer handshake con ellos.

En los logs veras algo como: [16 33 67 c09e c0a2 9e 39 6b c09f c0a3 9f 45 be 88 c4 9a c008 c009 c023 ...]
Estos corresponden a numeros hexadecimales de cifrados que en el ejemplo son obsoletos o inseguros y que
por diseño cualquier server con su certificado deberá rechazarlos.

 AdGuard Home (cifrado activo) está recibiendo conexiones TLS de:

🤖 Bots de escaneo

🔍 Escáneres de puertos

🛡 Sistemas automáticos de mapeo de seguridad

🧪 Herramientas que prueban compatibilidad SSL/TLS

Estos bots envían una lista enorme de cipher suites modernos:

AES-GCM 

CHACHA20 
ECDHE 
TLS 1.2 / 1.3 

Y tu servidor responde:

❌ no cipher suite supported by both client and server

Es decir:

Los bots prueban:

Si aceptas TLS 1.0 / 1.1

Si aceptas RC4, 3DES, MD5

Si tienes fallback inseguro

Si usas certificados débiles

Si hay downgrade attacks posibles

Como tu servidor los bloquea, dejan ese error en el log.

 Eso es una buena señal de seguridad, no una mala.
