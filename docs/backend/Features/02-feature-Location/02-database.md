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
    public class Location
    {
        public int Id { get; set; }
        public string Country { get; set; }
        public string City { get; set; }
        public int ZipCode { get; set; }
        public string Street { get; set; }
        public string Neighborhood { get; set; }
        public string ExteriorNumber { get; set; }
        public string InteriorNumber { get; set; }
        public Guid CreatedByUserId { get; set; }
        public DateTime CreatedAt { get; set; }

        public Location()
        {
            Country = string.Empty;
            City = string.Empty;
            Street = string.Empty;
            Neighborhood = string.Empty;
            ExteriorNumber = string.Empty;
            InteriorNumber = string.Empty;
        }
        public Location(string country, string city, int zipCode, string street, string neighborhood, string exteriorNumber, string interiorNumber, Guid createdByUserId)
        {
            if (string.IsNullOrWhiteSpace(country))
            {
                throw new ArgumentException("Country cannot be null or empty.", nameof(country));
            }
            if (string.IsNullOrWhiteSpace(city))
            {
                throw new ArgumentException("City cannot be null or empty.", nameof(city));
            }
            if (zipCode <= 0)
            {
                throw new ArgumentException("ZipCode must be a positive integer.", nameof(zipCode));
            }
            if (string.IsNullOrWhiteSpace(street))
            {
                throw new ArgumentException("Street cannot be null or empty.", nameof(street));
            }
            if (string.IsNullOrWhiteSpace(exteriorNumber))
            {
                throw new ArgumentException("ExteriorNumber cannot be null or empty.", nameof(neighborhood));
            }
            if (createdByUserId == Guid.Empty)
            {
                throw new ArgumentException("CreatedByUserId cannot be empty.", nameof(createdByUserId));
            }
            Country = country;
            City = city;
            ZipCode = zipCode;
            Street = street;
            Neighborhood = neighborhood ?? string.Empty;
            ExteriorNumber = exteriorNumber;
            InteriorNumber = interiorNumber ?? string.Empty;
            CreatedByUserId = createdByUserId;
            CreatedAt = DateTime.UtcNow;
        }
        public void Update(string country, string city, int zipCode, string street, string neighborhood, string exteriorNumber, string interiorNumber)
        {
            if (string.IsNullOrWhiteSpace(country))
            {
                throw new ArgumentException("Country cannot be null or empty.", nameof(country));
            }
            if (string.IsNullOrWhiteSpace(city))
            {
                throw new ArgumentException("City cannot be null or empty.", nameof(city));
            }
            if (zipCode <= 0)
            {
                throw new ArgumentException("ZipCode must be a positive integer.", nameof(zipCode));
            }
            if (string.IsNullOrWhiteSpace(street))
            {
                throw new ArgumentException("Street cannot be null or empty.", nameof(street));
            }
            if (string.IsNullOrWhiteSpace(exteriorNumber))
            {
                throw new ArgumentException("ExteriorNumber cannot be null or empty.", nameof(neighborhood));
            }
            Country = country;
            City = city;
            ZipCode = zipCode;
            Street = street;
            Neighborhood = neighborhood ?? string.Empty;
            ExteriorNumber = exteriorNumber;
            InteriorNumber = interiorNumber ?? string.Empty;
        }
    }
}
```

## 2. Migraciones de Base de Datos
Lista todas las migraciones de EF Core creadas o modificadas para esta feature.
    - `20250821043923_20250820_InitialLocationMitragion`: Crea la tabla de Ubicacines con sus campos iniciales.
    - `20250823051935_20250822_FixCreatedDateLocation` : correccion en el campo de control para la fecha de creacion.

## 3. Relaciones e Índices
Documenta las relaciones entre tablas (claves foráneas) y los índices creados para optimizar el rendimiento de las consultas y asegurar la integridad de los datos.

| Columna | Tipo Tipo / Cardinalidad <br/> (FK/INDEX)	 | 	Columna Destino (si es FK) | Propósito / Justificación |
|---------|------------------|------------|----------------------|
|`<Locations>.<CreatedByUserId>` | FK  |User(Id) |Integridad referencial: Asegura que una ubicacion tenga un responsable. |

