---
sidebar_position: 1
title: Reservación
---

## Entidad Reservation

 Propiedades: `Id`, `Fecha`, `HoraInicio`, `HoraFin`, `ResourceId`, `Estado` (pendiente, confirmada, cancelada), `ClienteId`.

 ``` mermaid
erDiagram
    Reservation {
        int Id
        date Fecha
        time HoraInicio
        time HoraFin
        int ResourceId
        string Estado
        int ClienteId
    }
    
    Cliente ||--o{ Reservation : "realiza"
    Resource ||--o{ Reservation : "es reservado por"

```