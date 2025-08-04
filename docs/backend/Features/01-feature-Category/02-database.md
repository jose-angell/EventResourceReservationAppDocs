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
Definicion de las entidades de dominio y su mapeo a la base de datos (tablas, agregados). Se decidio utiliar un Id de tipo entero para los catologos, se agregaron campos de control para autizar y dar un correcto seguimiento a cada categoria.


### Opción B: Definición con Clases de C# (EF Core Entities)
```c#
namespace ResourceReservationApp.Domain.Entities
{
    public class Category
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Description { get; set; }
        public DateTime CreatedAt { get; set; }
        public Guid CreatedByUserId { get; set; }

        public Category()
        {
            Name = string.Empty;
            Description = string.Empty;
        }
        public Category(string name, string description, Guid createdByUserId)
        {
            if (string.IsNullOrEmpty(name))
            {
                throw new ArgumentException("Name cannot be null or empty.", nameof(name));
            }
            if (name.Length > 100)
            {
                throw new ArgumentException("Name cannot exceed 100 characters.", nameof(name));
            }
            if (createdByUserId == Guid.Empty)
            {
                throw new ArgumentException("CreatedByUserId cannot be empty.", nameof(createdByUserId));
            }
            Name = name;
            Description = description ?? string.Empty;
            CreatedAt = DateTime.UtcNow;
            CreatedByUserId = createdByUserId;
        }
        public void Update(string name, string description)
        {
            if (string.IsNullOrEmpty(name))
            {
                throw new ArgumentException("Name cannot be null or empty.", nameof(name));
            }
            if (name.Length > 100)
            {
                throw new ArgumentException("Name cannot exceed 100 characters.", nameof(name));
            }
            Name = name;
            Description = description ?? string.Empty;
        }
    }
}
```

## 2. Migraciones de Base de Datos
Lista todas las migraciones de EF Core creadas o modificadas para esta feature.
    - `20250728001946_InitialCategoryMigration`: Crea la tabla de Categories con sus campos iniciales.

## 3. Relaciones e Índices
Documenta las relaciones entre tablas (claves foráneas) y los índices creados para optimizar el rendimiento de las consultas y asegurar la integridad de los datos.

| Columna | Tipo Tipo / Cardinalidad <br/> (FK/INDEX)	 | 	Columna Destino (si es FK) | Propósito / Justificación |
|---------|------------------|------------|----------------------|
|`<Categories>.<CreatedByUserId>` | FK  |User(Id) |Integridad referencial: Asegura que una categoria tenga un responsable. |

