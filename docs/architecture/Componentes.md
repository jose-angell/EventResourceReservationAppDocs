---
sidebar_position: 3
title: Componentes
---
## 3. Componentes del API (C4 – Nivel 3)

### 3.1 Descripción general  
En este nivel descomponemos el contenedor **API REST** en sus piezas internas: controllers, casos de uso (UseCases), repositorios e integraciones con servicios externos.

Cada componente:
- Tiene una responsabilidad clara.  
- Se ejecuta dentro del mismo proceso del API.  
- Se comunica a través de llamadas a métodos (in-process), no HTTP.

### 3.2 Módulos principales

| Módulo              | Controller                  | Use Cases (Application)                        | Infraestructura / Servicios externos          |
|---------------------|-----------------------------|------------------------------------------------|-----------------------------------------------|
| Autenticación       | `AuthController`            | `AuthenticateUser`, `RefreshToken`             | `IAuthService → AuthService`                  |
| Recursos            | `ResourcesController`       | `ListResources`, `GetResource`, `ManageResource` | `IResourceRepository → ResourceRepository`    |
| Reservas            | `ReservationsController`    | `BookResource`, `CancelReservation`, `ListUserReservations` | `IReservationRepository → ReservationRepository` |
| Pagos               | `PaymentsController`        | `ProcessPayment`, `RefundPayment`              | `IPaymentService → StripePaymentService`      |
| Sincronización      | `InventoryController`       | `SyncInventory`, `CheckAvailability`           | `IInventoryService → InventoryExternalService` |
| Salud y métricas    | `HealthController`, `MetricsController` | `RunHealthChecks`, `FetchMetrics` | `IHealthCheckService`, `IMetricsProducerService`|

---
## 3.2 Componentes del Contenedor Web SPA (React + Vite)

### 3.2.1 Módulos Principales

| Componente        | Tipo               | Responsabilidad                                    |
|-------------------|--------------------|----------------------------------------------------|
| Layout            | React Component    | Estructura global: header, footer y rutas          |
| NavBar            | React Component    | Navegación principal                               |
| ResourceList      | Feature Module     | Listado de recursos (sillas, mesas, luces)         |
| ResourceDetail    | Feature Module     | Vista de detalle de un recurso, disponibilidad     |
| ReservationForm   | Feature Module     | Formulario para reservar un recurso                |
| ApiService        | TS Class / Hook    | Métodos HTTP (fetch/Axios) para consumir la API    |
| useAuth           | Custom Hook        | Autenticación, persistencia de tokens              |
| StateStore        | Zustand/Redux      | Estado global: usuario, carrito de reserva         |
| SharedUI          | UI Library         | Botones, inputs, modales reutilizables             |

