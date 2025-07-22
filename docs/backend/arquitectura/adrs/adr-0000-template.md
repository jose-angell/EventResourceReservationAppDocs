---
sidebar_position: 1
title: ADR XXXX:Título de la Decisión
---

# ADR XXXX:  `<Título de la Decisión>`

* **Fecha:** YYYY-MM-DD
* **Estado:** [Propuesta | Aceptada | Obsoleta | Reemplazada por ADR YYYY]

## Contexto

Describe el problema o la situación que llevó a la necesidad de tomar esta decisión. ¿Cuáles eran las presiones, los requisitos o los desafíos?

* Ejemplo: "Necesitamos una forma de interactuar con la base de datos relacional de manera eficiente desde C#. La escritura de SQL plano es propensa a errores y ralentiza el desarrollo."

## Decisión

La decisión tomada. Sé claro y conciso sobre la solución elegida.

* Ejemplo: "Decidimos utilizar Entity Framework Core como nuestro Object-Relational Mapper (ORM)."

## Alternativas Consideradas

Lista las otras opciones que se evaluaron y una breve justificación de por qué no fueron elegidas.

* **Dapper:** Una micro-ORM. Más rápido que EF Core en ciertas operaciones, pero requiere escribir más SQL o mapeo manual. Elegido no usarlo por su menor productividad para CRUD complejo y menor encapsulación del modelo de dominio.
* **SQL puro con ADO.NET:** Máximo control y rendimiento, pero muy lento para el desarrollo, propenso a errores y requiere más código repetitivo.

## Consecuencias

Describe los pros y los contras de la decisión tomada. ¿Qué se gana y qué se pierde? ¿Qué implicaciones tiene para el futuro?

### Positivas (+)
* Mayor productividad en el desarrollo de operaciones CRUD.
* Menos código repetitivo y más fácil de mantener.
* Soporte integrado para migraciones de esquema.
* Comunidad y documentación robustas.

### Negativas (-)
* Curva de aprendizaje inicial para algunos miembros del equipo.
* Posibles problemas de rendimiento en consultas muy complejas si no se optimiza cuidadosamente.
* Abstracción que puede ocultar la complejidad real de la base de datos si no se entiende bien.

---