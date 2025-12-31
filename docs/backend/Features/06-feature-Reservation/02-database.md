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
     public class Reservation
{
    public Guid Id { get; set; }
    public DateTime StartTime { get; set; }
    public DateTime EndTime { get; set; }
    public int StatusId { get; set; }
    public decimal TotalAmount { get; set; }
    public string ClientComment { get; set; }
    public string ClientPhoneNumber { get; set; }   
    public int LocationId { get; set; }
    public Location? Location { get; set; }
    public Guid ClientId { get; set; }
    public ApplicationUser? Client { get; set; }
    public DateTime CreatedAt { get; set; }
    public Guid? AdminId { get; set; }
    public ApplicationUser? Admin { get; set; }
    public string? AdminComment { get; set; }
    public Guid? TransactionId { get; set; }
    //public ICollection<ReservationDetail? ReservationDetail { get; set; }
    public Reservation()
    {
        ClientComment = string.Empty;
        ClientPhoneNumber = string.Empty;
        AdminComment = string.Empty;
    }
    public Reservation(DateTime startTime, DateTime endTime, int statusId, decimal totalAmount, 
        string? clientComment, string clientPhoneNumber, int locationId, Guid clientId)
    {
        if(startTime >= endTime)
        {
            throw new ArgumentException("StartTime must be earlier than EndTime.", nameof(startTime));
        }
        if( statusId <= 0)
        {
            throw new ArgumentException("StatusId cannot be empty.", nameof(statusId));
        }
        if(totalAmount <= 0)
        {
            throw new ArgumentException("TotalAmount must be a positive value.", nameof(totalAmount));
        }
        if (string.IsNullOrWhiteSpace(clientPhoneNumber))
        {
            throw new ArgumentException("ClientPhoneNumber cannot be null or empty.", nameof(clientPhoneNumber));
        }
        if (locationId == 0)
        {
            throw new ArgumentException("LocationId cannot be empty.", nameof(locationId));
        }
        if (clientId == Guid.Empty)
        {
            throw new ArgumentException("ClientId cannot be empty.", nameof(clientId));
        }
        StartTime = startTime;
        EndTime = endTime;
        StatusId = statusId;
        TotalAmount = totalAmount;
        ClientComment = clientComment ?? string.Empty;
        ClientPhoneNumber = clientPhoneNumber;
        LocationId = locationId;
        ClientId = clientId;
        AdminComment = string.Empty;
        CreatedAt = DateTime.UtcNow;
    }
    public void Update(DateTime startTime, DateTime endTime, int statusId, decimal totalAmount,
        string clientComment, string clientPhoneNumber, int locationId)
    {
        if (startTime >= endTime)
        {
            throw new ArgumentException("StartTime must be earlier than EndTime.", nameof(startTime));
        }
        if (statusId <= 0)
        {
            throw new ArgumentException("StatusId cannot be empty.", nameof(statusId));
        }
        if (totalAmount <= 0)
        {
            throw new ArgumentException("TotalAmount must be a positive value.", nameof(totalAmount));
        }
        if (string.IsNullOrWhiteSpace(clientPhoneNumber))
        {
            throw new ArgumentException("ClientPhoneNumber cannot be null or empty.", nameof(clientPhoneNumber));
        }
        if (locationId == 0)
        {
            throw new ArgumentException("LocationId cannot be empty.", nameof(locationId));
        }
        StartTime = startTime;
        EndTime = endTime;
        StatusId = statusId;
        TotalAmount = totalAmount;
        ClientComment = clientComment ?? string.Empty;
        ClientPhoneNumber = clientPhoneNumber;
        LocationId = locationId;
    }
    public void UpdateStatus(int statusId, Guid adminId, string adminComment)
    {
        if (statusId <= 0)
        {
            throw new ArgumentException("StatusId cannot be empty.", nameof(statusId));
        }
        if (adminId == Guid.Empty)
        {
            throw new ArgumentException("AdminId cannot be empty.", nameof(adminId));
        }
        StatusId = statusId;
        AdminId = adminId;
        AdminComment = adminComment ?? string.Empty;
    }
    public void UpdateTransaction(int statusId, Guid adminId, string adminComment, Guid transactionId)
    {
        if (statusId <= 0)
        {
            throw new ArgumentException("StatusId cannot be empty.", nameof(statusId));
        }
        if (adminId == Guid.Empty)
        {
            throw new ArgumentException("AdminId cannot be empty.", nameof(adminId));
        }
        if (transactionId == Guid.Empty)
        {
            throw new ArgumentException("TransactionId cannot be empty.", nameof(transactionId));
        }
        StatusId = statusId;
        AdminId = adminId;
        AdminComment = adminComment ?? string.Empty;
        TransactionId = transactionId;
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
|`<Reservation>.<CreatedByUserId>` | FK  |User(Id) |Integridad referencial: Asegura que una reserva tenga un responsable. |
|`<Reservation>.<TransactionId>` | FK  |Category(Id) |Integridad referencial: Asegura que una reservacion tenga un pago relacionado. |
|`<Reservation>.<LocationId>` | FK  |Location(Id) |Integridad referencial: Asegura que una reservacion tenga una ubicacion. |

