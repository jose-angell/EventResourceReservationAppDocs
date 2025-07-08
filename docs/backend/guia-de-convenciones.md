---
sidebar_position: 3
title:  Guía de convenciones
---

## 📘 Guía de convenciones (Backend)

Este documento recoge las reglas de estilo, patrones de diseño, naming y procesos que garantizan consistencia y calidad en el código y la API del backend.

---

### 1. Convenciones de Código C#

#### 1.1 Naming de elementos  
- **Clases / Interfaces / Enums:** PascalCase (e.g. `ReservationService`, `IResourceRepository`).  
- **Métodos / Propiedades / Eventos:** PascalCase (e.g. `GetAvailableResources()`).  
- **Variables locales / parámetros:** camelCase (e.g. `reservationId`).  
- **Constantes / readonly:** PascalCase (e.g. `DefaultCacheDuration`).


#### 1.2 Estructura de carpetas y namespaces  
- Cada capa en su carpeta raíz:  /src /Api → Controllers, DTOs /Application → UseCases, Interfaces /Domain → Entidades, ValueObjects /Infrastructure → EF Core, clientes HTTP
- Namespaces reflejan la ruta de carpetas: `namespace Company.App.Application.UseCases.Reservations`


---

### 2. Diseño de la API REST

#### 2.1 Versionado  
- Siempre versiona tu ruta base:  `[Route("api/v1/[controller]")]`


#### 2.2 Naming de endpoints  
- Plural para recursos:  
- `GET    /api/v1/resources`  
- `GET    /api/v1/resources/{id}`  
- `POST   /api/v1/reservations`  
- `DELETE /api/v1/reservations/{id}`  

#### 2.3 Códigos de estado  
- `200 OK` – Consulta exitosa.  
- `201 Created` – Creación exitosa.  
- `400 Bad Request` – Validación fallida.  
- `401 Unauthorized` – Sin token o inválido.  
- `403 Forbidden` – Sin permiso.  
- `404 Not Found` – Recurso no existe.  
- `409 Conflict` – Estado inválido o duplicado.  
- `500 Internal Server Error` – Error imprevisto.

#### 2.4 Estructura de respuestas de error  
```jsonc
{
"errorCode": "Resource.NotFound",
"message": "No se encontró el recurso con id '123'.",
"details": [ ... ]           // Opcional, para validaciones de modelo
}
```

---

### 3. Base de datos y migraciones
#### 3.1 Naming de tablas y columnas
    - Tablas en singular PascalCase: Reservation, Resource.
    - Columnas PascalCase: CreatedAt, IsActive.
    - FKs: `{Entidad}Id` (e.g. ResourceId).

#### 3.2 Migraciones
    - Nombre con prefijo fecha: `YYYYMMDD_<Descripción>`
    - Descripción clara: `20250715_AddReservationTable`

### 4. Logging y métricas
    - Logger: inyectar `ILogger<T>`.
    - Mensajes:
        - Deben contener contexto (e.g. reservationId, userId).
        - Formato: `_logger.LogInformation("Booked reservation {ReservationId} for user {UserId}", reservationId, userId);`
    - Métricas: usar Prometheus / App Metrics si aplica, expón /metrics.

### 5. Testing
#### 5.1 Unit tests
    - Proyecto: tests/Unit.
    - Naming: `<Feature>UseCaseTests`, `<ControllerName>Tests`.
    - Arrange/Act/Assert claras en cada test.

#### 5.2 Integration tests
    - Proyecto: tests/Integration.
    - Usar `WebApplicationFactory<Program>`.
    - Base de datos InMemory o Testcontainers.

### 6. Git y flujo de trabajo en GitHub
#### 6.1 Branch naming
    - main → producción.
    - develop → staging/QA.
    - feature/`<nombre>` → nuevas features.
    - hotfix/`<nombre>` → correcciones urgentes.

#### 6.2 Commit messages
    - Formato: `<tipo>(<scope>): <mensaje breve>`
    - tipo: feat, fix, chore, docs, test.
    - scope: carpeta o módulo (e.g. reservations).
    - Ejemplo: feat(reservations): add endpoint for listing resources by date

### 6.3 Pull Requests
    - Usa plantilla PR que incluya:
        -Issue asociado (Closes #123).
        -Descripción de cambios.
        -Checklist de tests y documentación.