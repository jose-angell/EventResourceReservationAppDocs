---
sidebar_position: 2
title: ADR 0001:Uso de EF core
---

# ADR 0001:  Uso de EF Core 

* **Fecha:** 2025-07-08
* **Estado:** [Aceptada]

## Contexto

Necesitamos una forma de interactuar con la base de datos relacional de manera eficiente desde C#. La escritura de SQL plano es propensa a errores y ralentiza el desarrollo.

## Decisión

Decidimos utilizar Entity Framework Core como nuestro Object-Relational Mapper (ORM).

## Alternativas Consideradas

* **Dapper:** Una micro-ORM. Más rápido que EF Core en ciertas operaciones, pero requiere escribir más SQL o mapeo manual. Elegido no usarlo por su menor productividad para CRUD complejo y menor encapsulación del modelo de dominio.
* **SQL puro con ADO.NET:** Máximo control y rendimiento, pero muy lento para el desarrollo, propenso a errores y requiere más código repetitivo.

## Consecuencias

### Positivas (+)
* Mayor productividad en el desarrollo de operaciones CRUD.
* Menos código repetitivo y más fácil de mantener.
* Soporte integrado para migraciones de esquema.
* Comunidad y documentación robustas.

### Negativas (-)
* Curva de aprendizaje inicial para algunos miembros del equipo.
* Posibles problemas de rendimiento en consultas muy complejas si no se optimiza cuidadosamente.
* Abstracción que puede ocultar la complejidad real de la base de datos si no se entiende bien.

