---
sidebar_position: 2
title: Recurso
---

## Entidad Resource

Propiedades: 
- `Id` : `int` | Identificador unico para cada recurso.
- `TipoId` : `int` | Llave foranea para relacionar al recurso con el catologo de tipo de recruso (mesa, silla, etc.).
- `EstadoId` : `int` | Llave foranea para relacionar lo diferentes posibles estados del recurso  (Disponible, Bloqueado, Eliminado).
- `Nombre` : `string` | Nombre o descripcion breve del recurso.
- `Descripcion` : `string` | Descripcion del Recurso.
- `CantidadDisponible` : `int` |  Cantidad disponibles de los recursos para ser reservado. 
- `Precio` : `numeric` | Precio por unidad, en peso mexicano.
- `TipoAutorizacion` : `int` | Se puede especificar si el recurso se puede autorizar automaticamente al reservarla o si necesita que un administrador lo revise y lo autorice. 
- `DireccionId` : `int` | Llave foranea para relacionar la direccion almacenada en la tabla de direcciones.
- `UsuarioCreacionId` : `int` | Id de el usuario que crea el recurso.
- `FechaCreacion` : `Datetime` | Fecha de la creacion del recurso.


**Nota:** Las reservas tiene la capacidad de guarar imagenes pero estas se almacenan de forma distinta

 ``` mermaid
erDiagram
    Resource {
        int Id
        int TipoId
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

```