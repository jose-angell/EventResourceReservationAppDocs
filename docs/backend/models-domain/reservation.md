---
sidebar_position: 1
title: Reservación
---

## Entidad de Dominio: `Reservation`
> **Archivo:** models-domains/Reservation.md

Este documento define la entidad de dominio `Reservation`, sus propiedades, su propósito dentro del sistema y sus relaciones clave con otras entidades. Sirve como la fuente principal de verdad para la estructura de datos relacionada con las reservas.

---
### 1. Propósito de la Entidad
La entidad `Reservation` representa la solicitud y confirmación de uso de uno o más recursos por parte de un cliente durante un período de tiempo específico. Es central para la funcionalidad de planificación y gestión de eventos de la aplicación, controlando la disponibilidad de recursos y el historial de uso.

---
### 2. Propiedades y Atributos
A continuación, se detallan las propiedades de la entidad `Reservation`, incluyendo su tipo de dato y una descripción clara de su propósito.

| Propiedades | Tipo de Dato (conceptual) | Descripción |
|-------------|---------------------------|-------------|
| `Id`  | `UUID` (o `int` si es identidad generada por DB) | Identificador único de la reserva |
| `Fecha` | `Date` | Fecha de la reserva del recurso |
| `HoraInicio` | `Time` | Hora específica en que el recurso estará disponible para el cliente.|
| `HoraFin` | `Time` | Hora específica en que el cliente debe entregar el recurso. |
| `Estado` | `Enum` (`int` o `string`) | Estado de la reserva (`pendiente`, `confirmada`, `rechazada`). |
| `Total` | `Decimal`(`numeric`) | Monto total a pagar por los recursos reservados. |
| `ComentarioCliente` | `string` | Observaciones o indicaciones adicionales proporcionadas por el cliente. |
| `Telefono` | `string` |Número de teléfono del cliente para contacto|
| `DireccionId` | `UUID` (o `int`) | Clave foránea (`FK`) a la entidad `Location` (`Direccion`) donde se realizará la reserva.|
| `ClienteId` | `UUID` (o `int`)   | Clave foránea (`FK`) a la entidad User (`Cliente`) que realizó la reserva. |
| `FechaCreacion` | `DateTime` | Marca de tiempo que registra cuándo se creó la reserva. |
| `AdministradorId` | `UUID` (o `int`, opcional) | Clave foránea (`FK`) a la entidad `User` (`Administrador`) que `confirmó`/`rechazó` la reserva (`puede ser nulo`). |
| `ComentarioAdministrador` | `string` (opcional) | Motivo o nota del administrador al gestionar la reserva, especialmente si es rechazada. |
| `TransaccionId` | `UUID` (o `int`, opcional) | Clave foránea (`FK`) a la entidad Transaction (`Pago`) que relaciona el pago de la reserva (puede ser nulo).  |

---
### 3. Diagrama de Entidad-Relación (ERD)
Este diagrama visualiza la estructura de la entidad `Reservation` y sus relaciones clave con otras entidades en el modelo de dominio.

```mermaid
erDiagram
    User {
        UUID Id PK
        string Nombre
        string Email
    }

    Location {
        UUID Id PK
        string DireccionCompleta
        string Ciudad
    }

    Transaction {
        UUID Id PK
        Decimal Monto
        UUID EstadoTransaccion
    }

    ReservationStatus {
        int Id PK
        string NombreEstado
    }

    Reservation {
        UUID Id PK
        datetime Fecha
        datetime HoraInicio
        datetime HoraFin
        int Estado FK "ReservationStatus.Id"
        decimal Total
        string ComentarioCliente
        string Telefono
        UUID DireccionId FK "Location.Id"
        UUID ClienteId FK "User.Id"
        datetime FechaCreacion
        UUID AdministradorId FK "User.Id"
        string ComentarioAdministrador
        UUID TransaccionId FK "Transaction.Id"
    }

    ReservationDetail {
        UUID Id PK
        UUID ReservationId FK "Reservation.Id"
        UUID ResourceId FK "Resource.Id"
        int Cantidad
        decimal PrecioUnitario
    }

    Resource {
        UUID Id PK
        string Nombre
        string Tipo
        int Capacidad
    }

    User ||--o{ Reservation : "realiza"
    User ||--o{ Reservation : "gestiona (admin)"
    Location ||--o{ Reservation : "tiene_lugar_en"
    Transaction ||--o{ Reservation : "pago_reserva"
    ReservationStatus ||--o{ Reservation : "tiene_estado"
    Reservation ||--o{ ReservationDetail : "contiene_detalles"
    Resource ||--o{ ReservationDetail : "detalla_recursos"
```