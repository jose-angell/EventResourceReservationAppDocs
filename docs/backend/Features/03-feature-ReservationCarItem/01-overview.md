---
sidebar_position: 1
title: 01-overview
---

## 🚀 Feature: Gestión de Carrito de reservas

> **Archivo:** `01-overview.md`

---

### 1. Resumen de la Feature

Permite que un usuario pueda crear una lista de recursos que se pretenden reservar, junto con un resumen de los detalles que necesitan de esos recursos, esta lista estara sujeta a disponibilidad del recurso.

---

#### 1.2 Objetivos y Métricas de Éxito

* **Objetivo 1:**  Asegurar la creación y persistencia exitosa del 100% de carrito de reservas intentadas por los usuarios dentro del primer mes de uso.
    **Métrica de Seguimiento:** Tasa de éxito de operaciones POST /api/v1/ReservationCarItems
* **Objetivo 2:** Mantener la latencia de respuesta de todas las operaciones CRUD del carrito de reservas por debajo de 50 ms en el 99% de las peticiones.
* **Métricas de Seguimiento:**
    * Porcentaje de peticiones a GET/POST/PUT/DELETE /api/v1/ReservationCarItems con tiempo de respuesta inferior a 50 ms.

---

#### 1.3. Contexto de Negocio

1.  **Flujo Actual (sin carrito de reservas):** Los recursos a reservar solo se pueden solicitar de forma individual, sin importar que el evento sea el mismo. Lo cual genera retrasos en las respuestas a los usuarios y entorpecen la experiencia del usuario.

2.  **Flujo Propuesto (con gestión de carrito de reservas):** El gestor utiliza la nueva funcionalidad de Gestión de carrito de reservas para crear, editar y eliminar la lista de los recursos que ser reservaran, teniendo en cuenta la disponibilidad del recurso al estar dentro de esta lista.

3.  **Beneficios Clave:**
    * Mejora en la organización y reservacion de recursos: Permite una estructura lógica para la reserva de diferentes recursos para un mismo evento.
    * Agilización de la búsqueda y filtrado de recursos de interes: Tanto para gestores como para futuros usuarios finales, les facilita la organizacion de un evento y el cambio de un recurso por otro.


---

#### 1.4 Alcance (In-Scope / Out-of-Scope)

* **Incluye:** Implementación del endpoint REST del CRUD para el **carrito de reservas**, lógica de negocio asociada a la lista de recursos de interes, integración con la base de datos PostgreSQL.
* **No Incluye:** Cambios en la interfaz de usuario (responsabilidad del frontend y documentados en `docs/frontend/`).

---

### 2. Actores Involucrados

| Actor / Stakeholder          | Rol en la Feature / Interés                               |
| :--------------------------- | :-------------------------------------------------------- |
| Gestor     | Maneja la creación, edición y eliminación del carrito de reservas de los recursos.               |
| Equipo de Desarrollo Backend | Responsable de la implementación y mantenimiento de la API del carrito de reservas.      |

---

### 3. Historias de Usuario y Criterios de Aceptación

#### 3.1 Historia de Usuario

> “Como **Gestor**, quiero **gestionar la lista de cada recurso del evento** para **poder organizar los diferentes recursos que forman parte del mismo evento**.”

---

#### 3.2 Criterios BDD (Given–When–Then)  

**Escenario de éxito - Consulta de carrito de reservas disponibles**
* **Given** un gestor autenticado con el permiso `ReservationCarItem.read`
* **And** existen una lista de recursos disponibles para mostrar.
* **When** el gestor llama al endpoint `GET /api/v1/ReservationCarItems`
* **Then** el sistema responde con un `HTTP 200 OK`
* **And** el cuerpo de la respuesta contiene un array de objetos JSON con `{ id, Fecha, RecursoId, Cantidad, etc... }` para los recursos de interes del usuario.

**Escenario alternativo - Sin carrito de reservas disponibles**
* **Given** un gestor autenticado con el permiso `ReservationCarItem.read`
* **And** no existe una lista de recursos disponible para mostrar en el sistema
* **When** el gestor llama al endpoint `GET /api/v1/ReservationCarItems`
* **Then** el sistema responde con un `HTTP 200 OK`
* **And** el cuerpo de la respuesta contiene un array JSON vacío o un mensaje indicando que no hay recursos.

**Escenario de éxito - Crea un nuevo elmento a lista de recursos**
* **Given** un gestor autenticado con el permiso `ReservationCarItem.create`
* **And**  llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `POST /api/v1/ReservationCarItems`
* **Then** el sistema responde con un `HTTP 201 Created`
* **And** el cuerpo de la respuesta contiene un objeto JSON con `{ id, Fecha, RecursoId, Cantidad, etc... }` para los recursos de interes del usuario.

**Escenario alternativo - Crecion inválida**
* **Given** un gestor autenticado
* **And**  No llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `POST /api/v1/ReservationCarItems`
* **Then** el sistema responde con un `HTTP 400 Bad Request`
* **And** el cuerpo de la respuesta es un error con `errorCode: "Validation"` y un mensaje descriptivo.

**Escenario de éxito - Editar un elemento de la lista de recursos**
* **Given** un gestor autenticado con el permiso `ReservationCarItem.update`
* **And**  llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `PUT /api/v1/ReservationCarItems/id`
* **Then** el sistema responde con un `HTTP 204 No Content`

**Escenario alternativo - Edicion inválida**
* **Given** un gestor autenticado
* **And**  No llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `PUT /api/v1/ReservationCarItems/id`
* **Then** el sistema responde con un `HTTP 400 Bad Request`
* **And** el cuerpo de la respuesta es un error con `errorCode: "Validation"` y un mensaje descriptivo.


**Escenario de éxito - Eliminar un elemento de la lista**
* **Given** un gestor autenticado con el permiso `ReservationCarItem.delete`
* **And**  selecciona un elemento de la lista.
* **When** el gestor llama al endpoint `DELETE /api/v1/ReservationCarItema/id`
* **Then** el sistema responde con un `HTTP 204 No Content`

**Escenario alternativo - Eliminacion inválida**
* **Given** un gestor autenticado
* **And**  No llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `DELETE /api/v1/ReservationCarItems/id`
* **Then** el sistema responde con un `HTTP 404  Not Found`
* **And** el cuerpo de la respuesta es un error con `errorCode: "Validation"` y un mensaje descriptivo.