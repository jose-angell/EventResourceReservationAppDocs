# Event Resource Reservation App — Backend

Backend de una plataforma de **reserva de recursos para eventos**, desarrollado con **.NET 8**, **ASP.NET Core**, **PostgreSQL** y **Entity Framework Core**.

La aplicación administra recursos como mobiliario y equipo para eventos, permitiendo consultar disponibilidad por rango de fechas, gestionar reservas, usuarios, ubicaciones, categorías y reseñas.

## Objetivo

Construir una API backend que centralice la administración y reserva de recursos, aplicando reglas de disponibilidad, autenticación, persistencia relacional, logging y pruebas automatizadas.

## Funcionalidades principales

- API REST para recursos, reservas, categorías, ubicaciones y reseñas.
- Consulta de disponibilidad de recursos por rango de fechas.
- Gestión de reservas y detalle de recursos asociados.
- Autenticación y autorización mediante JWT y ASP.NET Core Identity.
- Persistencia con PostgreSQL y Entity Framework Core.
- Logging estructurado mediante Serilog.
- Documentación interactiva mediante Swagger/OpenAPI.
- Ejecución de API y PostgreSQL mediante Docker.
- Pruebas unitarias para reglas de dominio y casos de uso.

## Arquitectura

La solución está separada en proyectos con responsabilidades diferenciadas:

```text
EventResourceReservationApp
├── EventResourceReservationApp.Api
├── EventResourceReservationApp.Application
├── EventResourceReservationApp.Domain
├── EventResourceReservationApp.Infrastructure
├── EventResourceReservationApp.UnitTests
└── EventResourceReservationApp.IntegrationTests
```

### API

Punto de entrada HTTP de la aplicación. Configura controladores, autenticación JWT, autorización, Swagger, manejo de errores y logging de requests.

### Application

Contiene DTOs, contratos y casos de uso que coordinan las operaciones de la aplicación.

### Domain

Contiene las entidades principales del sistema y sus reglas asociadas.

### Infrastructure

Implementa persistencia con Entity Framework Core y PostgreSQL, repositorios, Unit of Work, Identity y generación de tokens.

### Tests

`EventResourceReservationApp.UnitTests` contiene pruebas sobre entidades de dominio y casos de uso mediante **xUnit**, **Moq** y **FluentAssertions**.

El proyecto `EventResourceReservationApp.IntegrationTests` ya está creado y configurado con `WebApplicationFactory`/Testcontainers como dependencias, pero la suite de integración todavía está pendiente de implementación real.

> La solución explora separación por capas y principios asociados con Clean Architecture, pero no se presenta como una implementación estricta: actualmente el dominio mantiene una dependencia con ASP.NET Core Identity y existen dependencias de infraestructura que todavía pueden desacoplarse.

## Stack tecnológico

### Backend

- .NET 8
- C#
- ASP.NET Core Web API
- ASP.NET Core Identity
- JWT Authentication

### Persistencia

- PostgreSQL
- Entity Framework Core
- Repository Pattern
- Unit of Work

### Observabilidad y API

- Serilog
- Swagger / OpenAPI

### Testing

- xUnit
- Moq
- FluentAssertions
- Coverlet

### Infraestructura

- Docker
- Docker Compose

## Regla de disponibilidad

Uno de los casos de uso centrales permite consultar un recurso dentro de un rango de fechas y calcular su disponibilidad considerando las cantidades involucradas en reservas existentes.

Esta lógica permite que la API exponga disponibilidad contextual en lugar de tratar el inventario únicamente como un valor estático.

## Ejecución local

### Requisitos

- .NET SDK 8
- Docker Desktop
- `dotnet-ef`

### 1. Clonar el repositorio

```bash
git clone https://github.com/jose-angell/EventResourceReservationAppBackend.git
cd EventResourceReservationAppBackend
```

### 2. Iniciar PostgreSQL

Puedes levantar únicamente la base de datos mediante Docker Compose:

```bash
docker compose up -d db_postgres
```

Por defecto PostgreSQL queda disponible en:

```text
localhost:5432
```

### 3. Configurar la aplicación

La API necesita:

- `ConnectionStrings:DefaultConnection`
- `JwtSettings:SecretKey`
- `JwtSettings:Issuer`
- `JwtSettings:Audience`

Para un entorno local se recomienda utilizar `appsettings.Development.json` o User Secrets y evitar publicar credenciales reales en el repositorio.

### 4. Aplicar migraciones

Desde la raíz:

```bash
dotnet ef database update \
  --project EventResourceReservationApp.Infrastructure \
  --startup-project EventResourceReservationApp.Api
```

### 5. Ejecutar la API

```bash
dotnet run --project EventResourceReservationApp.Api
```

Con el perfil HTTPS, la aplicación se ejecuta normalmente en:

```text
https://localhost:7198
```

Swagger está disponible en Development en:

```text
https://localhost:7198/swagger
```

## Docker Compose

El repositorio incluye un `Dockerfile` para la API y un `docker-compose.yml` con los servicios de PostgreSQL y backend.

Para construir y levantar ambos contenedores:

```bash
docker compose up --build -d
```

La API queda expuesta por HTTP en el puerto:

```text
http://localhost:8080
```

> Si la base de datos aún no tiene el esquema, aplica las migraciones antes de utilizar la aplicación. Swagger solo se habilita actualmente cuando `ASPNETCORE_ENVIRONMENT=Development`.

## Pruebas

### Ejecutar pruebas unitarias

```bash
dotnet test EventResourceReservationApp.UnitTests/EventResourceReservationApp.UnitTests.csproj
```

La suite cubre entidades como recursos, reservas, ubicaciones, categorías y reseñas, además de diferentes casos de uso de Application.

### Integración

La infraestructura inicial del proyecto de integration tests está creada, pero las pruebas de integración significativas todavía forman parte del trabajo pendiente.

## Documentación técnica

Existe documentación adicional del proyecto en:

https://jose-angell.github.io/EventResourceReservationAppDocs/docs/backend/setup

## Estado y mejoras futuras

El backend cuenta con el flujo principal de recursos, reservas, autenticación, persistencia, logging y pruebas unitarias.

Las siguientes mejoras son candidatas naturales para una futura iteración:

- Implementar una suite real de pruebas de integración con PostgreSQL/Testcontainers.
- Reducir el acoplamiento del dominio con ASP.NET Core Identity.
- Mover dependencias de infraestructura fuera de Application cuando corresponda.
- Fortalecer el manejo de errores y Problem Details.
- Incorporar CI para ejecutar build y tests automáticamente.
- Revisar concurrencia y consistencia en escenarios de reservas simultáneas.

## Otros componentes

Este backend forma parte de una solución más amplia que también contempla:

- Frontend desarrollado con React.
- Documentación técnica publicada con Docusaurus.

## Autor

**José Ángel Gallardo Córdova**  
.NET Backend Developer

- LinkedIn: https://www.linkedin.com/in/joseangelgallardocordova
- GitHub: https://github.com/jose-angell
