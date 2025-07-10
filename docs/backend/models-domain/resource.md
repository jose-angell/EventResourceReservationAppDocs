---
sidebar_position: 3
title: Recurso
---

## Entidad Resource

Propiedades: 
- `Id` : `int` | Identificador único para cada recurso.
- `CategoryId` : `int` | Llave foránea para relacionar al recurso con el catálogo de tipo de recurso (mesa, silla, etc.).
- `EstadoId` : `int` | Llave foránea para relacionar los diferentes posibles estados del recurso (Disponible, Bloqueado, Eliminado).
- `Nombre` : `string` | Nombre o descripción breve del recurso.
- `Descripcion` : `string` | Descripción del recurso.
- `CantidadDisponible` : `int` |  Cantidad disponible de los recursos para ser reservados. 
- `Precio` : `numeric` | Precio por unidad, en peso mexicano.
- `TipoAutorizacion` : `int` | Se puede especificar si el recurso se puede autorizar automáticamente al reservarlo o si necesita que un administrador lo revise y lo autorice. 
- `DireccionId` : `int` | Llave foránea para relacionar la dirección almacenada en la tabla de direcciones.
- `UsuarioCreacionId` : `int` | Id del usuario que crea el recurso.
- `FechaCreacion` : `Datetime` | Fecha de la creación del recurso.


**Nota:** Las reservas tienen la capacidad de guardar imágenes pero estas se almacenan de forma distinta

 ``` mermaid
erDiagram
    Resource {
        int Id
        int CategoryId
        int EstadoId
        string Nombre
        string Descripcion
        int CantidadDisponible
        numeric Precio
        int TipoAutorizacion
        int DireccionId
        int UsuarioCreacionId
        datetime FechaCreacion
    }

    Tipo ||--o{ Resource : "clasifica"
    Estado ||--o{ Resource : "define estado"
    Direccion ||--o{ Resource : "ubicado en"
    User ||--o{ Resource : "crea"
    Resource ||--o{ Reservation : "es reservado en"