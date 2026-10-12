# ADR-003: Adoptar una estrategia de caché para el catálogo

- **Estado:** Aceptado para el diseño inicial.
- **Fecha:** 2026-10-11.

## Contexto

Durante campañas comerciales, muchos usuarios pueden consultar
repetidamente los mismos productos del Marketplace.

Estas consultas pueden aumentar la carga sobre la fuente de datos
y afectar los tiempos de respuesta.

Esta decisión responde a DA02 — Rendimiento, cuyo origen es
AC01 — Rendimiento.

## Decisión

Aplicar una estrategia cache-aside a las consultas públicas
frecuentes del catálogo, mediante componentes de Infraestructura.

La caché almacenará temporalmente resultados de consulta.
Cada entrada se identificará mediante una clave que considere
la consulta y sus filtros.

## Funcionamiento

1. Consultar si existe una entrada vigente en la caché.
2. Si existe, devolver el resultado almacenado.
3. Si falta o ha caducado, consultar la fuente de datos.
4. Almacenar temporalmente el resultado y devolverlo.

Las entradas tendrán un tiempo de vida configurable, ajustado
según la frecuencia de actualización y las mediciones de rendimiento.

Después de actualizar un producto, su precio o su stock en la fuente,
se invalidarán las entradas de catálogo afectadas.

Si la caché falla, se intentará consultar directamente la fuente.

## Precio y stock durante una compra

La información mostrada en el catálogo puede quedar temporalmente
desactualizada.

Antes de confirmar una compra, el backend deberá verificar el precio
y el stock vigentes en la fuente autorizada.

El acceso utilizado por RegistrarCompra realizará esa verificación
directamente, mientras las consultas del catálogo podrán utilizar
el adaptador con caché.

## Relación con Clean Architecture

La implementación de la caché permanecerá en Infraestructura,
detrás de los contratos de acceso a productos.

Dominio y Aplicación mantendrán su independencia respecto
de la tecnología de caché.

La raíz de composición seleccionará los adaptadores utilizados
por cada caso de uso, conforme a ADR-002.

## Justificación

La estrategia busca reducir consultas repetitivas a la fuente
y mejorar los tiempos de respuesta del catálogo.

Su beneficio deberá comprobarse comparando tiempos de respuesta
y cantidad de consultas a la fuente con y sin caché.

## Consecuencias

- Será necesario gestionar caducidad e invalidación.
- Deberá limitarse el almacenamiento utilizado por la caché.
- Podrán existir datos temporalmente desactualizados.
- Si se despliegan varias instancias del backend, se evaluará
  una caché compartida.
- La tecnología y los valores de configuración se definirán
  según las mediciones y necesidades del sistema.

## Alternativa considerada

Consultar directamente la fuente en cada solicitud.

Esta opción simplifica la gestión de datos actualizados,
pero mantiene el costo de las consultas repetidas durante
periodos de alta demanda.

## Relación con el boilerplate

Los repositorios en memoria del ejemplo actúan como una fuente
de datos simulada.

La estrategia documentada corresponde al diseño propuesto
para las consultas del backend del Marketplace.