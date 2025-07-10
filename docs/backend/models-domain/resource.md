---
sidebar_position: 3
title: Recurso
---

## Entidad de Dominio: `Resource`
> **Archivo:** models-domains/Resource.md

Este documento define la entidad de dominio `Resource`, sus propiedades, su propósito dentro del sistema y sus relaciones clave con otras entidades. Sirve como la fuente principal de verdad para la estructura de datos relacionada con los recursos disponibles para reserva.

---
### 1. Propósito de la Entidad
La entidad `Resource` representa un bien o servicio individual disponible para ser reservado o utilizado por los clientes en el sistema. Su propósito es centralizar toda la información relevante de cada recurso, como sus detalles descriptivos, disponibilidad actual y precio, permitiendo a los usuarios visualizar y gestionar lo que se puede reservar.

---
### 2. Propiedades y Atributos
A continuación, se detallan las propiedades de la entidad `Resource`, incluyendo su tipo de dato conceptual y una descripción clara de su propósito.

| Propiedades | Tipo de Dato (conceptual) | Descripción |
|-------------|---------------------------|-------------|
| `Id`  | `UUID` (o `int` si es identidad generada por DB) | Identificador único  para cada recurso. |
| `CategoryId` | `UUID` (o `int`) |  Clave foránea (`FK`) a la entidad `Category`, indicando a qué categoria pertenece este recurso. |
| `EstadoId` | `int` (o `Enum`) | Clave foránea (`FK`) a la entidad StatusResource (o valor de enum), relacionando el estado actual del recurso (ej. `Disponible`, `Bloqueado`, `FueraDeServicio`, `Eliminado`).|
| `Nombre` | `string` | Nombre corto o título identificador del recurso. |
| `Descripcion` |  `string` |  Descripción detallada del recurso, sus características o funcionalidades.|
| `CantidadDisponible` |  `int` |  Número de unidades de este recurso que están actualmente disponibles para ser reservadas. |
| `PrecioUnitario` |  `Decimal`(`numeric`) | Precio por una unidad de este recurso al momento de la consulta.|
| `TipoAutorizacion` |  `Enum` (`int` o `string`) |  Define si la reserva de este recurso se autoriza automáticamente o requiere revisión y aprobación manual por un administrador. |
| `DireccionId` |  `UUID` (o `int`) | Clave foránea (`FK`) a la entidad `Location` (`Direccion`) donde se almacena el recurso.|
| `UsuarioCreacionId` | `UUID` (o `int`) | Clave foránea (`FK`) a la entidad `User` (`Usuario`) quien realiza el registro de la direccion.|
| `FechaCreacion` | `DateTime` | Marca de tiempo que registra cuándo se creó la reserva. |

**Nota sobre Imágenes:** Las imágenes asociadas a los recursos se almacenan de forma distinta, típicamente en un servicio de almacenamiento de archivos (como Azure Blob Storage, AWS S3) y se referencian mediante URLs. Esta entidad Resource contendría los metadatos de las imágenes o un enlace a la URL principal.



 ``` mermaid
erDiagram
Category {
        UUID Id PK
        string Nombre
        string Descripcion
    }
    
    StatusResource {
        int Id PK
        string Nombre
        string Descripcion
    }
    
    Location {
        UUID Id PK
        string Pais
        string Ciudad
    }
    
    User {
        UUID Id PK
        string Nombre
        string Email
    }

    Resource {
        UUID Id PK
        UUID CategoryId FK "Category.Id"
        int EstadoId FK "StatusResource.Id"
        string Nombre
        string Descripcion
        int CantidadDisponible
        decimal PrecioUnitario
        int TipoAutorizacion
        UUID DireccionId FK "Location.Id"
        UUID UsuarioCreacionId FK "User.Id"
        datetime FechaCreacion
    }

    ReservationDetail {
        UUID Id PK
        UUID ReservationId FK "Reservation.Id"
        UUID ResourceId FK "Resource.Id"
        int Cantidad
        decimal PrecioUnitario
    }
    
    Reservation {
        UUID Id PK
        datetime Fecha
        datetime HoraInicio
        datetime HoraFin
    }

    Category ||--o{ Resource : "clasifica_como"
    StatusResource ||--o{ Resource : "tiene_estado"
    Location ||--o{ Resource : "se_almacena_en"
    User ||--o{ Resource : "registra"
    Resource ||--o{ ReservationDetail : "es_incluido_en_detalle_de_reserva"
```