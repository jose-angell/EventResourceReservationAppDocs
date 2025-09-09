---
sidebar_position: 1
title: 01-overview
---

## 🚀 Feature: Gestión de Ubicaciones

> **Archivo:** `01-overview.md`

---

### 1. Resumen de la Feature

Permite a un usuario crear ubicaciones para conocer por un lado el destino de los recursos reservados por el cliente y tambien la ubicación donde se almacenan los recursos disponibles

---

#### 1.2 Objetivos y Métricas de Éxito

* **Objetivo 1:**  Asegurar la creación y persistencia exitosa del 100% de las ubicaciones intentadas por los usuarios dentro del primer mes de uso.
    **Métrica de Seguimiento:** Tasa de éxito de operaciones POST /api/v1/locations
* **Objetivo 2:** Mantener la latencia de respuesta de todas las operaciones CRUD de ubicaciones por debajo de 50 ms en el 99% de las peticiones.
* **Métricas de Seguimiento:**
    * Porcentaje de peticiones a GET/POST/PUT/DELETE /api/v1/locations con tiempo de respuesta inferior a 50 ms.

---

#### 1.3. Contexto de Negocio

1.  **Flujo Actual (sin ubicaciones):** Las ubicaciones de los recursos no cuentan con una digitalizacion que permita su facil acceso y cosultaeficiente, lo cual genera retrasos en las respuestas a los usuarios. de igual forma si un cliente frecuente tiene varias reservas tendra que volver a ingresar su dicreccion manualmente.

2.  **Flujo Propuesto (con gestión de ubicaciones):** El gestor utiliza la nueva funcionalidad de Gestión de ubicaciones para crear, editar y eliminar ubicaciones de los recursos que ser reservaran, y de igualmodo los usuarios pueden almacenar las ubicaciones en donde seran sus eventos. Estas ubicaiones se pueden asignar posteriormente a los recursos. Al consultar recursos, se podrán filtrar eficientemente por estas ubicaciones, lo que facilita la organización y búsqueda.
3.  **Beneficios Clave:**
    * Mejora en la organización y clasificacíon de recursos: Permite una estructura lógica para el inventario en la disponibilidad en la zona.
    * Agilización de la búsqueda y filtrado de recursos: Tanto para gestores como para futuros usuarios finales.


---

#### 1.4 Alcance (In-Scope / Out-of-Scope)

* **Incluye:** Implementación del endpoint REST del CRUD para **Ubicaciones**, lógica de negocio asociada a ubicaciones, integración con la base de datos PostgreSQL, y una capa de caché en memoria de 30 segundos para las consultas de ubicaciones.
* **No Incluye:** Cambios en la interfaz de usuario (responsabilidad del frontend y documentados en `docs/frontend/`).

---

### 2. Actores Involucrados

| Actor / Stakeholder          | Rol en la Feature / Interés                               |
| :--------------------------- | :-------------------------------------------------------- |
| Gestor     | Maneja la creación, edición y eliminación de las ubicaciones de recursos.               |
| Equipo de Desarrollo Backend | Responsable de la implementación y mantenimiento de la API de ubicaciones.      |

---

### 3. Historias de Usuario y Criterios de Aceptación

#### 3.1 Historia de Usuario

> “Como **Gestor**, quiero **gestionar las ubicaciones de cada recurso o evento** para **poder organizar la logistica de forma eficiente al consultar los recursos o agendar un evento**.”

---

#### 3.2 Criterios BDD (Given–When–Then)  

**Escenario de éxito - Consulta de ubicaciones disponibles**
* **Given** un gestor autenticado con el permiso `location.read`
* **And** existen ubicaciones disponibles para mostrar.
* **When** el gestor llama al endpoint `GET /api/v1/locations`
* **Then** el sistema responde con un `HTTP 200 OK`
* **And** el cuerpo de la respuesta contiene un array de objetos JSON con `{ id, City, Country, street, etc... }` para las ubicaciones disponibles.

**Escenario alternativo - Sin ubicaciones disponibles**
* **Given** un gestor autenticado con el permiso `location.read`
* **And** no existen ubicaciones disponibles para mostrar en el sistema
* **When** el gestor llama al endpoint `GET /api/v1/locations`
* **Then** el sistema responde con un `HTTP 200 OK`
* **And** el cuerpo de la respuesta contiene un array JSON vacío o un mensaje indicando que no hay recursos.

**Escenario de éxito - Crea una nueva ubicación**
* **Given** un gestor autenticado con el permiso `location.create`
* **And**  llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `POST /api/v1/locations`
* **Then** el sistema responde con un `HTTP 201 Created`
* **And** el cuerpo de la respuesta contiene un objeto JSON con `{ id, City, Country, street, etc... }` para la ubicacion creacada.

**Escenario alternativo - Crecion inválida**
* **Given** un gestor autenticado
* **And**  No llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `POST /api/v1/locations`
* **Then** el sistema responde con un `HTTP 400 Bad Request`
* **And** el cuerpo de la respuesta es un error con `errorCode: "Validation"` y un mensaje descriptivo.

**Escenario de éxito - Editar un ubicación**
* **Given** un gestor autenticado con el permiso `location.update`
* **And**  llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `PUT /api/v1/locations/id`
* **Then** el sistema responde con un `HTTP 204 No Content`

**Escenario alternativo - Edicion inválida**
* **Given** un gestor autenticado
* **And**  No llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `PUT /api/v1/locations/id`
* **Then** el sistema responde con un `HTTP 400 Bad Request`
* **And** el cuerpo de la respuesta es un error con `errorCode: "Validation"` y un mensaje descriptivo.


**Escenario de éxito - Eliminar un ubicaion**
* **Given** un gestor autenticado con el permiso `location.delete`
* **And**  selecciona una ubicacion.
* **When** el gestor llama al endpoint `DELETE /api/v1/location/id`
* **Then** el sistema responde con un `HTTP 204 No Content`

**Escenario alternativo - Eliminacion inválida**
* **Given** un gestor autenticado
* **And**  No llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `DELETE /api/v1/locations/id`
* **Then** el sistema responde con un `HTTP 404  Not Found`
* **And** el cuerpo de la respuesta es un error con `errorCode: "Validation"` y un mensaje descriptivo.