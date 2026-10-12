# ADR-002: Adoptar Clean Architecture

- **Estado:** Aceptado para el diseño inicial.
- **Fecha:** 2026-10-11.

## Contexto

El Marketplace contiene reglas relacionadas con productos, stock,
carrito, precios, pedidos y compras.

Estas reglas deben mantenerse organizadas y poder probarse cuando
cambien la interfaz, el almacenamiento o los proveedores externos.

Esta decisión responde a DA06 — Mantenibilidad, cuyo origen es
AC05 — Mantenibilidad, y complementa el monolito modular definido
en ADR-001.

## Decisión

Adoptar Clean Architecture para organizar las responsabilidades
y controlar las dependencias internas.

Se definirán cuatro capas:

| Capa | Responsabilidad |
|------|-----------------|
| Dominio | Contener entidades, reglas de negocio y contratos necesarios para acceder a productos, pedidos, pagos y notificaciones. |
| Aplicación | Coordinar los casos de uso mediante las entidades y los contratos del dominio. |
| Infraestructura | Implementar los contratos mediante almacenamiento, API y adaptadores de servicios externos. |
| Presentación | Recibir solicitudes, invocar casos de uso y mostrar o devolver resultados. |

En el backend, Presentación incluirá los controladores de la API REST.
En el boilerplate, esta responsabilidad corresponde a los componentes
de la interfaz Angular.

## Regla de dependencias

Las dependencias del código apuntarán hacia el núcleo del negocio:

- Dominio no dependerá de las otras capas ni de frameworks.
- Aplicación dependerá del dominio.
- Infraestructura implementará los contratos definidos en el dominio.
- Presentación utilizará los casos de uso y los modelos necesarios
  para comunicar sus resultados.

Los casos de uso recibirán sus colaboradores mediante contratos.
Por ejemplo, RegistrarCompra utilizará el contrato ProcesadorPagos,
mientras un adaptador concreto implementará la comunicación
con el proveedor.

La selección y conexión de implementaciones se realizará en una
raíz de composición, fuera del dominio y de los casos de uso.

## Justificación

La separación permite probar las reglas y los casos de uso utilizando
adaptadores en memoria o de prueba.

También permite cambiar implementaciones de almacenamiento,
pagos o notificaciones manteniendo los contratos cuando las necesidades
del negocio sean las mismas.

Las responsabilidades quedan identificadas y los cambios pueden
localizarse en las capas correspondientes.

## Consecuencias

- Las entidades y los casos de uso deberán mantenerse independientes
  de Angular, controladores HTTP y tecnologías de almacenamiento.
- Los adaptadores deberán cumplir los contratos del dominio.
- Los mecanismos de inyección de dependencias permanecerán
  fuera del núcleo del negocio.
- Será necesario mantener contratos, adaptadores y una raíz
  de composición.
- La revisión de importaciones permitirá comprobar que las
  dependencias respetan los límites definidos.

## Alternativa considerada

Mantener únicamente la organización inicial en tres capas:
Presentación, Lógica de negocio y Datos.

Se decidió refinar esa organización separando las reglas del dominio
de la coordinación de los casos de uso y estableciendo una regla
explícita para las dependencias internas.

## Relación con el boilerplate

El ejemplo organiza sus responsabilidades en las carpetas dominio,
aplicacion, infraestructura y presentacion.

Los contratos están en dominio/contratos. Los adaptadores concretos
están en infraestructura y app.config.ts conecta las implementaciones
mediante la configuración de proveedores.

Se ejecutaron correctamente las 16 pruebas existentes mediante
npm run pruebas, sin iniciar la interfaz Angular. Este resultado
aporta evidencia de que las reglas y los casos de uso incluidos
pueden verificarse con adaptadores en memoria.