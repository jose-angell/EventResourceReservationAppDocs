---
sidebar_position: 1
title: 01-overview
---

## 🚀 Feature: `Reservation`

> **Archivo:** `01-overview.md`

Permite que los `Manager` y `client` pueda gestionar las reservas dentro del sistema, cada reserva creada debe porder ser actulizado y eliminado o rechazada, segun sea requerido  por el negocio.

---

### 1. Resumen de la Feature

#### 1.2 Objetivos y Métricas de Éxito

* **Objetivo 1:**  Asegurar la creación y persistencia exitosa del 100% de las reservas intentados por los usuarios dentro del primer mes de uso.
    **Métrica de Seguimiento:** Tasa de éxito de operaciones POST /api/v1/Reservation
* **Objetivo 2:** Mantener la latencia de respuesta de todas las operaciones CRUD de las reservas por debajo de 50 ms en el 99% de las peticiones.
* **Métricas de Seguimiento:**
    * Porcentaje de peticiones a GET/POST/PUT/DELETE /api/v1/Reservation con tiempo de respuesta inferior a 50 ms.

#### 1.2. Contexto de Negocio

1.  **Flujo Actual (sin Reservaciones):** La creacion de las reservas no cuentan con una digitalizacion que permita su facil acceso y cosulta eficiente, lo cual genera retrasos en las respuestas a los usuarios.
2.  **Flujo Propuesto (con gestión de Reservaciones):** El gestor utiliza la nueva funcionalidad de Gestión de Reservas para visualizar las solicitudes del cliente. Permite a un organizador consultar las solicitudes de cada recursos en tiempo real, eliminando la necesidad de llamadas manuales y mejorando la eficiencia del equipo de inventario, lo que facilita la organización y búsqueda. El Cliente puede Crear, eliminar o editar cada reserva segun la necesidades de su evento y las disponibilidades del recurso.
3.  **Beneficios Clave:**
    * Mejora en la organización y administracion de reservas: Permite una estructura lógica para atender las solicitudes.
    * Agilización de la búsqueda y filtrado de reservas: Tanto para gestores como para futuros usuarios finales.
    * Reducción de errores en la asignación: Asegura que los recursos se mantengan actualizados en cada reservacion.

---

#### 1.3 Alcance (In-Scope / Out-of-Scope)
* **Incluye:** Implementación del endpoint REST del CRUD para el **Reservation**, lógica de negocio asociada a las reservas solicitadas, integración con la base de datos PostgreSQL.
* **No Incluye:** Cambios en la interfaz de usuario (responsabilidad del frontend y documentados en `docs/frontend/`).
---

### 2. Actores Involucrados

| Actor / Stakeholder          | Rol en la Feature / Interés                               |
| :--------------------------- | :-------------------------------------------------------- |
| Manager     | Maneja el seguimiento de la reservacion.               |
| Client     | Maneja la creación, edición y eliminación de las reservas.               |
| Equipo de Desarrollo Backend | Responsable de la implementación y mantenimiento de la API de las reseñas.      |

---

### 3. Historias de Usuario y Criterios de Aceptación

#### 3.1 Historia de Usuario

> “Como **Manager**, quiero **consultar las resevas disponibles por fecha** para **poder planificar eventos sin depender de comunicaciones por email**.”

---

#### 3.2 Criterios BDD (Given–When–Then)  

**Escenario de éxito - Consulta de las reservas disponibles**
* **Given** un gestor autenticado con el permiso `Reservation.read`
* **And** existen una lista de reservas disponibles para mostrar.
* **When** el gestor llama al endpoint `GET /api/v1/Reservation`
* **Then** el sistema responde con un `HTTP 200 OK`
* **And** el cuerpo de la respuesta contiene un array de objetos JSON con `{ id, RecursoId, etc... }` para los recrusos de interes del usuario.

**Escenario alternativo - Sin reservas disponibles**
* **Given** un gestor autenticado con el permiso `Reservation.read`
* **And** no existe una lista de reservas disponible para mostrar en el sistema
* **When** el gestor llama al endpoint `GET /api/v1/Reservation`
* **Then** el sistema responde con un `HTTP 200 OK`
* **And** el cuerpo de la respuesta contiene un array JSON vacío o un mensaje indicando que no hay recursos.

**Escenario de éxito - Crea una nueva reservas**
* **Given** un gestor autenticado con el permiso `Reservation.create`
* **And**  llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `POST /api/v1/Reservation`
* **Then** el sistema responde con un `HTTP 201 Created`
* **And** el cuerpo de la respuesta contiene un objeto JSON con `{ id, Fecha, etc... }` para los recrusos de interes del usuario.

**Escenario alternativo - Crecion inválida**
* **Given** un gestor autenticado
* **And**  No llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `POST /api/v1/Reservation`
* **Then** el sistema responde con un `HTTP 400 Bad Request`
* **And** el cuerpo de la respuesta es un error con `errorCode: "Validation"` y un mensaje descriptivo.

**Escenario de éxito - Editar una reservas**
* **Given** un gestor autenticado con el permiso `Reservation.update`
* **And**  llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `PUT /api/v1/Reservation/id`
* **Then** el sistema responde con un `HTTP 204 No Content`

**Escenario alternativo - Edicion inválida**
* **Given** un gestor autenticado
* **And**  No llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `PUT /api/v1/Reservation/id`
* **Then** el sistema responde con un `HTTP 400 Bad Request`
* **And** el cuerpo de la respuesta es un error con `errorCode: "Validation"` y un mensaje descriptivo.


**Escenario de éxito - Eliminar una reservas**
* **Given** un gestor autenticado con el permiso `Reservation.delete`
* **And**  selecciona un elemento de la lista.
* **When** el gestor llama al endpoint `DELETE /api/v1/Reservation/id`
* **Then** el sistema responde con un `HTTP 204 No Content`

**Escenario alternativo - Eliminacion inválida**
* **Given** un gestor autenticado
* **And**  No llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `DELETE /api/v1/Reservation/id`
* **Then** el sistema responde con un `HTTP 404  Not Found`
* **And** el cuerpo de la respuesta es un error con `errorCode: "Validation"` y un mensaje descriptivo.