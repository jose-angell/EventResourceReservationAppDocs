---
sidebar_position: 3
title: Guía de Convenciones
---

## 📘 Guía de Convenciones (Backend)

Este documento centraliza las **reglas de estilo, patrones de diseño, convenciones de nomenclatura (naming)** y procesos que aseguran la **consistencia y alta calidad** tanto en el código del backend como en su API. Seguir estas directrices es fundamental para la mantenibilidad y colaboración en el proyecto.

---

### 1. Convenciones de Código C#

#### 1.1 Nomenclatura (Naming)

* **Clases / Interfaces / Enums:** Usar **PascalCase** (ej. `ReservationService`, `IResourceRepository`, `ReservationStatus`).
* **Métodos / Propiedades / Eventos:** Usar **PascalCase** (ej. `GetAvailableResources()`, `UserName`, `OnReservationCreated`).
* **Variables locales / Parámetros:** Usar **camelCase** (ej. `reservationId`, `resourceName`).
* **Constantes / Campos `readonly`:** Usar **PascalCase** (ej. `DefaultCacheDuration`, `MaxConcurrentReservations`).

#### 1.2 Estructura de Carpetas y Namespaces

* **Organización por Capas:** Cada capa principal del proyecto reside en su propia carpeta raíz, reflejando una arquitectura limpia y modular:
    * `/src/Api` → Controladores, DTOs de API.
    * `/src/Application` → Casos de uso (Use Cases), interfaces de servicio.
    * `/src/Domain` → Entidades de dominio, Objetos de Valor (Value Objects), interfaces de repositorio.
    * `/src/Infrastructure` → Implementaciones de EF Core, clientes HTTP, servicios de terceros.
* **Namespaces Coherentes:** Los namespaces deben reflejar la ruta de las carpetas para mantener la consistencia y la fácil ubicación del código (ej. `Company.App.Application.UseCases.Reservations`).

---

### 2. Diseño de la API REST

#### 2.1 Versionado de la API

* Siempre se debe **versionar la ruta base** de la API para permitir futuras evoluciones sin romper la compatibilidad con clientes existentes.
    * Ejemplo: `[Route("api/v1/[controller]")]`

#### 2.2 Nomenclatura de Endpoints

* Utilizar **sustantivos en plural** para los recursos, promoviendo un diseño RESTful claro y predecible.
    * `GET    /api/v1/resources`
    * `GET    /api/v1/resources/{id}`
    * `POST   /api/v1/reservations`
    * `DELETE /api/v1/reservations/{id}`
    * `PUT    /api/v1/reservations/{id}` (Para actualización completa)
    * `PATCH  /api/v1/reservations/{id}` (Para actualización parcial, si aplica)

#### 2.3 Códigos de Estado HTTP

* Retornar códigos de estado HTTP semánticamente correctos para indicar el resultado de cada operación:
    * `200 OK` – La consulta o actualización fue exitosa.
    * `201 Created` – El recurso fue creado exitosamente (respuesta a `POST`).
    * `204 No Content` – La solicitud fue procesada exitosamente, pero no hay contenido que devolver (ej. para `DELETE` exitoso).
    * `400 Bad Request` – La solicitud es inválida o la validación de datos falló.
    * `401 Unauthorized` – La autenticación es requerida o el token es inválido/ausente.
    * `403 Forbidden` – El cliente no tiene permisos para acceder al recurso.
    * `404 Not Found` – El recurso solicitado no existe.
    * `409 Conflict` – El estado actual del recurso es inválido o ya existe un recurso con datos duplicados.
    * `500 Internal Server Error` – Un error inesperado ocurrió en el servidor.

#### 2.4 Estructura de Respuestas de Error

* Las respuestas de error deben seguir un formato consistente para facilitar el manejo de errores por parte del cliente.
```jsonc
{
  "errorCode": "Resource.NotFound", // Código interno que categoriza el error
  "message": "No se encontró el recurso con id '123'.", // Mensaje legible para el desarrollador/usuario
  "details": [ /* Opcional: detalles adicionales, ej., errores de validación de modelo */ ]
}
```
---

### 3. Base de Datos y Migraciones

