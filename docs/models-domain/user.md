---
sidebar_position: 3
title: Usuario
---

## Entidad User

 Propiedades: 
 - `Id`: `int` | Identificador unico para cada usuario.
 - `Nombre` : `string` | primer y segundo nombre del usuario
 - `PrimerApellido` : `string` | Primer apellido
 - `SegundoApellido` : `string` | Segundo apellido
 - `Correo` : `string` | Correo electronico registrado en el sistema
 - `Contraseña` : `string` | Contraseña de acceso al sistema.
 - `UbicacionId` : `int` | Llave foranea para relacionar la ubicacion ingresada
 - `Telefono` : `string` | Telefono relacionado al usuario
 - `Rol` : `int` | Llave foranea para indicar el rol que desempeña el usuario (Administrador principal, administrador secundario, Cliente).
 
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


```