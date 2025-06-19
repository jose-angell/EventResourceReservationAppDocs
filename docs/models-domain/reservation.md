---
sidebar_position: 1
title: Reservación
---

## Entidad Reservation

Propiedades: 
- `Id` : `int` | Identificador único de la reserva
- `Fecha` : `DateTime` | Fecha de la reserva del recurso
- `HoraInicio` : `DateTime` | Hora de inicio en la que el cliente espera tener el recurso a su disposición
- `HoraFin` : `DateTime` | Hora fin en la que el cliente espera entregar el recurso al proveedor
- `Estado` : `int` | Estado de la reserva (pendiente, confirmada, rechazada).
- `Total` : `numeric` | Total a pagar por todos los recursos reservados.
- `ComentarioCliente` : `string` | Espacio para que el cliente pueda poner observaciones o indicaciones extras en la reserva
- `Telefono` : `string` | El cliente agrega su teléfono para mantener el contacto con el proveedor
- `DireccionId` : `int` | Llave foránea para relacionar la dirección almacenada en la tabla de direcciones.
- `ClienteId` : `int` | Llave foránea para identificar al usuario al que le pertenece la reserva.
- `FechaCreacion` : `DateTime` | Fecha de creación de la reserva.
- `AdministradorId` : `int` | Llave foránea para indicar cuál administrador se encargó de confirmar o rechazar una reserva
- `ComentarioAdministrador` : `string` | Espacio para que el administrador le pueda dar algún motivo por el cual se rechazó su reserva

``` mermaid
erDiagram
    Reservation {
        int Id
        datetime Fecha
        datetime HoraInicio
        datetime HoraFin
        int Estado
        numeric Total
        string ComentarioCliente
        string Telefono
        int DireccionId
        int ClienteId
        datetime FechaCreacion
        int AdministradorId
        string ComentarioAdministrador
    }

    User ||--o{ Reservation : "realiza"
    User ||--o{ Reservation : "gestiona como admin"
    Location ||--o{ Reservation : "se entrega en"
    Estado ||--o{ Reservation : "tiene estado"
    Reservation ||--o{ ReservationDetail : "se detalla en"
```