---
sidebar_position: 7
title: Carrito de compras
---

## Entidad ShoppingCart

Propiedades: 
- `Id` : `int` | Identificador único del carrito de compras.
- `RecursoId` : `int` | Llave foránea para saber qué recurso está relacionado con el carrito
- `Cantidad` : `int` | Cantidad de unidades a reservar
- `ClienteId` : `int` | Llave foránea para identificar al usuario al que le pertenece el carrito.

## Diagrama
```mermaid
erDiagram
    shoppingCart {
        int Id
        int RecursoId
        int Cantidad
        int ClienteId
    }

    User ||--o{ shoppingCart : "posee"
    Resource ||--o{ shoppingCart : "incluye recurso"


```