---
sidebar_position: 4
title: 05-tests
---

# 🧪 Pruebas

> Archivo: `05-tests.md`

Este documento detalla la **estrategia de pruebas** implementada para la feature `<Nombre de la Feature>`. Se especifica la cobertura de pruebas unitarias y de integración, así como los escenarios clave probados para asegurar la funcionalidad, fiabilidad y calidad del código.

---
## 1. Pruebas Unitarias (Unit Tests)

Las pruebas unitarias se enfocan en verificar la **lógica de negocio individual y aislada** de cada componente (clases, métodos), utilizando mocks para sus dependencias externas.

| Clase de Test                         | Componente o Lógica Probada         | Escenarios Clave / Casos de Prueba                         |
| :------------------------------------ | :---------------------------------- | :--------------------------------------------------------- |
| `ReservationCreationUseCaseTests`     | Lógica de creación de reservas      | - Creación exitosa de una reserva con datos válidos.      |
|                                       |                                     | - Fallo por validación de datos de entrada (ej. fecha inválida). |
|                                       |                                     | - Manejo de excepciones al intentar interactuar con un repositorio mockeado. |
| `ResourceServiceTests`                | Lógica de obtención de recursos     | - Obtención de recursos disponibles para una fecha.        |
|                                       |                                     | - Filtrado de recursos por tipo.                           |
| `ReservationDomainTests`              | Métodos y lógica de la entidad `Reservation` | - Cambiar estado de reserva a 'Confirmado' correctamente. |
|                                       |                                     | - Intentar cambiar a un estado inválido (ej. 'Cancelado' a 'Pendiente'). |
| `...`                                 | `...`                               | `...`                                                    |

**Consideraciones Clave:**
* Se utilizan **mocks** (con Moq) para aislar las unidades de código y probar su lógica sin dependencias reales de base de datos o servicios externos.
* Las pruebas siguen el patrón **Arrange-Act-Assert** para una estructura clara y legible.

---

## 2. Pruebas de Integración (Integration Tests)
Las pruebas de integración validan la **interacción entre múltiples componentes** del sistema, incluyendo la API, la lógica de negocio y la persistencia de datos real (o simulada con bases de datos en memoria/contenedores).

* **Verificación de Endpoints y Flujos Completos:**
    * `POST /api/v1/reservations` con payload válido: Verifica la creación de una reserva de principio a fin, incluyendo la persistencia en la base de datos y la respuesta `HTTP 201 Created`.
    * `GET /api/v1/resources?date=YYYY-MM-DD`: Confirma que la API devuelve los recursos correctos según el filtro, interactuando con la capa de datos.
    * `DELETE /api/v1/reservations/{id}`: Valida la eliminación de una reserva y su correcta reflejo en la base de datos.
* **Pruebas de Seguridad y Autorización:**
    * `GET /api/v1/reservations` sin token de autenticación: Espera `HTTP 401 Unauthorized`.
    * `POST /api/v1/reservations` con token inválido o permisos insuficientes: Espera `HTTP 403 Forbidden`.
* **Manejo de Errores y Casos de Borde:**
    * Creación de reserva con conflicto de horario (`HTTP 409 Conflict`).
    * Intentar acceder a un recurso que no existe (`HTTP 404 Not Found`).
    * Escenarios de validación (`HTTP 400 Bad Request`) con payloads inválidos.
* **Uso de `WebApplicationFactory<T>`:** Se utiliza `WebApplicationFactory<Program>` para hospedar la aplicación de backend en memoria, permitiendo realizar llamadas HTTP reales a los controladores.
* **Base de Datos para Pruebas:** Se configura una base de datos **InMemory** (o se sugiere el uso de Testcontainers para escenarios más complejos) para asegurar la independencia de cada suite de pruebas y una ejecución rápida.

---
## 3. Cobertura de Código
La cobertura de código es una métrica clave para evaluar la calidad de las pruebas.

* **Objetivo Mínimo:** Se aspira a una cobertura de pruebas de al menos **80%** para la lógica de los **Casos de Uso (Application Layer)** y los **Controladores (API Layer)** de esta feature.
* **Herramientas de Medición:** Se recomienda el uso de herramientas como **Coverlet** (integrado con .NET SDK) para generar reportes de cobertura.