#### 3.1 Naming de Tablas y Columnas
    - **Tablas:** Usar **singular** y **PascalCase**: (ej. `Reservation`, `Resource`, `User`).
    - **Columnas:** Usar **PascalCase** (ej. `CreatedAt`, `IsActive`, `ReservationStatus`).
    - **Claves Foráneas (FKs)**: Seguir el patrón `{EntidadPrincipal}Id` (e.g. `ResourceId`).

#### 3.2 Migraciones de Base de Datos
    - **Nombres de Archivo:** Prefijo con la fecha en formata `YYYYMMDD` seguido de una descripción clara y concisa (ej. `20250715_AddReservationTable`, `20250720_UpdateResourceCapacity`).
    - **Propósito Claro:** Cada migración debe tener un único propósito bien definido.

### 4. Logging y Métricas
    - **Inyección de Logger:** Siempre inyectar `ILogger<T>` en las clases donde se necesite registrar eventos o errores.
    - **Mensajes Contextuales:** Los mensajes de log deben ser informativos y contener contexto relevante (ej. `reservationId`, `userId`, `resourceName`) para facilitar la depuración.
        - **Formato Recomendado:** `_logger.LogInformation("Booked reservation {ReservationId} for user {UserId}", reservationId, userId);`
    - **Métricas de Aplicación:** Si aplica, integrar librerías como Prometheus o App Metrics y exponer un endpoint /metrics para la monitorización del rendimiento.

### 5. Testing
#### 5.1 Pruebas Unitarias (Unit Tests)
    - **Ubicación:** Los proyectos de pruebas unitarias deben estar en la carpeta `tests/Unit`.
    - **Nomenclatura:** Seguir el patrón `<Feature>UseCaseTests` o `<ControllerName>Tests` (ej. `ReservationCreationUseCaseTests`, `ResourcesControllerTests`).
    - **Estructura del Test:** Cada prueba debe seguir claramente el patrón **Arrange/Act/Assert** para definir la preparación, la ejecución y la verificación.

#### 5.2 Pruebas de Integración (Integration Tests)
    - **Ubicación:** Los proyectos de pruebas de integración deben estar en la carpeta `tests/Integration`.
    -** Marco de Pruebas:** Utilizar `WebApplicationFactory<Program>` para configurar y ejecutar la aplicación de forma real en el contexto de las pruebas.
    - **Bases de Datos para Pruebas:** Emplear bases de datos en memoria (InMemory) o soluciones basadas en contenedores (Testcontainers) para garantizar la independencia y reproducibilidad de las pruebas.

### 6. Git y Flujo de Trabajo en GitHub
#### 6.1 Nomenclatura de Ramas (Branch Naming)
    - `main`: Rama principal de producción.
    - `develop`: Rama para el entorno de staging/QA, donde se integran las nuevas funcionalidades.
    - `feature/<nombre-descriptivo>`: Ramas para el desarrollo de nuevas funcionalidades (ej. `feature/add-payment-gateway`).
    - `hotfix/<nombre-descriptivo>`: Ramas para correcciones urgentes en producción (ej. `hotfix/fix-reservation-bug`).

#### 6.2 Mensajes de Commit
    - Utilizar un formato conciso y descriptivo para los mensajes de commit, siguiendo la convención de [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/):
        - **Formato:** `<tipo>(<scope>): <mensaje breve>`
        - `tipo`: `feat` (nueva característica), `fix` (corrección de bug), `chore` (tarea de mantenimiento), `docs` (cambio en documentación), `test` (cambio en pruebas).
        - `scope` (opcional): La carpeta o módulo afectado (ej. `reservations`, `auth`, `api`).
        - Ejemplo: `feat(reservations): add endpoint for listing resources by date`

### 6.3 Pull Requests (PRs)
    - **Plantilla de PR:** Utilizar una plantilla de Pull Request que incluya elementos clave para una revisión eficiente:
        - **Issue Asociado:** Referencia al issue de GitHub que resuelve (ej. `Closes #123`, `Fixes #456`).
        - **Descripción de Cambios:** Un resumen claro de lo que se ha implementado o modificado.
        - **Checklist:** Una lista de verificación para asegurar que se han completado los tests, la documentación actualizada y se cumplen las convenciones.
    