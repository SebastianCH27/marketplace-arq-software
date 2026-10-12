# ADR-001: Adoptar un monolito modular para el backend

- **Estado:** Aceptado para el diseño inicial.
- **Fecha:** 2026-10-11.

## Contexto

El Marketplace permite consultar productos para mascotas, gestionar
el carrito y realizar compras. También contempla funcionalidades para
sellers y administradores, además de integraciones con sistemas externos.

Durante campañas comerciales puede aumentar la cantidad de usuarios
y solicitudes. El sistema también debe permitir modificar funcionalidades
sin afectar innecesariamente otros módulos.

Esta decisión responde a los siguientes drivers:

| Driver | Necesidad |
|--------|-----------|
| DA01 — Escalabilidad | Atender el incremento de usuarios durante campañas comerciales. |
| DA06 — Mantenibilidad | Facilitar cambios mediante módulos con responsabilidades definidas. |

## Decisión

Organizar el backend como un monolito modular: una aplicación desplegable
que contiene módulos con responsabilidades y límites definidos.

Los principales módulos serán:

| Módulo | Responsabilidad |
|--------|-----------------|
| Usuarios | Gestionar cuentas y roles de clientes, sellers y administradores. |
| Catálogo | Gestionar y consultar productos, precios y disponibilidad. |
| Carrito | Gestionar productos seleccionados, cantidades y totales. |
| Pedidos | Registrar compras y gestionar sus estados y consultas. |
| Pagos | Coordinar el procesamiento de pagos mediante la pasarela externa. |

Los módulos se comunicarán mediante interfaces definidas. Las integraciones
con servicios externos se organizarán mediante adaptadores.

La interfaz cliente se comunicará con el backend mediante una API REST,
conforme al driver DA05.

## Justificación

Esta organización permite establecer límites entre las funcionalidades
manteniendo una operación y un despliegue inicial sencillos.

La separación modular facilita localizar las responsabilidades,
realizar cambios y probar partes del sistema.

Para atender incrementos de demanda se podrán desplegar varias instancias
del backend, acompañadas de una gestión adecuada del estado compartido
y de pruebas que permitan comprobar su capacidad.

## Consecuencias

- Los módulos deberán respetar sus límites y evitar acceder directamente
  a los detalles internos de otros módulos.
- El backend se desplegará como una unidad.
- El escalamiento horizontal replicará el backend completo.
- La capacidad y los tiempos de respuesta deberán verificarse mediante
  pruebas de carga.
- La modularidad facilitará evaluar una separación futura de servicios
  si las necesidades del sistema la justifican.

## Alternativa considerada

Los microservicios permitirían desplegar y escalar servicios
independientemente, pero requieren gestionar comunicación distribuida,
fallos entre servicios y varios despliegues.

Se adopta el monolito modular como organización inicial del backend.

## Relación con el boilerplate

El boilerplate Angular se utiliza para estudiar la separación de capas
y las dependencias internas. Este ADR define la organización global
propuesta para el backend del Marketplace.