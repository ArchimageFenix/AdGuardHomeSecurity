# Introducción

Este repositorio nace a partir de una necesidad real: el temor inicial de exponer un servicio crítico (AdGuard Home) en un VPS público sin contar con guías claras y realistas orientadas a entornos pequeños, personales y de bajo presupuesto.

Hace aproximadamente dos años comencé a utilizar AdGuard Home en una VPS pública para uso principalmente hogareño y personal. Desde el inicio me enfrenté a una realidad evidente: cualquier servicio expuesto a Internet comienza a recibir escaneos, intentos de abuso, tráfico automatizado y comportamientos hostiles casi de inmediato.

Busqué guías que abordaran este escenario desde una perspectiva práctica, sin depender de soluciones empresariales como WAF comerciales, SIEM, firewalls de nueva generación o SOC dedicados. Al no encontrar documentación que reflejara exactamente este contexto, decidí comenzar a documentar mis propias experiencias, errores, ajustes y aprendizajes.

Este repositorio es el resultado de ese proceso. No es una guía teórica ni académica: todo lo que aquí se documenta proviene de experiencia real en producción, operando un DNS público durante más de dos años, bajo tráfico y ataques reales.

El objetivo principal es compartir un enfoque de seguridad práctico, replicable y eficiente para personas que, exponen servicios en VPS pequeños, homelabs o infraestructuras personales, priorizando:

- Seguridad realista
- Bajo consumo de recursos
- Herramientas disponibles en cualquier servidor Linux
- Automatización ligera
- Control total del entorno

Este proyecto también sirve como bitácora técnica personal, para no perder decisiones importantes, configuraciones críticas y lecciones aprendidas a lo largo del tiempo.

