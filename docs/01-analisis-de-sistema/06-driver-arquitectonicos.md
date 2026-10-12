# Drivers arquitectónicos del Marketplace

Los drivers arquitectónicos son las necesidades y condiciones que tienen
una influencia significativa en las decisiones de arquitectura.

Se identifican a partir del análisis realizado previamente, especialmente
de los atributos de calidad y las restricciones.

| ID | Driver arquitectónico | Origen | ¿Por qué influye en la arquitectura? |
|----|-----------------------|--------|-------------------------------------|
| DA01 | El sistema debe soportar un incremento importante de usuarios durante campañas comerciales. | AC03 — Escalabilidad | Puede influir en la estrategia de escalamiento y despliegue del sistema. |
| DA02 | El sistema debe mantener tiempos de respuesta adecuados cuando muchos usuarios lo utilizan simultáneamente. | AC01 — Rendimiento | Puede influir en la comunicación entre componentes, el procesamiento y el almacenamiento de información. |
| DA03 | El sistema debe proteger los datos de los usuarios y las operaciones de compra. | AC04 — Seguridad | Puede influir en la autenticación, la autorización y la protección de datos. |
| DA04 | El sistema debe integrarse con una pasarela de pago externa mediante una API. | RC04 — Pasarela de pago | Condiciona la forma de comunicación e integración con el servicio externo de pago. |
| DA05 | El sistema debe utilizar una API REST para la comunicación entre el frontend y el backend. | RC03 — API REST | Limita las alternativas de comunicación entre estas partes del sistema. |
| DA06 | El sistema debe permitir modificar funcionalidades sin afectar innecesariamente otros módulos. | AC05 — Mantenibilidad | Influye en la separación de responsabilidades, la modularidad y el control de las dependencias internas. |