En internet cuando un bot llega a unos de los puetos ya sea el 443 o el 853 deja algún rastro en los log de ubuntu, en este caso el journalctl
o diario de Ubuntu que registra todos los eventos del sistema.
En este caso cuando tienes una VPS con dns cifrado veras este tipo de error: 
```bash

[error] dnsproxy: reading msg proto=tcp err="reading len: read tcp IP:853->147.xxx.xxx.xx:xxxxx: read: connection reset by peer"
