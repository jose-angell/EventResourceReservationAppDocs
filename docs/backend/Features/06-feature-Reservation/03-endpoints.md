---
sidebar_position: 3
title: 03-endpoints
---

# 🔌 Contrato API

> Archivo: `03-endpoints.md`

Este documento detalla los **endpoints** expuestos por el backend para la feature `Reservation`. Incluye el método HTTP, la ruta, una descripción, así como ejemplos completos de solicitudes (requests) y respuestas (responses) para cada operación, incluyendo posibles códigos de error.

---

## 1. Resumen de Endpoints
Esta tabla proporciona una vista rápida de todos los endpoints relacionados con esta feature.

| Método HTTP | Ruta de la API                     | Descripción Breve                                        |
| :---------- | :--------------------------------- | :------------------------------------------------------- |
| `GET`       | `/api/v1/reservations`                | Obtiene una lista de todos las reservas disponibles.     |
| `GET`       | `/api/v1/reservations/{id}`           | Obtiene los detalles de una reserva específica por su ID. |
| `POST`      | `/api/v1/reservations`                | Crea una nuevo reserva para un recurso.                  |
| `PUT`       | `/api/v1/reservations/{id}`           | Actualiza completamente una reserva existente.           |
| `PUT`       | `/api/v1/reservations/status/{id}`           | Actualiza el estatus de la reserva existente.           |
| `PUT`       | `/api/v1/reservations/transation/{id}`           | Actualiza los datos de la transaccion de la reserva.           |
| `DELETE`    | `/api/v1/reservations/{id}`           | Elimina una reserva específica por su ID.                |

---
## 2. Detalle de Endpoints (Request & Response)

Para cada endpoint, se especifican los detalles de la solicitud, los parámetros, el cuerpo de la respuesta esperada y los posibles códigos de error.


### Endpoint: `GET /api/v1/reservations`
* **Descripción:** Permite obtener un listado de todas las reservaciones disponibles para mostrar al usuario.
* **Autenticación:** Requiere token JWT válido. Permiso: `reservations.read`.
* **Parámetros de Consulta (Query Parameters):**
    * `StartTime` (opcional, DateTime): Filtra las reservas disponibles por rango de fecha.
    * `EndTime` (opcional, DateTime): Filtra las reservas disponibles por rango de fecha.
    * `LocationId` (opcional, int): Filtra las reservas disponibles por ubicacion.
    * `StatusId` (opcional, int): Filtra las reservas disponibles por estatus.
    * `CreatedByUserIdFilter` (opcional, Guid): Filtra por usuario de creacion.
    * `OrderBy` (opcional, string): ordena reservas ascendete por fecha de creacion (ej. `"CreatedAt_desc"`, `"CreatedAt_asc"`).
* **Request (Ejemplo):**

```
GET /api/v1/reservations
```

* **Response (HTTP 200 Ok - Ejemplo de éxito):**
    ```json
    [
      {
        "Id" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "ClientId" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "StartTime" : "2025-08-23",
        "EndTime" : "2025-08-23",
        "ClientName" : "jose",
        "ClientPhoneNumber" : "223232113",
        "StatusId" : 1,
        "StatusDescription" : "Disponible",
        "TotalAmount" : 230.99,
        "Quantity" : 109,
        "LocationId" : 4,
        "LocationDescription" : "colonia del bosque",
        "Created" : "2025-08-23"
      },
      {
        "Id" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "ClientId" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "StartTime" : "2025-08-23",
        "EndTime" : "2025-08-23",
        "ClientName" : "jose",
        "ClientPhoneNumber" : "223232113",
        "StatusId" : 1,
        "StatusDescription" : "Disponible",
        "TotalAmount" : 230.99,
        "Quantity" : 109,
        "LocationId" : 4,
        "LocationDescription" : "colonia del bosque",
        "Created" : "2025-08-23"
      }
    ]
    ```
* **Códigos de Error Posibles:**
    * `500 Internal Server Error"`: Si la operacion falla.
        ```json
        {
          "Status": 500,
          "Title": "Error inesperado al consultar las reservas.",
          "Detail": "Ocurrió un error inesperado en el servidor."
        }
        ```
---        
### Endpoint: `GET /api/v1/reservations/id`
* **Descripción:** Permite obtener una reserva utilizando su Id.
* **Autenticación:** Requiere token JWT válido. Permiso: `reservations.read`.
* **Parámetros de Consulta (Route Parameters):**
    * `id` (obligatiro, int): Filtra reserva disponible por identificacion unico.
   
* **Request (Ejemplo):**

