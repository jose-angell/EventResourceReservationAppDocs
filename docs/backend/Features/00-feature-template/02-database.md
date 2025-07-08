---
sidebar_position: 2
title: 02-database
---

# 📦 Modelo de datos

> **Archivo:** `02-database.md`

Este documento describe las **entidades, tablas, migraciones y relaciones** de base de datos asociadas a la implementación de esta feature, asegurando la consistencia y la trazabilidad de los cambios en el esquema.

---

## 1. Entidades nuevas o modificadas
<!-- Define las tablas o agregados con su esquema SQL o clases EF -->
Aquí se definen las entidades de dominio y su mapeo a la base de datos (tablas, agregados). Puedes presentarlas con su **esquema SQL** o mediante las **clases de C# (modelos de EF Core)** que las representan. Se recomienda incluir una breve descripción de cada entidad/tabla y sus campos más relevantes.

---

### Opción A: Definición con Esquema SQL (CREATE TABLE)

```sql
-- Ejemplo: Entidad 'Reservation'
CREATE TABLE Reservations (
  Id UUID PRIMARY KEY NOT NULL,
  ResourceId UUID NOT NULL,            -- Clave foránea a la tabla Resources
  UserId UUID NOT NULL,                -- Clave foránea a la tabla Users
  StartTime TIMESTAMP WITH TIME ZONE NOT NULL,
  EndTime TIMESTAMP WITH TIME ZONE NOT NULL,
  Status VARCHAR(50) NOT NULL,         -- Ej: 'Pending', 'Confirmed', 'Cancelled'
  CreatedAt TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT NOW(),
  UpdatedAt TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT NOW(),
  -- Agrega aquí otras columnas relevantes
);

COMMENT ON TABLE Reservations IS 'Almacena la información de las reservas de recursos.';
COMMENT ON COLUMN Reservations.Status IS 'Estado actual de la reserva (Pendiente, Confirmada, Cancelada).';
```

### Opción B: Definición con Clases de C# (EF Core Entities)
```c#
// Ejemplo: Entidad 'Reservation' (Domain Layer)
namespace ResourceReservationApp.Domain.Entities
{
    public class Reservation
    {
        public Guid Id { get; private set; }
        public Guid ResourceId { get; private set; } // Referencia al recurso reservado
        public Guid UserId { get; private set; }     // Referencia al usuario que realiza la reserva
        public DateTimeOffset StartTime { get; private set; }
        public DateTimeOffset EndTime { get; private set; }
        public ReservationStatus Status { get; private set; } // Enum o string
        public DateTimeOffset CreatedAt { get; private set; }
        public DateTimeOffset UpdatedAt { get; private set; }

        // Constructor y métodos para la lógica de negocio
    }

    // Ejemplo de Enum (si aplica)
    public enum ReservationStatus
    {
        Pending,
        Confirmed,
        Cancelled
    }
}

// Consideraciones de Mapeo EF Core (Infrastructure Layer - DbContext)
// Puedes añadir aquí ejemplos de configuraciones Fluent API si son complejas.
// Por ejemplo: entity.Property(e => e.Status).HasConversion<string>();
```

## 2. Migraciones de Base de Datos
Lista todas las migraciones de EF Core creadas o modificadas para esta feature. Esto proporciona un historial claro de cómo evolucionó el esquema de la base de datos.
    - `YYYYMMDD_NombreDescriptivoDeLaMigracion`: Objetivo de la migración.
      - `20250708_AddReservationAndResourceTables`: Crea las tablas `Reservations` y `Resources` con sus campos iniciales.
      - `20250710_AddReservationStatusEnum`: Agrega la columna `Status` a la tabla `Reservations` y la mapea a un `enum`.
      - `20250712_AddUniqueIndexOnResourceTime`: Añade un índice único para evitar reservas duplicadas del mismo recurso a la misma hora.

## 3. Relaciones e Índices
Documenta las relaciones entre tablas (claves foráneas) y los índices creados para optimizar el rendimiento de las consultas y asegurar la integridad de los datos.

| Columna | Tipo Tipo / Cardinalidad <br/> (FK/INDEX)	 | 	Columna Destino (si es FK) | Propósito / Justificación |
|---------|------------------|------------|----------------------|
|`<Tabla>.<Campo>` | FK  |OtraTabla(Id) |Integridad referencial |
|`<Tabla>.<Campo2>` | INDEX | N/A|Optimizar consultas X |
|`Reservations.ResourceId`| FK (1:N) | `Resources.Id` | Integridad referencial: Asegura que una reserva siempre apunte a un recurso existente.|
| `Reservations.UserId` | FK (1:N) | `Users.Id` | Integridad referencial: Vincula la reserva a un usuario específico.|
| `Reservations.StartTime` | INDEX | N/A | Optimizar consultas por rango de fechas de reserva.|
|`Reservations.EndTime` | INDEX | N/A | Optimizar consultas por rango de fechas de reserva. |
|`Resources.Name` | INDEX (Unique) | N/A | Asegura que los nombres de los recursos sean únicos para facilitar búsquedas. |
|`Reservations.ResourceId, StartTime, EndTime` |INDEX (Unique Composite) | N/A |Previene la doble reserva del mismo recurso para el mismo intervalo de tiempo.|
