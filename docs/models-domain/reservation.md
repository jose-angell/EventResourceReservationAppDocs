---
sidebar_position: 1
title: Reservación
---

## Entidad Reservation

 Propiedades: 
 - `Id` : `int` | Identificador unico de la reserva 
 - `Fecha` : `DateTime` | Fecha de la reserva del recurso
 - `HoraInicio` : `DateTime` | Hora de inicio en la que el cliente espera tener el recurso a su disposicion
 - `HoraFin` : `DateTime` | Hora fin en la que el cliente espera entregar el recurso al proveedor
 - `Estado` : `int` | Estado de la reserva (pendiente, confirmada, Rechazada).
 - `Total` : `numeric` | Total a pagar por todos los recursos reservados.
 - `ComentarioCliente` : `int` | Espcion para que el cliente pueda poner observaciones o indicaciones extras en la reserva
 - `Telefono` : `string` | El cliente agrega su telefono para mantener el contacto con el proveedor
 - `DireccionId` : `int` | Llave foranea para relacionar la direccion almacenada en la tabla de direcciones.
 - `ClienteId` : `int` | Llave foranea para identificar al usuario al que le pertenece la reserva.
 - `FechaCreacion` : `DateTime` | Fecha de crecion de la reserva.
 - `AdministradorId` : `int` | Llave foranea para indicar cual administrador se encargo de confirmar o rechazar una reserva
 - `ComentarioAdministrador` : `string` | Espacio para que el administrador le pueda dar algun motivo por el cual se rechazo su reserva

 ``` mermaid
erDiagram
    Reservation {
        int Id
        datetime Fecha
        datetime HoraInicio
        datetime HoraFin
        int Estado
        numric Total
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