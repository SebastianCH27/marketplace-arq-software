# Restricciones del Marketplace

Las restricciones establecen condiciones que deben respetarse al tomar
decisiones sobre el diseño y la arquitectura del sistema.

## Restricciones presentadas en la guía

| ID | Restricción | Descripción |
|----|-------------|-------------|
| RC01 | Aplicación web | El sistema debe desarrollarse como una aplicación accesible mediante un navegador web. |
| RC02 | Control de versiones | El código fuente debe gestionarse utilizando Git y mantenerse en un repositorio compartido. |
| RC03 | API REST | La comunicación entre el frontend y los servicios del sistema debe realizarse mediante una API REST. |
| RC04 | Pasarela de pago | El sistema debe integrarse con una pasarela de pago externa para procesar las operaciones de pago. |
| RC05 | Servicio de envío | El sistema debe integrarse con un servicio externo de envío para gestionar la información relacionada con la entrega de pedidos. |

## Restricciones adicionales identificadas

Estas restricciones se proponen a partir de los sistemas externos
identificados en el análisis de actores.

| ID | Restricción | Descripción |
|----|-------------|-------------|
| RC06 | Integración con ERP | La integración para obtener información de productos y stock debe ajustarse a las interfaces y formatos proporcionados por el ERP externo. |
| RC07 | Servicio de facturación | La integración para generar comprobantes de pago debe ajustarse a las interfaces y formatos proporcionados por el servicio externo de facturación. |