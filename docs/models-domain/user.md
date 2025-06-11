---
sidebar_position: 3
title: Usuario
---

## Entidad User

 Propiedades: `Id`, `Nombre`, `Correo`, `Rol` (Administrador, Cliente).
 
 ``` mermaid
erDiagram
    User {
        int Id
        string Nombre
        string Correo
        string Rol
    }
    
    User ||--o{ Reservation : "realiza"
```