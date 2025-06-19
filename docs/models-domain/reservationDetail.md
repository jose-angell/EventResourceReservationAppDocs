---
sidebar_position: 2
title: Detalle de Reserva
---

## Entidad ReservationDetail

Propiedades: 
- `Id` : `int` | Identificador único del detalle de la reserva
- `ReservationId` : `int` | Llave foránea para relacionar el detalle de la reserva con la reserva.
- `ResourceId` : `int` | Llave foránea para relacionar el recurso con la reserva.
- `Cantidad` : `int` | Número de unidades del recurso reservado.
- `Precio` : `numeric` | Precio por unidad al momento de reservar.

``` mermaid
erDiagram
    ReservationDetail {
        int Id
        int ReservationId
        int ResourceId
        int Cantidad
        numeric Precio
    }

    Reservation ||--o{ ReservationDetail : "tiene detalle"
    Resource ||--o{ ReservationDetail : "incluye recurso"
```