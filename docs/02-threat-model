# Threat model (modelo de amenazas)

Este proyecto parte de un modelo de amenazas basado exclusivamente en observación real de tráfico malicioso durante la operación de un DNS público en una VPS.

No se basa en supuestos teóricos, sino en patrones repetitivos detectados en producción.

---

## Principales amenazas observadas

### 1. Escaneo masivo de IPv4

- Escaneo constante de rangos completos de IP
- Detección automática de servicios expuestos
- Fingerprinting de puertos abiertos
- Pruebas de protocolos y versiones

Este tipo de escaneo es permanente y completamente automatizado.

---

### 2. Bots automatizados

- Bots que prueban rutas web inexistentes
- Bots que generan errores TLS intencionalmente
- Bots que intentan forzar comportamientos anómalos del servidor
- Bots que prueban miles de combinaciones por segundo

---

### 3. TLS fuzzing y handshakes inválidos

- Conexiones con:
  - SNI inexistente
  - Certificados corruptos
  - Versiones TLS incompatibles
  - Protocolos malformados

Este tipo de tráfico no es accidental; es claramente automatizado para reconocimiento y detección de fallos.

---

### 4. Abuso de DNS público

- Intentos de usar el DNS como:
  - Reflector de ataques
  - Amplificador
  - Proxy de resolución para bots

---

### 5. Fuerza bruta (principalmente en accesos web)

- Intentos repetidos de login al panel de administración
- Pruebas de credenciales comunes
- Enumeración de rutas administrativas

---

## Principios defensivos derivados del threat model

A partir de estas amenazas, se adoptaron los siguientes principios:

- Minimizar superficie de ataque
- Exponer solo lo estrictamente necesario
- Mover accesos administrativos a zonas privadas
- Bloquear lo antes posible en kernel (ipset)
- No confiar en validaciones suaves
- Priorizar el rechazo temprano del tráfico anómalo
- Asumir siempre comportamiento hostil por defecto

---

## Enfoque general

Este proyecto adopta un enfoque de:

- Defensa en profundidad (defense in depth)
- Zero Trust práctico
- Bloqueo temprano
- Observabilidad ligera
- Automatización defensiva

Todo lo que se documenta en los siguientes capítulos se deriva directamente de este modelo de amenazas.
