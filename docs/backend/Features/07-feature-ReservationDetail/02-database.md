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
Definicion de las entidades de dominio y su mapeo a la base de datos (tablas, agregados). Se decidio utiliar un Id de tipo GUID para las reservaciones.


### Opción B: Definición con Clases de C# (EF Core Entities)
```c#
namespace ResourceReservationApp.Domain.Entities
{
      public class ReservationDetail
    {
        public Guid Id { get; set; }
        public Guid ReservationId { get; set; }
        public Reservation? Reservation { get; set; }
        public Guid ResourceId { get; set; }
        public Resource? Resource { get; set; } 
        public int Quantity { get; set; }   
        public decimal UnitPrice { get; set; }
        public DateTime Created { get; set; }
        public ReservationDetail()
        {
        }
        public ReservationDetail(Guid reservationId, Guid resourceId, int quantity, decimal unitPrice)
        {
            if (reservationId == Guid.Empty)
            {
                throw new ArgumentException("ReservationId cannot be empty.", nameof(reservationId));
            }
            if (resourceId == Guid.Empty)
            {
                throw new ArgumentException("ResourceId cannot be empty.", nameof(resourceId));
            }
            if (quantity <= 0)
            {
                throw new ArgumentException("Quantity must be a positive value.", nameof(quantity));
            }
            if (unitPrice < 0)
            {
                throw new ArgumentException("UnitPrice cannot be negative.", nameof(unitPrice));
            }
            ReservationId = reservationId;
            ResourceId = resourceId;
            Quantity = quantity;
            UnitPrice = unitPrice;
        }
        public void UpdateQuantity(int quantity)
        {
            if (quantity <= 0)
            {
                throw new ArgumentException("Quantity must be a positive value.", nameof(quantity));
            }
            Quantity = quantity;
        }

    }
}
```

## 2. Migraciones de Base de Datos
Lista todas las migraciones de EF Core creadas o modificadas para esta feature.
    <!-- - `20250821043923_20250820_InitialLocationMitragion`: Crea la tabla de Ubicacines con sus campos iniciales. -->
    

## 3. Relaciones e Índices
Documenta las relaciones entre tablas (claves foráneas) y los índices creados para optimizar el rendimiento de las consultas y asegurar la integridad de los datos.

| Columna | Tipo Tipo / Cardinalidad <br/> (FK/INDEX)	 | 	Columna Destino (si es FK) | Propósito / Justificación |
|---------|------------------|------------|----------------------|
|`<ReservationDetail>.<CreatedByUserId>` | FK  |User(Id) |Integridad referencial: Asegura que cada detalle de reserva tenga un responsable. |
|`<ReservationDetail>.<ReservationId>` | FK  |Category(Id) |Integridad referencial: Asegura que un detalle de reservacion tenga una reserva relacionada |
|`<ReservationDetail>.<ResourceId>` | FK  |Location(Id) |Integridad referencial: Asegura que un detalle de reservacion tenga un recurso relacionado. |

