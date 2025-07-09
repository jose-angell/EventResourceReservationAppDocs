---
sidebar_position: 3
title: Componentes
---
## 3. Componentes del API (C4 – Nivel 3)

### 3.1 Descripción General 
En este nivel de abstracción, desglosamos el contenedor principal de la API REST (tu aplicación ASP.NET Core) en sus **piezas internas clave o módulos principales**. Estos componentes, aunque lógicamente separados, se ejecutan dentro del mismo proceso y se comunican principalmente mediante llamadas a métodos (in-process).
Cada componente:
    - Tiene una **responsabilidad clara** y **específica**.
    - Contribuye a la funcionalidad general del API.
    - Interactúa con otros componentes a través de interfaces bien definidas.

### 3.2 Módulos Principales y Responsabilidades

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

