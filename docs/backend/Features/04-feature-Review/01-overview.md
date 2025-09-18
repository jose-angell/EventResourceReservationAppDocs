---
sidebar_position: 1
title: 01-overview
---

## 🚀 Feature: Gestión de Reseñas

> **Archivo:** `01-overview.md`

---

### 1. Resumen de la Feature

Permite que un cliente peuda crear un reseña que refleje la experiencia que tuvo al reservar con nosotros para su evento, esta opinion puede ser para cualquier recurso que la persona reservara y sirviendo tambien para que otras personas puedan usar como referencia.

---

#### 1.2 Objetivos y Métricas de Éxito

* **Objetivo 1:**  Asegurar la creación y persistencia exitosa del 100% de las reseñas intentadas por los usuarios dentro del primer mes de uso.
    **Métrica de Seguimiento:** Tasa de éxito de operaciones POST /api/v1/Reviews
* **Objetivo 2:** Mantener la latencia de respuesta de todas las operaciones CRUD de las reseñas por debajo de 50 ms en el 99% de las peticiones.
* **Métricas de Seguimiento:**
    * Porcentaje de peticiones a GET/POST/PUT/DELETE /api/v1/Reviews con tiempo de respuesta inferior a 50 ms.

---

#### 1.3. Contexto de Negocio

1.  **Flujo Actual (sin reseñas):** Actualmente no existe una forma de que los usuarios puedan compartir su experiencia al reservar con nosotros, los clientes solo pueden pedir opiniones en foros externos

2.  **Flujo Propuesto (con gestión de reseñas):** El cliente utiliza la nueva funcionalidad de Gestión de reseñas para crear, editar y eliminar las opiniones para cada recurso que esta persona reservara en algun momento.

3.  **Beneficios Clave:**
    * Mejora en la confianza de los clientes: Permite que los usuarios puedan compartir su experiencia con otros y generar mas confianza en futuros consumidores.



---

#### 1.4 Alcance (In-Scope / Out-of-Scope)

* **Incluye:** Implementación del endpoint REST del CRUD para el **Reseñas**, lógica de negocio asociada a la reseñas de los usuarios, integración con la base de datos PostgreSQL.
* **No Incluye:** Cambios en la interfaz de usuario (responsabilidad del frontend y documentados en `docs/frontend/`).

---

### 2. Actores Involucrados

| Actor / Stakeholder          | Rol en la Feature / Interés                               |
| :--------------------------- | :-------------------------------------------------------- |
| Gestor     | Maneja la creación, edición y eliminación de las reseñas de los recursos.               |
| Equipo de Desarrollo Backend | Responsable de la implementación y mantenimiento de la API de las reseñas.      |

---

### 3. Historias de Usuario y Criterios de Aceptación

#### 3.1 Historia de Usuario

> “Como **Gestor**, quiero **gestionar la reseñas cada recurso del evento** para **poder compartir las diferentes experiencias que forman parte del evento**.”

---

#### 3.2 Criterios BDD (Given–When–Then)  

**Escenario de éxito - Consulta de las reseñas disponibles**
* **Given** un gestor autenticado con el permiso `Review.read`
* **And** existen una lista de reseñas disponibles para mostrar.
* **When** el gestor llama al endpoint `GET /api/v1/Reviews`
* **Then** el sistema responde con un `HTTP 200 OK`
* **And** el cuerpo de la respuesta contiene un array de objetos JSON con `{ id, RecursoId, Comment, etc... }` para los opiniones de interes del usuario.

**Escenario alternativo - Sin reseñas disponibles**
* **Given** un gestor autenticado con el permiso `Review.read`
* **And** no existe una lista de reseñas disponible para mostrar en el sistema
* **When** el gestor llama al endpoint `GET /api/v1/Reviews`
* **Then** el sistema responde con un `HTTP 200 OK`
* **And** el cuerpo de la respuesta contiene un array JSON vacío o un mensaje indicando que no hay reseñas.

**Escenario de éxito - Crea una nueva reseña**
* **Given** un gestor autenticado con el permiso `Review.create`
* **And**  llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `POST /api/v1/Reviews`
* **Then** el sistema responde con un `HTTP 201 Created`
* **And** el cuerpo de la respuesta contiene un objeto JSON con `{ id, Fecha, RecursoId, Comment, etc... }` para los opiniones de interes del usuario.

**Escenario alternativo - Crecion inválida**
* **Given** un gestor autenticado
* **And**  No llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `POST /api/v1/Reviews`
* **Then** el sistema responde con un `HTTP 400 Bad Request`
* **And** el cuerpo de la respuesta es un error con `errorCode: "Validation"` y un mensaje descriptivo.

**Escenario de éxito - Editar una reseña**
* **Given** un gestor autenticado con el permiso `Review.update`
* **And**  llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `PUT /api/v1/Reviews/id`
* **Then** el sistema responde con un `HTTP 204 No Content`

**Escenario alternativo - Edicion inválida**
* **Given** un gestor autenticado
* **And**  No llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `PUT /api/v1/Review/id`
* **Then** el sistema responde con un `HTTP 400 Bad Request`
* **And** el cuerpo de la respuesta es un error con `errorCode: "Validation"` y un mensaje descriptivo.


**Escenario de éxito - Eliminar una reseeña**
* **Given** un gestor autenticado con el permiso `Review.delete`
* **And**  selecciona un elemento de la lista.
* **When** el gestor llama al endpoint `DELETE /api/v1/Reviews/id`
* **Then** el sistema responde con un `HTTP 204 No Content`

**Escenario alternativo - Eliminacion inválida**
* **Given** un gestor autenticado
* **And**  No llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `DELETE /api/v1/Review/id`
* **Then** el sistema responde con un `HTTP 404  Not Found`
* **And** el cuerpo de la respuesta es un error con `errorCode: "Validation"` y un mensaje descriptivo.