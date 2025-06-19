---
sidebar_position: 4
title: Usuario
---

## Entidad User

Propiedades: 
- `Id`: `int` | Identificador único para cada usuario.
- `Nombre` : `string` | Primer y segundo nombre del usuario
- `PrimerApellido` : `string` | Primer apellido
- `SegundoApellido` : `string` | Segundo apellido
- `Correo` : `string` | Correo electrónico registrado en el sistema
- `Contraseña` : `string` | Contraseña de acceso al sistema.
- `UbicacionId` : `int` | Llave foránea para relacionar la ubicación ingresada
- `Telefono` : `string` | Teléfono relacionado al usuario
- `Rol` : `int` | Llave foránea para indicar el rol que desempeña el usuario (Administrador principal, administrador secundario, Cliente).

``` mermaid
erDiagram
    User {
        int Id
        string Nombre
        string PrimerApellido
        string SegundoApellido
        string Correo
        string Contraseña
        int UbicacionId
        string Telefono
        int Rol
    }

    Location ||--o{ User : "tiene ubicación"
    Rol ||--o{ User : "desempeña rol"