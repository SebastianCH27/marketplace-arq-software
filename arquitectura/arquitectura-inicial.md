# Arquitectura inicial del Marketplace

## Arquitectura en tres capas

La propuesta inicial organiza el sistema en tres capas, cada una con
responsabilidades definidas.

| Capa | Pregunta que responde | Elementos | Responsabilidad |
|------|-----------------------|-----------|-----------------|
| Presentación | ¿Cómo interactúa el usuario? | Aplicación Web y API REST | Presentar información, recibir las solicitudes de los usuarios y comunicar sus resultados. |
| Lógica de negocio | ¿Qué hace el sistema? | Usuarios, Sellers, Catálogo, Carrito y Pedidos | Procesar las operaciones y aplicar las reglas del negocio. |
| Datos | ¿Dónde se almacena la información? | Base de datos | Almacenar y recuperar la información utilizada por el sistema. |

## Módulos de la capa de negocio

| Módulo | Responsabilidad |
|--------|-----------------|
| Usuarios | Gestionar la información de los usuarios y su acceso al sistema. |
| Sellers | Gestionar los vendedores registrados en la plataforma. |
| Catálogo | Gestionar los productos y permitir consultar su información y disponibilidad. |
| Carrito | Gestionar los productos seleccionados para la compra y sus cantidades. |
| Pedidos | Generar pedidos y permitir consultar su detalle y estado. |

## Relación entre las capas

La capa de presentación recibe las solicitudes y las comunica a la capa
de lógica de negocio.

La capa de lógica de negocio procesa las solicitudes y utiliza la capa
de datos cuando necesita almacenar o recuperar información.

Los resultados regresan a la capa de presentación para mostrarse al usuario.


## Diagrama de arquitectura

```mermaid
flowchart TB
    subgraph ACTORES["Actores"]
        Cliente["Cliente"]
        Seller["Seller"]
        Admin["Administrador"]
    end

    subgraph PRESENTACION["Capa de presentación"]
        Web["Aplicación web"]
        API["API REST"]
        Web -->|"Solicitudes HTTPS"| API
    end

    subgraph NEGOCIO["Capa de lógica de negocio"]
        Usuarios["Usuarios"]
        Sellers["Sellers"]
        Catalogo["Catálogo"]
        Carrito["Carrito"]
        Pedidos["Pedidos"]
    end

    subgraph DATOS["Capa de datos"]
        Acceso["Componentes de acceso a datos"]
        BD[("Base de datos")]
        Acceso --> BD
    end

    subgraph INTEGRACION["Componentes de integración"]
        AdaptPago["Adaptador de pagos"]
        AdaptEnvio["Adaptador de envíos"]
        AdaptFactura["Adaptador de facturación"]
        AdaptERP["Adaptador de ERP"]
    end

    subgraph EXTERNOS["Sistemas externos"]
        Pago["Pasarela de pago"]
        Envio["Servicio de envío"]
        Factura["Servicio de facturación"]
        ERP["ERP"]
    end

    Cliente --> Web
    Seller --> Web
    Admin --> Web

    API --> Usuarios
    API --> Sellers
    API --> Catalogo
    API --> Carrito
    API --> Pedidos

    Usuarios --> Acceso
    Sellers --> Acceso
    Catalogo --> Acceso
    Carrito --> Acceso
    Pedidos --> Acceso

    Pedidos --> AdaptPago
    Pedidos --> AdaptEnvio
    Pedidos --> AdaptFactura
    Catalogo --> AdaptERP

    AdaptPago --> Pago
    AdaptEnvio --> Envio
    AdaptFactura --> Factura
    AdaptERP --> ERP
```

### Observación sobre el diagrama de referencia

El diagrama reproduce el ejemplo de la guía, que dibuja la flecha de
integraciones desde la capa de Datos. Sin embargo, la descripción de
la guía asigna al módulo de Pedidos la integración con la pasarela
de pago y el servicio de envío.