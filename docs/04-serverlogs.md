La administracion del servidor ha sido una cuestión de leer pero mas allá de caer en el estereotipo de leer libros la forma mas rápida
que he hallado para aprender sobre bot es por medio de lectura de logs de forma periódica. Esto me ha permitido de cierta forma entender
como "piensan" los atacantes.
```bash
Mis log mas comunes al tener los puertos 443 y 853 expuestos a internet:
failregex = ^.*TLS handshake error from <HOST>:.*tls: client offered only unsupported versions.*$
        ^.*TLS handshake error from <HOST>:.*tls: client requested unsupported application protocols.*$
        ^.*TLS handshake error from <HOST>:.*tls: no cipher suite supported by both client and server server=https.*$
        ^.*TLS handshake error from <HOST>:.*tls: first record does not look like a TLS handshake server=https.*$
        ^.*TLS handshake error from <HOST>:.*client sent an HTTP request to an HTTPS server server=https.*$
        ^.*TLS handshake error from <HOST>:.*remote error: tls: unknown certificate server=https.*$
        ^.*TLS handshake error from <HOST>:.*remote error: tls: unexpected message server=https.*$
        ^.*error reading preface from client <HOST>:.*bogus greeting.*$
        ^.*TLS handshake error from <HOST>:.*remote error: tls: bad certificate server=https.*$
        .*\[error\] POST \S+ /control/login: from ip <HOST>: invalid username or password
        ^.*TLS handshake error from <HOST>:.*tls: client requested unsupported application protocols ([http/0.9 http/1.0 spdy/1 spdy/2 spdy/3 h2c hq]) server=https.*$
        ^.*auth: raddr <HOST>:\d+: invalid basic authorization value.*$
        ^.*http2: server: error reading preface from client <HOST>:\d+: bogus greeting "(GET|POST|HEAD|OPTIONS|PUT|DELETE|TRACE|CONNECT) [^"]+" server=https.*
        ^.*TLS handshake error from <HOST>:.*no cipher suite supported by both client and server.*
         .*TLS handshake error from <HOST>:[0-9]+: tls: missing signature_algorithm
        TLS handshake error from <HOST>:\d+: tls: unsupported SSLv2 handshake received server=https
        TLS handshake error from <HOST>:\d+: tls: missing signature_algorithms from TLS 1\.2 peer server=https
        ^.*AdGuardHome\[\d+\]: .*TLS handshake error from <HOST>:\d+: tls: no key exchanges supported by both client and server server=https$

```bash

Los log estan en formato REGEX para  herramientas como Fail2ban. Cada una de estas lineas no se redacto en una sola sesión sino que
a lo largo de los años se fue puliendo y su efectividad puedo decir es del 96% de detecciones de bots.
