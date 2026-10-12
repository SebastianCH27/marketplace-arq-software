# ADR-004: Integrar pagos mediante interfaces y adaptadores

- **Estado:** Aceptado para el diseño inicial.
- **Fecha:** 2026-10-11.

## Contexto

El Marketplace debe integrarse con una pasarela de pago externa
mediante una API.

Los proveedores pueden utilizar diferentes solicitudes, respuestas
y mecanismos de autenticación. Estos detalles deben mantenerse
separados de las reglas y los casos de uso del Marketplace.

Esta decisión responde a DA04 — Integración con la pasarela de pago,
cuyo origen es RC04 — Pasarela de pago, y aplica el enfoque
definido en ADR-002.

## Decisión

Definir un contrato ProcesadorPagos en el dominio e implementar
adaptadores de pago en Infraestructura.

El caso de uso RegistrarCompra recibirá un procesador mediante
ese contrato.

| Elemento | Responsabilidad |
|----------|-----------------|
| Contrato ProcesadorPagos | Definir la operación de pago y el resultado que necesita el Marketplace. |
| Caso de uso RegistrarCompra | Coordinar la compra y utilizar el resultado del pago. |
| Adaptador de pagos | Implementar el contrato y traducir las solicitudes y respuestas del proveedor. |
| Raíz de composición | Seleccionar y conectar el adaptador utilizado. |

La integración real con la API de la pasarela se realizará
desde el backend. Las credenciales del proveedor se gestionarán
mediante la configuración del servidor.

## Funcionamiento

1. El backend verificará los datos de la compra y determinará
   el importe aplicando las reglas del negocio.
2. RegistrarCompra solicitará el pago mediante ProcesadorPagos.
3. El adaptador realizará la operación y traducirá su resultado
   al formato definido por el contrato.
4. El caso de uso continuará según el resultado recibido.

La compra solo podrá confirmarse como pagada cuando exista
una confirmación válida del proveedor.

Los errores de comunicación se distinguirán de los rechazos
confirmados, porque una respuesta perdida no demuestra
que el proveedor haya rechazado el pago.

## Justificación

La separación permite cambiar o incorporar proveedores manteniendo
el contrato cuando las necesidades del negocio sean las mismas.

También permite probar el caso de uso con un procesador simulado,
sin realizar cobros reales.

Los detalles de comunicación quedan localizados en el adaptador
correspondiente.

## Consecuencias

- Los casos de uso dependerán del contrato de pagos.
- Cada proveedor requerirá un adaptador que cumpla ese contrato.
- Los resultados y errores del proveedor deberán traducirse
  a resultados comprensibles para la aplicación.
- La integración real requerirá validar la configuración
  y el comportamiento de la pasarela seleccionada.
- Si se incorporan reintentos, deberán diseñarse para evitar
  que una misma compra produzca cobros duplicados.

## Alternativa considerada

Invocar directamente la API del proveedor desde RegistrarCompra.

Esta opción requiere menos abstracciones, pero los cambios
en la integración afectan directamente al caso de uso.

Se adopta la separación mediante contrato y adaptadores.

## Relación con el boilerplate

El ejemplo incluye procesador-pagos.contrato.ts dentro del dominio
y los adaptadores procesador-pagos-simulado.ts
y procesador-pagos-niubiz.ts dentro de Infraestructura.

app.config.ts permite seleccionar las implementaciones.

Las pruebas existentes verificaron una compra aprobada
y el rechazo de un pago mediante el procesador simulado.