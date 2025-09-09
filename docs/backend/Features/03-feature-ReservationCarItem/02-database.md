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
Definicion de las entidades de dominio y su mapeo a la base de datos (tablas, agregados). Se decidio utiliar un Id de tipo entero para los catologos, se agregaron campos de control para autizar y dar un correcto seguimiento a cada ubicacion.


### Opción B: Definición con Clases de C# (EF Core Entities)
```c#
namespace ResourceReservationApp.Domain.Entities
{
     public class ReservationCarItem
    {
        public Guid Id { get; set; }
        public Guid ClientId { get; set; }
        public Guid ResourceId { get; set; }
        public int Quantity { get; set; }
        public DateTime AddedAt { get; set; }


        public ReservationCarItem()
        {
            Id = Guid.NewGuid();
            Quantity = 1;
            AddedAt = DateTime.UtcNow;
        }
        public ReservationCarItem(Guid clientId, Guid resourceId, int quantity)
        {
            if (clientId == Guid.Empty)
            {
                throw new ArgumentException("ClientId cannot be empty.", nameof(clientId));
            }
            if (resourceId == Guid.Empty)
            {
                throw new ArgumentException("ClientId cannot be empty.", nameof(resourceId));
            }
            if (quantity <= 0)
            {
                throw new ArgumentException("Quantity must be greater than zero.", nameof(quantity));
            }

            Id = Guid.NewGuid();
            ClientId = clientId;
            ResourceId = resourceId;
            Quantity = quantity;
            AddedAt = DateTime.UtcNow;
        }
        public void UpdateQuantity(int quantity)
        {
            if (quantity <= 0)
            {
                throw new ArgumentException("Quantity must be greater than zero.", nameof(quantity));
            }
            Quantity = quantity;
        }
    }
}
```

## 2. Migraciones de Base de Datos
Lista todas las migraciones de EF Core creadas o modificadas para esta feature.
    - `20250902032647_20250901_InitialReservationCarItemMigration`: Crea la tabla de carrito de reservas con sus campos iniciales.

## 3. Relaciones e Índices
Documenta las relaciones entre tablas (claves foráneas) y los índices creados para optimizar el rendimiento de las consultas y asegurar la integridad de los datos.

| Columna | Tipo Tipo / Cardinalidad <br/> (FK/INDEX)	 | 	Columna Destino (si es FK) | Propósito / Justificación |
|---------|------------------|------------|----------------------|
|`<ReservationCarItems>.<ClientId>` | FK  |User(Id) |Integridad referencial: Asegura que un item tenga un responsable. |
|`<ReservationCarItems>.<ResourceId>` | FK  |Resource(Id) |Integridad referencial: Asegura que un  item este relacionado a un recurso especifico. |

