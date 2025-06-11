---
sidebar_position: 2
title: Recurso
---

## Entidad Resource

Propiedades: `Id`, `Tipo` (mesa, silla, etc.), `Estado` (disponible, reservado), `Descripción`.

 ``` mermaid
erDiagram
    Resource {
        int Id
        string Tipo
        string Estado
        string Descripcion
    }
    
    Resource ||--o{ Reservation : "es reservado en"

```