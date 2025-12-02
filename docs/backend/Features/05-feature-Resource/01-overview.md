---
sidebar_position: 1
title: 01-overview
---

## 🚀 Feature: `Recurso`

> **Archivo:** `01-overview.md`

Permite que un `Manager` pueda gestionar los recursos disponibles dentro del sistema, cada recurso creado debe porder ser actulizado y eliminado o bloqueado, segun sea requerido  por el negocio.

---

### 1. Resumen de la Feature

#### 1.2 Objetivos y Métricas de Éxito

* **Objetivo 1:**  Asegurar la creación y persistencia exitosa del 100% de los recursos intentados por los usuarios dentro del primer mes de uso.
    **Métrica de Seguimiento:** Tasa de éxito de operaciones POST /api/v1/Resource
* **Objetivo 2:** Mantener la latencia de respuesta de todas las operaciones CRUD de los recursos por debajo de 50 ms en el 99% de las peticiones.
* **Métricas de Seguimiento:**
    * Porcentaje de peticiones a GET/POST/PUT/DELETE /api/v1/Resource con tiempo de respuesta inferior a 50 ms.

#### 1.2. Contexto de Negocio

1.  **Flujo Actual (sin Recursos):** Las ubicaciones de los recursos no cuentan con una digitalizacion que permita su facil acceso y cosultaeficiente, lo cual genera retrasos en las respuestas a los usuarios. de igual forma si un cliente frecuente tiene varias reservas tendra que volver a ingresar el nombre del recurso manualmente.
2.  **Flujo Propuesto (con gestión de Recursos):** El gestor utiliza la nueva funcionalidad de Gestión de Recursos para crear, editar y eliminar. Permite a un organizador consultar la disponibilidad de recursos en tiempo real, eliminando la necesidad de llamadas manuales y mejorando la eficiencia del equipo de inventario, lo que facilita la organización y búsqueda.
3.  **Beneficios Clave:**
    * Mejora en la organización y administracion de recursos: Permite una estructura lógica para el inventario.
    * Agilización de la búsqueda y filtrado de recursos: Tanto para gestores como para futuros usuarios finales.
    * Reducción de errores en la asignación: Asegura que los recursos se mantengan actualizados.

---

#### 1.3 Alcance (In-Scope / Out-of-Scope)
* **Incluye:** Implementación del endpoint REST del CRUD para el **Recursos**, lógica de negocio asociada a los recursos disponibles, integración con la base de datos PostgreSQL.
* **No Incluye:** Cambios en la interfaz de usuario (responsabilidad del frontend y documentados en `docs/frontend/`).
---

### 2. Actores Involucrados
<!-- Lista quiénes participan en este proceso -->
#### Definición  
**Definición:** Un **actor** es un usuario o sistema externo que interactúa directamente con la feature. Los **stakeholders** son roles o equipos interesados en el correcto funcionamiento y éxito de la misma.

| Actor / Stakeholder          | Rol en la Feature / Interés                               |
| :--------------------------- | :-------------------------------------------------------- |
| Manager     | Maneja la creación, edición y eliminación de los recursos.               |
| Equipo de Desarrollo Backend | Responsable de la implementación y mantenimiento de la API de las reseñas.      |

---

### 3. Historias de Usuario y Criterios de Aceptación

#### 3.1 Historia de Usuario

**Definición:** Una frase concisa que describe la necesidad desde la **perspectiva del usuario**, enfocándose en el valor.
* **Quién** (rol/actor).
* **Qué** (la capacidad o acción que se desea).
* **Para qué** (el valor o beneficio obtenido).

**Formato:**
> “Como `<Actor>`, quiero `<acción>` para `<beneficio>`.”

**Ejemplo:**
> “Como **Organizador**, quiero **consultar los recursos disponibles por fecha** para **poder planificar eventos sin depender de comunicaciones por email**.”

---

#### 3.2 Criterios BDD (Given–When–Then)  

**Escenario de éxito - Consulta de los recursos disponibles**
* **Given** un gestor autenticado con el permiso `Resource.read`
* **And** existen una lista de recursos disponibles para mostrar.
* **When** el gestor llama al endpoint `GET /api/v1/Resources`
* **Then** el sistema responde con un `HTTP 200 OK`
* **And** el cuerpo de la respuesta contiene un array de objetos JSON con `{ id, RecursoId, etc... }` para los recrusos de interes del usuario.

**Escenario alternativo - Sin recursos disponibles**
* **Given** un gestor autenticado con el permiso `Resource.read`
* **And** no existe una lista de recursos disponible para mostrar en el sistema
* **When** el gestor llama al endpoint `GET /api/v1/Resources`
* **Then** el sistema responde con un `HTTP 200 OK`
* **And** el cuerpo de la respuesta contiene un array JSON vacío o un mensaje indicando que no hay recursos.

**Escenario de éxito - Crea un nuevo recurso**
* **Given** un gestor autenticado con el permiso `Resource.create`
* **And**  llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `POST /api/v1/Resources`
* **Then** el sistema responde con un `HTTP 201 Created`
* **And** el cuerpo de la respuesta contiene un objeto JSON con `{ id, Fecha, RecursoId, etc... }` para los recrusos de interes del usuario.

**Escenario alternativo - Crecion inválida**
* **Given** un gestor autenticado
* **And**  No llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `POST /api/v1/Resources`
* **Then** el sistema responde con un `HTTP 400 Bad Request`
* **And** el cuerpo de la respuesta es un error con `errorCode: "Validation"` y un mensaje descriptivo.

**Escenario de éxito - Editar un recurso**
* **Given** un gestor autenticado con el permiso `Resource.update`
* **And**  llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `PUT /api/v1/Reviews/id`
* **Then** el sistema responde con un `HTTP 204 No Content`

**Escenario alternativo - Edicion inválida**
* **Given** un gestor autenticado
* **And**  No llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `PUT /api/v1/Resources/id`
* **Then** el sistema responde con un `HTTP 400 Bad Request`
* **And** el cuerpo de la respuesta es un error con `errorCode: "Validation"` y un mensaje descriptivo.


**Escenario de éxito - Eliminar un recurso**
* **Given** un gestor autenticado con el permiso `Resource.delete`
* **And**  selecciona un elemento de la lista.
* **When** el gestor llama al endpoint `DELETE /api/v1/Resources/id`
* **Then** el sistema responde con un `HTTP 204 No Content`

**Escenario alternativo - Eliminacion inválida**
* **Given** un gestor autenticado
* **And**  No llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `DELETE /api/v1/Resources/id`
* **Then** el sistema responde con un `HTTP 404  Not Found`
* **And** el cuerpo de la respuesta es un error con `errorCode: "Validation"` y un mensaje descriptivo.