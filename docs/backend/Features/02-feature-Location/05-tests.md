---
sidebar_position: 4
title: 05-tests
---

# 🧪 Pruebas

> Archivo: `05-tests.md`

Este documento detalla la **estrategia de pruebas** implementada para la feature `Location Management`. Se especifica la cobertura de pruebas unitarias y de integración, así como los escenarios clave probados para asegurar la funcionalidad, fiabilidad y calidad del código.

---
## 1. Pruebas Unitarias (Unit Tests)

Las pruebas unitarias se enfocan en verificar la **lógica de negocio individual y aislada** de cada componente (clases, métodos), utilizando mocks para sus dependencias externas.

| Clase de Test                         | Componente o Lógica Probada         | Escenarios Clave / Casos de Prueba                         |
| :------------------------------------ | :---------------------------------- | :--------------------------------------------------------- |
| `CreateLocationUseCaseTests`          | Lógica de creación de ubicaiones    | - Creación exitosa de una ubicaión con datos válidos.       |
|                                       |                                     | - Fallo por validación de datos de entrada inválidos (`InvalidInput`).  |
|                                       |                                     | - Manejo de excepciones de persistencia (`PersistenceException`).  |
| `UpdateLocationUseCaseTests`          | Lógica de actualización de ubicaiones | - Actualización exitosa de una ubicaión.|
|                                       |                                     | - Fallo si la ubicaión no existe (`NotFound`).|
|                                       |                                     | - Fallo por datos inválidos de la entidad de dominio (`InvalidInput`).|
| `DeleteLocationUseCaseTests`          | Lógica de eliminación de ubicaiones | - Eliminación exitosa de una ubicaión existente. |
|                                       |                                     | - Fallo si la ubicaión no existe (`NotFound`). |
|                                       |                                     | - Manejo de excepciones de persistencia (`PersistenceException`).  |
| `ReadAllLocaitonUseCaseTests`         | Lógica de consulta de ubicaiones | - Obtención de todas las ubicaiones sin filtros|
|                                       |                                     | - Filtrado de ubicaiones por nombre de la ciudad (City).|
|                                       |                                     | - Filtrado de ubicaiones por el codigo postal (ZipCode).|
|                                       |                                     | - Filtrado de ubicaiones por ID de usuario (CreatedByUserIdFilter). |
|                                       |                                     | - Ordenación de ubicaiones por ciudad y fecha de creación. |
|  `ReadByIdLocationUseCaseTests`       | Lógica de consulta de ubicaiones por Id | - Obtención de la ubicaión por Id |
|                                       |                                     | - Filtrado de ubicaiones por Id.|
|                                      |                                      | Fallo si la ubicaión no existe (`NotFound`). |
|                                      |                                     | Manejo de excepciones de persistencia (`PersistenceException`). |

**Consideraciones Clave:**
* Se utilizan mocks (con Moq) para aislar las unidades de código y probar su lógica sin dependencias reales de base de datos o servicios externos.
* Las pruebas siguen el patrón Arrange-Act-Assert para una estructura clara y legible.

---

## 2. Pruebas de Integración (Integration Tests)
Las pruebas de integración validan la interacción entre múltiples componentes del sistema, incluyendo la API, la lógica de negocio y la persistencia de datos real (o simulada con bases de datos en memoria/contenedores).

* **Verificación de Endpoints y Flujos Completos:**
    * `POST /api/locaitons` con payload válido: Verifica la creación de una ubicaión de principio a fin, incluyendo la persistencia en la base de datos y la respuesta HTTP `201 Created`.
    * `PUT /api/locations/{id}` con payload válido: Confirma la actualización de una ubicaión, la persistencia en la base de datos y la respuesta HTTP 200 OK.
    * `GET /api/locations?nameFilter=...`: Confirma que la API devuelve los resultados correctos según los filtros, interactuando con la capa de datos.
    * `GET /api/locations/{id}`: Confirma que la API devuelve los resultados correctos según el id,, interactuando con la capa de datos.
    * `GET /api/locations/list`: Confirma que la API devuelve los resultados correctos, interactuando con la capa de datos.
    * `DELETE /api/locations/{id}`: Valida la eliminación de una ubicaión y su correcta reflejo en la base de datos.
* **Pruebas de Seguridad y Autorización:**
    * POST /api/locations sin token de autenticación: Espera HTTP 401 Unauthorized.
    * DELETE /api/locations sin permisos insuficientes: Espera HTTP 403 Forbidden.
* **Manejo de Errores y Casos de Borde:**
    * Intentar actualizar una ubicaión que no existe (HTTP 404 Not Found).
    * Escenarios de validación (HTTP 400 Bad Request) con payloads inválidos.
* **Uso de `WebApplicationFactory<T>`**: Se utiliza `WebApplicationFactory<Program>` para hospedar la aplicación de backend en memoria, permitiendo realizar llamadas HTTP reales a los controladores.
* **Base de Datos para Pruebas**: Se configura una base de datos `InMemory` (o se sugiere el uso de Testcontainers para escenarios más complejos) para asegurar la independencia de cada suite de pruebas y una ejecución rápida.

---
## 3. Cobertura de Código
La cobertura de código es una métrica clave para evaluar la calidad de las pruebas.
    - **Objetivo Mínimo**: Se aspira a una cobertura de pruebas de al menos **80%** para la lógica de los **Casos de Uso (Application Layer)** y los **Controladores (API Layer** de esta feature.
    - **Herramientas de Medición**: Se recomienda el uso de herramientas como Coverlet (integrado con .NET SDK) para generar reportes de cobertura.
