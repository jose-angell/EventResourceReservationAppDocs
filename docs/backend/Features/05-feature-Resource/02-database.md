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
Definicion de las entidades de dominio y su mapeo a la base de datos (tablas, agregados). Se decidio utiliar un Id de tipo GUID para los recursos, se agregaron campos de control para agilizar la disponibilidad de los recursos.


### Opción B: Definición con Clases de C# (EF Core Entities)
```c#
namespace ResourceReservationApp.Domain.Entities
{
      public class Resource
  {
      public Guid Id { get; set; }
      public int CategoryId { get; set; }
      public Category? Category { get; set; }
      public int StatusId { get; set; }
      public string Name { get; set; }
      public string Description { get; set; }
      public int Quantity { get; set; }
      public decimal Price { get; set; }
      public int AuthorizationType { get; set; }
      public int LocationId { get; set; }
      public Location? Location { get; set; }
      public Guid CreatedByUserId { get; set; }
      public ApplicationUser? CreatedByUser { get; set; }
      public DateTime CreatedAt { get; set; }
      //public ICollection<Reservation>? Reservations { get; set; }
      public Resource()
      {
          Name = string.Empty;
          Description = string.Empty;
      }
      public Resource(int categoryId, string name, string description, int quantity, decimal price,
          int authorizationType, int locationId, Guid createdByUserId)
      {
          if (categoryId == 0)
          {
              throw new ArgumentException("CategoryId cannot be empty.", nameof(categoryId));
          }
          if (string.IsNullOrWhiteSpace(name))
          {
              throw new ArgumentException("Name cannot be null or empty.", nameof(name));
          }
          if (string.IsNullOrWhiteSpace(description))
          {
              throw new ArgumentException("Description cannot be null or empty.", nameof(description));
          }
          if (quantity < 0)
          {
              throw new ArgumentException("AvailableQuantity cannot be negative.", nameof(quantity));
          }
          if (price < 0)
          {
              throw new ArgumentException("UnitPrice cannot be negative.", nameof(price));
          }
          if (authorizationType < 0)
          {
              throw new ArgumentException("AuthorizationType cannot be negative.", nameof(authorizationType));
          }
          if (locationId == 0)
          {
              throw new ArgumentException("LocationId cannot be empty.", nameof(locationId));
          }
          if (createdByUserId == Guid.Empty)
          {
              throw new ArgumentException("userId cannot be empty.", nameof(createdByUserId));
          }

          CategoryId = categoryId;
          StatusId = 1;
          Name = name;
          Description = description;
          Quantity = quantity;
          Price = price;
          AuthorizationType = authorizationType;
          LocationId = locationId;
          CreatedByUserId = createdByUserId;
          CreatedAt = DateTime.UtcNow;
      }
      public void Update(int categoryId, int statud, string name, string description, int quantity, decimal price,
          int authorizationType, int locationId)
      {
          if (categoryId == 0)
          {
              throw new ArgumentException("CategoryId cannot be empty.", nameof(categoryId));
          }
          if (string.IsNullOrWhiteSpace(name))
          {
              throw new ArgumentException("Name cannot be null or empty.", nameof(name));
          }
          if (string.IsNullOrWhiteSpace(description))
          {
              throw new ArgumentException("Description cannot be null or empty.", nameof(description));
          }
          if (quantity < 0)
          {
              throw new ArgumentException("AvailableQuantity cannot be negative.", nameof(quantity));
          }
          if (price < 0)
          {
              throw new ArgumentException("UnitPrice cannot be negative.", nameof(price));
          }
          if (authorizationType < 0)
          {
              throw new ArgumentException("AuthorizationType cannot be negative.", nameof(authorizationType));
          }
          if (locationId == 0)
          {
              throw new ArgumentException("LocationId cannot be empty.", nameof(locationId));
          }
          if (statud < 0)
          {
              throw new ArgumentException("StatusId cannot be negative.", nameof(statud));
          }
          CategoryId = categoryId;
          StatusId = statud;
          Name = name;
          Description = description;
          Quantity = quantity;
          Price = price;
          AuthorizationType = authorizationType;
          LocationId = locationId;
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
|`<Resource>.<CreatedByUserId>` | FK  |User(Id) |Integridad referencial: Asegura que una recurso tenga un responsable. |
|`<Resource>.<CategoryId>` | FK  |Category(Id) |Integridad referencial: Asegura que una recurso tenga una categoria. |
|`<Resource>.<LocationId>` | FK  |Location(Id) |Integridad referencial: Asegura que una recurso tenga una ubicacion. |

