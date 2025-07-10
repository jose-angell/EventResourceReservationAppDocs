---
sidebar_position: 2
title: Detalle de Reserva
---

## Entidad de Dominio: `ReservationDetail`
> **Archivo:** models-domains/ReservationDetail.md

Este documento define la entidad de dominio `ReservationDetail`, sus propiedades, su propósito dentro del sistema y sus relaciones clave con otras entidades. Sirve como la fuente principal de verdad para la estructura de datos relacionada con los detalles específicos de cada reserva.

---
### 1. Propósito de la Entidad
La entidad `ReservationDetail` representa la información específica de cada recurso individual asociado a una `Reservation`. Su propósito es registrar la cantidad de unidades de un recurso ocupadas y el precio por unidad al momento de realizar la reserva. Actúa como una tabla de unión con atributos (un "join table with attributes") entre `Reservation` y `Resource`.

---

### 2. Propiedades y Atributos
A continuación, se detallan las propiedades de la entidad `ReservationDetail`, incluyendo su tipo de dato y una descripción clara de su propósito.

| Propiedades | Tipo de Dato (conceptual) | Descripción |
|-------------|---------------------------|-------------|
| `Id`  | `UUID` (o `int` si es identidad generada por DB) | Identificador único del detalle de la reserva |
| `ReservationId` | `UUID` (o `int`) |  Clave foránea (`FK`) a la entidad `Reservation`, indicando a qué reserva pertenece este detalle. |
| `ResourceId` | `UUID` (o `int`) | Clave foránea (`FK`) a la entidad `Resource`, identificando el recurso específico que se reservó.|
| `Cantidad` | `int` | Número de unidades de este recurso específico incluidas en la reserva. |
| `PrecioUnitario` |  `Decimal`(`numeric`) |  Precio por unidad del recurso en el momento exacto en que se realizó la reserva. |

---

### 3. Diagrama de Entidad-Relación (ERD)
Este diagrama visualiza la estructura de la entidad `ReservationDetail` y sus relaciones clave con las entidades `Reservation` y `Resource` en el modelo de dominio.

``` mermaid
erDiagram
    Reservation {
        UUID Id PK
        datetime Fecha
        datetime HoraInicio
        datetime HoraFin
    }
    
    Resource {
        UUID Id PK
        string Nombre
        string Tipo
        int Capacidad
    }

    ReservationDetail {
        UUID Id PK
        UUID ReservationId FK "Reservation.Id"
        UUID ResourceId FK "Resource.Id"
        int Cantidad
        decimal PrecioUnitario
    }
    
    Reservation ||--o{ ReservationDetail : "tiene_detalle_de_recurso"
    Resource ||--o{ ReservationDetail : "es_parte_del_detalle_como"
```