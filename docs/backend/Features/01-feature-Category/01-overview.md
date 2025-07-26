---
sidebar_position: 1
title: 01-overview
---

## 🚀 Feature: Gestión de Categorías

> **Archivo:** `01-overview.md`

---

### 1. Resumen de la Feature

Permite aun gestor crear categorias para organizar de forma mas eficiente los recursos disponibles en le sistema, ajilizando la busquedas y mejorando la experiencia del usuario.

---

#### 1.2 Objetivos y Métricas de Éxito

* **Objetivo 1:**  Asegurar la creación y persistencia exitosa del 100% de las categorías intentadas por los gestores dentro del primer mes de uso.
    **Métrica de Seguimiento:** Tasa de éxito de operaciones POST /api/v1/categories
* **Objetivo 2:** Mantener la latencia de respuesta de todas las operaciones CRUD de categorías por debajo de 50 ms en el 99% de las peticiones.
* **Métricas de Seguimiento:**
    * Porcentaje de peticiones a GET/POST/PUT/DELETE /api/v1/categories con tiempo de respuesta inferior a 50 ms.

---

#### 1.3. Contexto de Negocio

1.  **Flujo Actual (sin categorías):** Los recursos se gestionan como una lista plana sin una clasificación formal. Los gestores deben recordar o buscar manualmente la "naturaleza" o el "tipo" de cada recurso, lo que puede llevar a inconsistencias o dificultades para encontrar elementos específicos.
2.  **Flujo Propuesto (con gestión de categorías):** El gestor utiliza la nueva funcionalidad de Gestión de Categorías para crear, editar y eliminar etiquetas de clasificación (ej., "Mobiliario", "Iluminación", "Audio y Video"). Estas categorías se pueden asignar posteriormente a los recursos. Al consultar recursos, se podrán filtrar eficientemente por estas categorías, lo que facilita la organización y búsqueda.
3.  **Beneficios Clave:**
    * Mejora en la organización y clasificacíon de recursos: Permite una estructura lógica para el inventario.
    * Agilización de la búsqueda y filtrado de recursos: Tanto para gestores como para futuros usuarios finales.
    * Reducción de errores en la asignación: Asegura que los recursos se agrupen de manera consistente.
    * Base para reportes y análisis: Facilita la generación de informes basados en tipos de recursos.

---

#### 1.4 Alcance (In-Scope / Out-of-Scope)

* **Incluye:** Implementación del endpoint REST del CRUD para **Categorías**, lógica de negocio asociada a categorías, integración con la base de datos PostgreSQL, y una capa de caché en memoria de 30 segundos para las consultas de categorías.
* **No Incluye:** Cambios en la interfaz de usuario (responsabilidad del frontend y documentados en `docs/frontend/`).

---

### 2. Actores Involucrados

| Actor / Stakeholder          | Rol en la Feature / Interés                               |
| :--------------------------- | :-------------------------------------------------------- |
| Gestor     | Maneja la creación, edición y eliminación de las categorías de recursos.               |
| Equipo de Desarrollo Backend | Responsable de la implementación y mantenimiento de la API de categorías.      |

---

### 3. Historias de Usuario y Criterios de Aceptación

#### 3.1 Historia de Usuario

> “Como **Gestor**, quiero **gestionar las categorias disponibles para cada recurso** para **poder organizar la infromacion de forma eficiente al consultar los recursos**.”

---

#### 3.2 Criterios BDD (Given–When–Then)  
**Definición:** Escenarios detallados que describen el **comportamiento esperado** de la feature de forma estructurada. Sirven como base directa para la implementación y las pruebas automatizadas.

* **Given** (Dado que): Describe las precondiciones o el contexto inicial del escenario.
* **When** (Cuando): Describe la acción o el evento que desencadena la lógica de la feature.
* **Then** (Entonces): Describe el resultado esperado o el cambio observable en el sistema.

Se pueden añadir escenarios alternativos para cubrir casos de borde, errores o flujos no ideales.

**Escenario de éxito - Consulta de categorias disponibles**
* **Given** un gestor autenticado con el permiso `category.read`
* **And** existen categorias disponibles para mostrar.
* **When** el gestor llama al endpoint `GET /api/v1/categories`
* **Then** el sistema responde con un `HTTP 200 OK`
* **And** el cuerpo de la respuesta contiene un array de objetos JSON con `{ id, name, Description, isAvailable }` para las categorias disponibles.

**Escenario alternativo - Sin categorias disponibles**
* **Given** un gestor autenticado con el permiso `category.read`
* **And** no existen categorias disponibles para mostrar en el sistema
* **When** el gestor llama al endpoint `GET /api/v1/categories`
* **Then** el sistema responde con un `HTTP 200 OK`
* **And** el cuerpo de la respuesta contiene un array JSON vacío o un mensaje indicando que no hay recursos.

**Escenario de éxito - Crea una nueva categoria**
* **Given** un gestor autenticado con el permiso `category.create`
* **And**  llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `POST /api/v1/categories`
* **Then** el sistema responde con un `HTTP 201 Created`
* **And** el cuerpo de la respuesta contiene un objeto JSON con `{ id, name, Description, isAvailable }` para las categoria creacada.

**Escenario alternativo - Crecion inválida**
* **Given** un gestor autenticado
* **And**  No llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `POST /api/v1/categories`
* **Then** el sistema responde con un `HTTP 400 Bad Request`
* **And** el cuerpo de la respuesta es un error con `errorCode: "Validation"` y un mensaje descriptivo.

**Escenario de éxito - Editar un categoria**
* **Given** un gestor autenticado con el permiso `category.update`
* **And**  llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `PUT /api/v1/categories/id`
* **Then** el sistema responde con un `HTTP 204 No Content`

**Escenario alternativo - Edicion inválida**
* **Given** un gestor autenticado
* **And**  No llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `PUT /api/v1/categories/id`
* **Then** el sistema responde con un `HTTP 400 Bad Request`
* **And** el cuerpo de la respuesta es un error con `errorCode: "Validation"` y un mensaje descriptivo.


**Escenario de éxito - Eliminar un categoria**
* **Given** un gestor autenticado con el permiso `category.delete`
* **And**  selecciona una categoria.
* **When** el gestor llama al endpoint `DELETE /api/v1/categories/id`
* **Then** el sistema responde con un `HTTP 204 No Content`

**Escenario alternativo - Eliminacion inválida**
* **Given** un gestor autenticado
* **And**  No llena todos los campos requeridos del formulario.
* **When** el gestor llama al endpoint `DELETE /api/v1/categories/id`
* **Then** el sistema responde con un `HTTP 404  Not Found`
* **And** el cuerpo de la respuesta es un error con `errorCode: "Validation"` y un mensaje descriptivo.