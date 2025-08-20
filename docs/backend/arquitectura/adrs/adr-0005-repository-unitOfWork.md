---
sidebar_position: 6
title: ADR 0005:Patrones Repository y Unit of Work
---

# ADR 0005:   Implementación de los Patrones Repository y Unit of Work

* **Fecha:** 2025-07-27
* **Estado:** [Aceptada]

## Contexto

En el desarrollo de la aplicación EventResourceReservationApp, se ha optado por seguir una arquitectura limpia (Clean Architecture). Esta decisión nos exige mantener las capas de dominio y aplicación desacopladas de los detalles de la infraestructura, como la base de datos y el ORM (Object-Relational Mapper).
Para gestionar la persistencia de datos de forma robusta, eficiente y desacoplada, se hace necesario un mecanismo que abstraiga las operaciones CRUD (Crear, Leer, Actualizar, Borrar) y que orqueste la gestión de transacciones. Esto nos permitirá cambiar el proveedor de persistencia (ej. de PostgreSQL a SQL Server o a una base de datos NoSQL) sin modificar la lógica de negocio central y tambien para permitir crear pruebas de forma mas controlada y facil durante el desarrollo.

## Decisión

Se ha decidido implementar los patrones **Repository** y **Unit of Work** para gestionar la persistencia de datos en la aplicación.
    - **Patrón Repository:** Se creará una interfaz genérica `IRepository<T>` en la capa de **Application** para definir las operaciones de consulta y persistencia de entidades. Esta interfaz será implementada por una clase concreta `Repository<T>` en la capa de **Infraestructura**, utilizando **Entity Framework Core**. Esta implementación concreta se encargará de interactuar directamente con el `DbContext`. Se seguirán estos principios:
        - **Interfaces en Application:** Las interfaces (`IRepository<T>`, `ICategoryRepository`, etc.) vivirán en la capa de Application, definiendo el contrato que se necesita en la persistencia.
        - **Implementaciones en Infraestructura:** Las clases concretas (`Repository<T>`, `CategoryRepository`, etc.) vivirán en la capa de Infraestructura, conteniendo la lógica específica de EF Core.
        - **Abstracción:** La lógica de negocio en la capa de Aplicación solo dependerá de las interfaces, no de las implementaciones.
    - **Patrón Unit of Work:** Se creará una interfaz `IUnitOfWork` en la capa de Aplicación. Esta interfaz servirá como una abstracción para gestionar transacciones y guardar cambios. La implementación concreta `UnitOfWork` se colocará en la capa de **Infraestructura** y encapsulará el `DbContext`. Su responsabilidad principal será:
        - **Orquestación de Repositorios:** Proporcionará acceso a las instancias de los repositorios específicos (ej., `IUnitOfWork.Categories`).
        - **Gestión de Transacciones:** Tendrá un método `SaveAsync()` que aplicará todos los cambios pendientes en el `DbContext` como una única transacción, asegurando la atomicidad de las operaciones.

## Alternativas Consideradas

- **Repository sin Unit of Work:** Se descartó porque no ofrece una gestión transaccional clara para múltiples operaciones de escritura. Dejaría la responsabilidad de SaveChanges() a cada repositorio, lo que podría llevar a inconsistencias.
- **Usar el `DbContext` directamente en la capa de Aplicación:** Se descartó por completo. Rompería la regla de dependencia de la arquitectura limpia, acoplando la lógica de negocio a una tecnología de infraestructura (DbContext). Esto haría las pruebas y el mantenimiento significativamente más difíciles.

## Consecuencias

### Positivas (+)
* **Desacoplamiento:** La capa de Dominio y Aplicación no tienen dependencias directas de Entity Framework Core o de la base de datos. Esto facilita las pruebas unitarias y de integración.
* **Mantenibilidad:** El código es más fácil de mantener, ya que la lógica de persistencia está centralizada y separada de la lógica de negocio.
* **Consistencia de Datos:** El patrón Unit of Work garantiza que múltiples operaciones de escritura se manejen en una única transacción, evitando estados inconsistentes en la base de datos.
* **Flexibilidad:** Permite cambiar fácilmente el proveedor de base de datos o el ORM en el futuro sin reescribir gran parte de la aplicación.

### Negativas (-)
* **Complejidad Inicial:** Añade una capa de abstracción que requiere más interfaces y clases al inicio del proyecto.
* **Curva de Aprendizaje:** Requiere que los desarrolladores entiendan la implementación de estos patrones y cómo interactúan con las diferentes capas de la arquitectura.
* **Sobrecarga de Código:** Para funcionalidades muy simples, la abstracción puede parecer excesiva. Sin embargo, se considera una inversión necesaria para la escalabilidad y mantenibilidad a largo plazo del proyecto.

---