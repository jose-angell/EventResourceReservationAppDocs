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
namespace EventResourceReservationApp.Domain
{
    public class Review
    {
        public Guid Id { get; set; }
        public Guid ResourceId { get; set; }
        public Guid UserId { get; set; }
        public int Rating { get; set; } 
        public string Comment { get; set; }
        public DateTime CreatedAt { get; set; }

        public Review()
        {
            Comment = string.Empty;
            CreatedAt = DateTime.UtcNow;
            Rating = 1;
        }
        public Review(Guid resourceId, Guid userId, int rating, string comment)
        {
            if(resourceId == Guid.Empty)
            {
                throw new ArgumentException("ResourceId cannot be empty.", nameof(resourceId));
            }
            if(userId == Guid.Empty)
            {
                throw new ArgumentException("UserId cannot be empty.", nameof(userId));
            }
            if (rating < 1 || rating > 5)
            {
                throw new ArgumentException("Rating must be between 1 and 5.", nameof(rating));
            }
            if (string.IsNullOrWhiteSpace(comment))
            {
                throw new ArgumentException("Comment cannot be null or empty.", nameof(comment));
            }
            Id = Guid.NewGuid();
            ResourceId = resourceId;
            UserId = userId;
            Rating = rating;
            Comment = comment;
            CreatedAt = DateTime.UtcNow;
        }
        public void Update(int rating, string comment)
        {
            if (rating < 1 || rating > 5)
            {
                throw new ArgumentException("Rating must be between 1 and 5.", nameof(rating));
            }
            if (string.IsNullOrWhiteSpace(comment))
            {
                throw new ArgumentException("Comment cannot be null or empty.", nameof(comment));
            }
            Rating = rating;
            Comment = comment;
        }
    }
}
```

## 2. Migraciones de Base de Datos
Lista todas las migraciones de EF Core creadas o modificadas para esta feature.
    - `20250913_InitialReviewMigration`: Crea la tabla de Reseñas con sus campos iniciales.

## 3. Relaciones e Índices
Documenta las relaciones entre tablas (claves foráneas) y los índices creados para optimizar el rendimiento de las consultas y asegurar la integridad de los datos.

| Columna | Tipo Tipo / Cardinalidad <br/> (FK/INDEX)	 | 	Columna Destino (si es FK) | Propósito / Justificación |
|---------|------------------|------------|----------------------|
|`<ReservationCarItems>.<ClientId>` | FK  |User(Id) |Integridad referencial: Asegura que un item tenga un responsable. |
|`<ReservationCarItems>.<ResourceId>` | FK  |Resource(Id) |Integridad referencial: Asegura que un  item este relacionado a un recurso especifico. |

