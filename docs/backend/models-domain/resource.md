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
| `StatusId` | `int` (o `Enum`) | Clave foránea (`FK`) a la entidad `StatusResource` (o valor de enum), relacionando el estado actual del recurso (ej. `Disponible`, `Bloqueado`, `FueraDeServicio`, `Eliminado`).|
| `Name` | `string` | Nombre corto o título identificador del recurso. |
| `Description` |  `string` |  Descripción detallada del recurso, sus características o funcionalidades.|
| `AvailableQuantity` |  `int` |  Número de unidades de este recurso que están actualmente disponibles para ser reservadas. |
| `UnitPrice` |  `Decimal`(`numeric`) | Precio por una unidad de este recurso al momento de la consulta.|
| `AuthorizationType` |  `Enum` (`int`) |   Define si la reserva de este recurso se autoriza automáticamente o requiere revisión y aprobación manual por un administrador. Los valores posibles se definen en el `enum TypeAuthorization` en el código (ej. `automatico=1`, `manual=2`).|
| `LocationId` |  `UUID` (o `int`) | Clave foránea (`FK`) a la entidad `Location` (`Direccion`) donde se almacena el recurso.|
| `CreatedByUserId` | `UUID` (o `int`) | Clave foránea (`FK`) a la entidad `User` (`Usuario`) quien realiza el registro de la direccion.|
| `CreatedAt` | `DateTime` | Marca de tiempo que registra cuándo se creó la reserva. |

**Nota sobre Imágenes:** Las imágenes asociadas a los recursos se almacenan de forma distinta, típicamente en un servicio de almacenamiento de archivos (como Azure Blob Storage, AWS S3) y se referencian mediante URLs. Esta entidad Resource contendría los metadatos de las imágenes o un enlace a la URL principal.



 ``` mermaid
erDiagram
Category {
        UUID Id PK
        string Name
        string Description
    }
    
    StatusResource {
        int Id PK
        string StatusName
        string Description
    }
    
    Location {
        UUID Id PK
        string Country
        string City
    }
    
    User {
        UUID Id PK
        string FirstName
        string Email
    }

    Resource {
        UUID Id PK
        UUID CategoryId FK "Category.Id"
        int StatusId FK "ResourceStatus.Id"
        string Name
        string Description
        int AvailableQuantity
        decimal UnitPrice
        int AuthorizationType
        UUID LocationId FK "Location.Id"
        UUID CreatedByUserId FK "User.Id"
        datetime CreatedAt
    }

    ReservationDetail {
        UUID Id PK
        UUID ReservationId FK "Reservation.Id"
        UUID ResourceId FK "Resource.Id"
        int Quantity
        decimal UnitPrice
    }
    
    Reservation {
        UUID Id PK
        datetime Date
        datetime StartTime
        datetime EndTime
    }

    Category ||--o{ Resource : "clasifica_como"
    StatusResource ||--o{ Resource : "tiene_estado"
    Location ||--o{ Resource : "se_almacena_en"
    User ||--o{ Resource : "registra"
    Resource ||--o{ ReservationDetail : "es_incluido_en_detalle_de_reserva"
```