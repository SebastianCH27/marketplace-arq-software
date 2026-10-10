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
flowchart TD

    subgraph ACTORES["ACTORES"]
        Cliente["Cliente"]
        Seller["Seller"]
        Admin["Administrador"]
    end

    subgraph PRESENTACION["PRESENTACIÓN"]
        Web["Aplicación Web → API REST"]
    end

    subgraph NEGOCIO["LÓGICA DE NEGOCIO"]
        Usuarios["Usuarios"]
        Sellers["Sellers"]
        Catalogo["Catálogo"]
        Carrito["Carrito"]
        Pedidos["Pedidos"]
    end

    subgraph DATOS["DATOS"]
        BD["Base de datos"]
    end

    subgraph EXTERNOS["SISTEMAS EXTERNOS"]
        Pago["Pasarela de pago"]
        ERP["ERP"]
        Envio["Servicio de envío"]
    end

    ACTORES --> PRESENTACION
    PRESENTACION --> NEGOCIO
    NEGOCIO --> DATOS
    DATOS -->|"integraciones"| EXTERNOS

    Cliente ~~~ Seller
    Seller ~~~ Admin

    Usuarios ~~~ Sellers
    Sellers ~~~ Catalogo
    Catalogo ~~~ Carrito
    Carrito ~~~ Pedidos

    Pago ~~~ ERP
    ERP ~~~ Envio

    style ACTORES fill:#222,stroke:#fff,stroke-width:2px,color:#fff
    style PRESENTACION fill:#222,stroke:#fff,stroke-width:2px,color:#fff
    style NEGOCIO fill:#222,stroke:#fff,stroke-width:2px,color:#fff
    style DATOS fill:#222,stroke:#fff,stroke-width:2px,color:#fff
    style EXTERNOS fill:#222,stroke:#fff,stroke-width:2px,color:#fff

    style Cliente fill:#222,stroke:#fff,color:#fff
    style Seller fill:#222,stroke:#fff,color:#fff
    style Admin fill:#222,stroke:#fff,color:#fff

    style Web fill:#222,stroke:#fff,color:#fff

    style Usuarios fill:#222,stroke:#fff,color:#fff
    style Sellers fill:#222,stroke:#fff,color:#fff
    style Catalogo fill:#222,stroke:#fff,color:#fff
    style Carrito fill:#222,stroke:#fff,color:#fff
    style Pedidos fill:#222,stroke:#fff,color:#fff

    style BD fill:#222,stroke:#fff,color:#fff

    style Pago fill:#222,stroke:#fff,color:#fff
    style ERP fill:#222,stroke:#fff,color:#fff
    style Envio fill:#222,stroke:#fff,color:#fff
```

### Observación sobre el diagrama de referencia

El diagrama reproduce el ejemplo de la guía, que dibuja la flecha de
integraciones desde la capa de Datos. Sin embargo, la descripción de
la guía asigna al módulo de Pedidos la integración con la pasarela
de pago y el servicio de envío.