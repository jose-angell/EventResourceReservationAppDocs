---
sidebar_position: 4
title: 05-tests
---

# 🧪 Pruebas

> Archivo: `05-tests.md`

Este documento detalla la **estrategia de pruebas** implementada para la feature `Resource`. Se especifica la cobertura de pruebas unitarias y de integración, así como los escenarios clave probados para asegurar la funcionalidad, fiabilidad y calidad del código.

---
## 1. Pruebas Unitarias (Unit Tests)

Las pruebas unitarias se enfocan en verificar la **lógica de negocio individual y aislada** de cada componente (clases, métodos), utilizando mocks para sus dependencias externas.

| Clase de Test                         | Componente o Lógica Probada         | Escenarios Clave / Casos de Prueba                         |
| :------------------------------------ | :---------------------------------- | :--------------------------------------------------------- |
| `CreateResourceseCaseTests`          | Lógica de creación de Recurso    | - Creación exitosa de una nueva Recurso con datos válidos.       |
|                                       |                                     | - Fallo por validación de datos de entrada inválidos (`InvalidInput`).  |
|                                       |                                     | - Manejo de excepciones de persistencia (`PersistenceException`).  |
| `UpdateResourceUseCaseTests`          | Lógica de actualización de Recurso | - Actualización exitosa de los datos.|
|                                       |                                     | - Fallo si el Recurso no existe (`NotFound`).|
|                                       |                                     | - Fallo por datos inválidos de la entidad de dominio (`InvalidInput`).|
| `DeleteResourceUseCaseTests`          | Lógica de eliminación de Recurso | - Eliminación exitosa de un Recurso existente. |
|                                       |                                     | - Fallo si el Recurso no existe (`NotFound`). |
|                                       |                                     | - Manejo de excepciones de persistencia (`PersistenceException`).  |
|  `ReadByIdResourceUseCaseTests`       | Lógica de consulta de Recurso por Id | - Obtención de el Recurso por Id |
|                                       |                                     | - Filtrado de Recurso por Id.|
|                                      |                                      | Fallo si el Recurso no existe (`NotFound`). |
|                                      |                                     | Manejo de excepciones de persistencia (`PersistenceException`). |
|  `ReadByIdAndDateRangeResourceUseCaseTests`       | Lógica de consulta de Recurso por Id y rango de fechas | - Obtención de la reseñas por Id  y rango de fechas|
|                                       |                                     | - Filtrado de Recurso por Id y rango de fechas.|
|                                      |                                      | Fallo si el Recurso no existe (`NotFound`). |
|                                      |                                     | Manejo de excepciones de persistencia (`PersistenceException`). |



**Consideraciones Clave:**
* Se utilizan mocks (con Moq) para aislar las unidades de código y probar su lógica sin dependencias reales de base de datos o servicios externos.
* Las pruebas siguen el patrón Arrange-Act-Assert para una estructura clara y legible.

---

## 2. Pruebas de Integración (Integration Tests)
Las pruebas de integración validan la interacción entre múltiples componentes del sistema, incluyendo la API, la lógica de negocio y la persistencia de datos real (o simulada con bases de datos en memoria/contenedores).

* **Verificación de Endpoints y Flujos Completos:**
    * `POST /api/Resource` con payload válido: Verifica la creación de un nuevo recurso de principio a fin, incluyendo la persistencia en la base de datos y la respuesta HTTP `201 Created`.
    * `PUT /api/Resource/{id}` con payload válido: Confirma la actualización de un recurso, la persistencia en la base de datos y la respuesta HTTP 200 OK.
    * `GET /api/Resource?clientId=...`: Confirma que la API devuelve los resultados correctos según los filtros, interactuando con la capa de datos.
    * `DELETE /api/Resource/{id}`: Valida la eliminación de un recurso y su correcta reflejo en la base de datos.
* **Pruebas de Seguridad y Autorización:**
    * POST /api/Resource sin token de autenticación: Espera HTTP 401 Unauthorized.
    * DELETE /api/Resource sin permisos insuficientes: Espera HTTP 403 Forbidden.
* **Manejo de Errores y Casos de Borde:**
    * Intentar actualizar un recurso que no existe (HTTP 404 Not Found).
    * Escenarios de validación (HTTP 400 Bad Request) con payloads inválidos.
* **Uso de `WebApplicationFactory<T>`**: Se utiliza `WebApplicationFactory<Program>` para hospedar la aplicación de backend en memoria, permitiendo realizar llamadas HTTP reales a los controladores.
* **Base de Datos para Pruebas**: Se configura una base de datos `InMemory` (o se sugiere el uso de Testcontainers para escenarios más complejos) para asegurar la independencia de cada suite de pruebas y una ejecución rápida.

---
## 3. Cobertura de Código
La cobertura de código es una métrica clave para evaluar la calidad de las pruebas.
    - **Objetivo Mínimo**: Se aspira a una cobertura de pruebas de al menos **80%** para la lógica de los **Casos de Uso (Application Layer)** y los **Controladores (API Layer)** de esta feature.
    - **Herramientas de Medición**: Se recomienda el uso de herramientas como Coverlet (integrado con .NET SDK) para generar reportes de cobertura.