```
GET /api/v1/reservations/1
```

* **Response (HTTP 200 Ok - Ejemplo de éxito):**
    ```json
      {
        "Id" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "ClientId" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "StartTime" : "2025-08-23",
        "EndTime" : "2025-08-23",
        "ClientName" : "jose",
        "ClientPhoneNumber" : "223232113",
        "StatusId" : 1,
        "StatusDescription" : "Disponible",
        "TotalAmount" : 230.99,
        "Quantity" : 109,
        "LocationId" : 4,
        "LocationDescription" : "colonia del bosque",
        "Created" : "2025-08-23"
      }
    ```
* **Códigos de Error Posibles:**
    * `404 Not Found`: Si el `id`de la reserva no existe.
        ```json
        {
          "Status": 404,
          "Title": "Recurso no encontrado",
          "Detail": "La operación de consulta falló porque la reserva no existe."
        }
        ```
    * `500 Internal Server Error"`: Si la operacion falla.
        ```json
        {
          "Status": 500,
          "Title": "Error inesperado al consultar la reserva.",
          "Detail": "Ocurrió un error inesperado en el servidor."
        }
        ```
---
### Endpoint: `POST /api/v1/reservations`

* **Descripción:** Permite crear un nuevo resevacion.
* **Autenticación:** Requiere token JWT válido. Permiso: `reservations.create`.
* **Request Body (JSON - Ejemplo):**
    ```json
    {
        "Id" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "ClientId" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "StartTime" : "2025-08-23",
        "EndTime" : "2025-08-23",
        "ClientName" : "jose",
        "ClientPhoneNumber" : "223232113",
        "StatusId" : 1,
        "TotalAmount" : 230.99,
        "Quantity" : 109,
        "LocationId" : 4,
    }
    ```
* **Response (HTTP 201 Created - Ejemplo de éxito):**
    ```json
    {
        "Id" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "ClientId" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "StartTime" : "2025-08-23",
        "EndTime" : "2025-08-23",
        "ClientName" : "jose",
        "ClientPhoneNumber" : "223232113",
        "StatusId" : 1,
        "StatusDescription" : "Disponible",
        "TotalAmount" : 230.99,
        "Quantity" : 109,
        "LocationId" : 4,
        "LocationDescription" : "colonia del bosque",
        "Created" : "2025-08-23"
    }
    ```
* **Códigos de Error Posibles:**
    * `400 Bad Request`:
        * Si `ClientId` no es UUID válido.
        * Si `ClientPhoneNumber`  esta vacio.
        * Si `StartTime`  esta vacio.
        * Si `EndTime`  esta vacio.
        * Si `LocationId` esta vacio.
        * Si `TotalAmount` La cantidad a pagar debe ser mayor que cero.
        ```json
        {
          "Status" : 400,
          "Errors": {
            "name": "La cantidad a pagar debe ser mayor que cero"
          }
        }
        ```
    * `401 Unauthorized`: Si no se proporciona un token JWT o es inválido.
    * `403 Forbidden`: Si el usuario no tiene el permiso `reservations.create`.
   
    * `500 Internal Server Error`: 
      * Para errores inesperados.
        ```json
        {
          "Status" : 500,
          "Title" : "Error inesperado al crear la reserva.",
          "Detail" : "Ocurrió un error inesperado en el servidor."
        }
        ```
---

### Endpoint: `PUT /api/v1/reservations/id`

* **Descripción:** Permite actualizar una reserva.
* **Autenticación:** Requiere token JWT válido. Permiso: `reservations.create`.
* **Parámetros de Consulta (Route Parameters):**
    * `id` (obligatiro, int): Filtra reserva disponible por identificacion unico.
```
PUT /api/v1/reservations/1
```
* **Request Body (JSON - Ejemplo):**
    ```json
    {
        "Id" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "StartTime" : "2025-08-23",
        "EndTime" : "2025-08-23",
        "ClientPhoneNumber" : "223232113",
        "StatusId" : 1,
        "TotalAmount" : 230.99,
        "Quantity" : 109,
        "LocationId" : 4,
    }
    ```
* **Response (HTTP 204 No Content - Ejemplo de éxito):**
    ```json
    {
      "Status": 204
    }
    ```
