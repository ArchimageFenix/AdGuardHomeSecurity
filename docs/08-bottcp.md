En internet cuando un bot llega a unos de los puetos ya sea el 443 o el 853 deja algún rastro en los log de ubuntu, en este caso el journalctl o diario de Ubuntu.
En este caso cuando tienes una VPS con dns cifrado veras este tipo de error: 
```bash

[error] dnsproxy: reading msg proto=tcp err="reading len: read tcp IP:853->147.xxx.xxx.xx:xxxxx: read: connection reset by peer"

Significa que el bot no vio nada interesante o no pudo continuar la conexión TCP y simplemente envio un RST o reset de conexión.
Acá AdGuardHome estuvo leyendo los datos pero el otro lado o cliente no continuo y corta de tajo.

No hay mas que decir que simplemente tu servidor no fue atacado ni nada solo fue un intento fugaz de conexion fallido.


El connection reset by peer viene del cliente/bot.
 Probablemente el bot:

probó conectar,

no logró lo que quería (TLS, fingerprint, vulnerabilidad, etc.),

abortó feo con un RST.

Tú estás viéndolo como:

“Mi servidor intentó leer más datos y el otro lado colgó de golpe.”

Que a nivel práctico se traduce como:

“No consiguió nada útil y se fue.”