* **Códigos de Error Posibles:**
    * `400 Bad Request`:
        * Si `ClientComment` excede el tamaño maximo permitido (2000 careacteres).
        ```json
        {
          "Status" : 400,
          "Errors": {
            "name": "El comentario del cliente supera los 200 caracteres."
          }
        }
        ```
    * `401 Unauthorized`: Si no se proporciona un token JWT o es inválido.
    * `403 Forbidden`: Si el usuario no tiene el permiso `reservations.create`.
    * `500 Internal Server Error`: 
      * Para errores inesperados.
        ```json
        {
          "Status" : 500,
          "Title" : "Error inesperado al actualizar la reserva.",
          "Detail" : "Ocurrió un error inesperado en el servidor."
        }
        ```
---

### Endpoint: `PUT /api/v1/reservations/status/id`

* **Descripción:** Permite actualizar el estatus de una reserva.
* **Autenticación:** Requiere token JWT válido. Permiso: `reservations.create`.
* **Parámetros de Consulta (Route Parameters):**
    * `id` (obligatiro, int): Filtra reserva disponible por identificacion unico.
```
PUT /api/v1/reservations/status/1
```
* **Request Body (JSON - Ejemplo):**
    ```json
    {
        "Id" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "AdminId" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "StatusId" : 1,
        "AdminComment" : "aprobado",
    }
    ```
* **Response (HTTP 204 No Content - Ejemplo de éxito):**
    ```json
    {
      "Status": 204
    }
    ```
* **Códigos de Error Posibles:**
    * `400 Bad Request`:
        * Si `AdminComment` excede el tamaño maximo permitido (2000 careacteres).
        ```json
        {
          "Status" : 400,
          "Errors": {
            "AdminComment": "El comentario del administrador supera los 200 caracteres."
          }
        }
        ```
    * `401 Unauthorized`: Si no se proporciona un token JWT o es inválido.
    * `403 Forbidden`: Si el usuario no tiene el permiso `reservations.create`.
    * `500 Internal Server Error`: 
      * Para errores inesperados.
        ```json
        {
          "Status" : 500,
          "Title" : "Error inesperado al actualizar la reserva.",
          "Detail" : "Ocurrió un error inesperado en el servidor."
        }
        ```
---
### Endpoint: `PUT /api/v1/reservations/transation/id`

* **Descripción:** Permite actualizar el id del pago de una reserva.
* **Autenticación:** Requiere token JWT válido. Permiso: `reservations.create`.
* **Parámetros de Consulta (Route Parameters):**
    * `id` (obligatiro, int): Filtra reserva disponible por identificacion unico.
```
PUT /api/v1/reservations/transation/1
```
* **Request Body (JSON - Ejemplo):**
    ```json
    {
        "Id" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "TrasnsationId" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "StatusId" : 1,
    }
    ```
* **Response (HTTP 204 No Content - Ejemplo de éxito):**
    ```json
    {
      "Status": 204
    }
    ```
* **Códigos de Error Posibles:**
    * `400 Bad Request`:
        * Si `TrasnsationId` esta vacio.
        ```json
        {
          "Status" : 400,
          "Errors": {
            "TrasnsationId": "El id de la transaccion es requerido."
          }
        }
        ```
    * `401 Unauthorized`: Si no se proporciona un token JWT o es inválido.
    * `403 Forbidden`: Si el usuario no tiene el permiso `reservations.create`.
    * `500 Internal Server Error`: 
      * Para errores inesperados.
        ```json
        {
          "Status" : 500,
          "Title" : "Error inesperado al actualizar la reserva.",
          "Detail" : "Ocurrió un error inesperado en el servidor."
        }
        ```
---

### Endpoint: `DELETE /api/v1/reservations/id`

* **Descripción:** Permite eliminar una reserva.
* **Autenticación:** Requiere token JWT válido. Permiso: `reservations.update`.
* **Parámetros de Consulta (Route Parameters):**
    * `id` (obligatiro, int): Filtra recurso disponible por identificacion unico.
```
DELETE /api/v1/reservations/1
```

* **Response (HTTP 204 No Content - Ejemplo de éxito):**
    ```json
    {
      "Status": 204
    }
    ```
* **Códigos de Error Posibles:**
    * `400 Bad Request`:
        * Si `id` no es un valor valido.
        ```json
        {
          "Status" : 400,
          "Title" : "Entrada inválida",
          "Detail" : "El ID de la reserva debe ser valido."
        }
        ```
    * `401 Unauthorized`: Si no se proporciona un token JWT o es inválido.
    * `403 Forbidden`: Si el usuario no tiene el permiso `reservations.delete`.
    * `500 Internal Server Error`: 
      * Para errores inesperados.
        ```json
        {
          "Status" : 500,
          "Title" : "Error inesperado al eliminar la reserva.",
          "Detail" : "Ocurrió un error inesperado en el servidor."
        }
        ```
---
