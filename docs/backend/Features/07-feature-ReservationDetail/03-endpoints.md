---
sidebar_position: 3
title: 03-endpoints
---

# 🔌 Contrato API

> Archivo: `03-endpoints.md`

Este documento detalla los **endpoints** expuestos por el backend para la feature `ReservationDetail`. Incluye el método HTTP, la ruta, una descripción, así como ejemplos completos de solicitudes (requests) y respuestas (responses) para cada operación, incluyendo posibles códigos de error.

---

## 1. Resumen de Endpoints
Esta tabla proporciona una vista rápida de todos los endpoints relacionados con esta feature.

| Método HTTP | Ruta de la API                     | Descripción Breve                                        |
| :---------- | :--------------------------------- | :------------------------------------------------------- |
| `GET`       | `/api/v1/reservation-details`                | Obtiene una lista de todos los detalles de las reservas disponibles por id de resevacion.     |
| `GET`       | `/api/v1/reservation-details/{id}`           | Obtiene los detalles de una reserva específica por su ID. |
| `POST`      | `/api/v1/reservation-details`                | Crea un nuevo detalle de reserva para una reserva.                  |
| `PUT`       | `/api/v1/reservation-details/{id}`           | Actualiza la cantidad de recursos para un detalle de una reserva existente.           |
| `DELETE`    | `/api/v1/reservation-details/{id}`           | Elimina una reserva específica por su ID.                |

---
## 2. Detalle de Endpoints (Request & Response)

Para cada endpoint, se especifican los detalles de la solicitud, los parámetros, el cuerpo de la respuesta esperada y los posibles códigos de error.


### Endpoint: `GET /api/v1/reservation-details`
* **Descripción:** Permite obtener un listado de todos los detalles de la reservacion disponibles para mostrar al usuario.
* **Autenticación:** Requiere token JWT válido. Permiso: `reservationDetails.read`.
* **Parámetros de Consulta (Query Parameters):**
    * `id` (obligatorio, DateTime): Filtra los detalles la reserva disponibles por Id de reserva.
* **Request (Ejemplo):**

```
GET /api/v1/reservation-details
```

* **Response (HTTP 200 Ok - Ejemplo de éxito):**
    ```json
    [
      {
        "Id" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "ReservationId" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "ResourceId" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "ResourceName" : "mesas",
        "UnitPrice" : 230.99,
        "Quantity" : 109,
        "Created" : "2025-08-23"
      },
      {
        "Id" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "ReservationId" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "ResourceId" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "ResourceName" : "mesas",
        "UnitPrice" : 230.99,
        "Quantity" : 109,
        "Created" : "2025-08-23"
      }
    ]
    ```
* **Códigos de Error Posibles:**
    * `500 Internal Server Error"`: Si la operacion falla.
        ```json
        {
          "Status": 500,
          "Title": "Error inesperado al consultar los detalles de las reservas.",
          "Detail": "Ocurrió un error inesperado en el servidor."
        }
        ```
---        
### Endpoint: `GET /api/v1/reservation-details/id`
* **Descripción:** Permite obtener un detalle de reserva utilizando su Id.
* **Autenticación:** Requiere token JWT válido. Permiso: `reservationDetails.read`.
* **Parámetros de Consulta (Route Parameters):**
    * `id` (obligatiro, int): Filtra el detalle de la reserva disponible por identificacion unico.
   
* **Request (Ejemplo):**

```
GET /api/v1/reservation-details/1
```

* **Response (HTTP 200 Ok - Ejemplo de éxito):**
    ```json
      {
        "Id" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "ReservationId" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "ResourceId" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "ResourceName" : "mesas",
        "UnitPrice" : 230.99,
        "Quantity" : 109,
        "Created" : "2025-08-23"
      }
    ```
* **Códigos de Error Posibles:**
    * `404 Not Found`: Si el `id`de la reserva no existe.
        ```json
        {
          "Status": 404,
          "Title": "Recurso no encontrado",
          "Detail": "La operación de consulta falló porque el detalle de la reserva no existe."
        }
        ```
    * `500 Internal Server Error"`: Si la operacion falla.
        ```json
        {
          "Status": 500,
          "Title": "Error inesperado al consultar el detalle de la reserva.",
          "Detail": "Ocurrió un error inesperado en el servidor."
        }
        ```
---
### Endpoint: `POST /api/v1/reservation-details`

* **Descripción:** Permite crear un nuevo detalle de resevacion.
* **Autenticación:** Requiere token JWT válido. Permiso: `reservationDetails.create`.
* **Request Body (JSON - Ejemplo):**
    ```json
    {
        "Id" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "ReservationId" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "ResourceId" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "UnitPrice" : 230.99,
        "Quantity" : 109
    }
    ```
* **Response (HTTP 201 Created - Ejemplo de éxito):**
    ```json
    {
        "Id" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "ReservationId" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "ResourceId" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "UnitPrice" : 230.99,
        "Quantity" : 109
    }
    ```
* **Códigos de Error Posibles:**
    * `400 Bad Request`:
        * Si `ReservationId` no es UUID válido.
        * Si `ResourceId`   no es UUID válido.
        * Si `UnitPrice` es un numero invalido.
        * Si `Quantity` es un numero invalido.
        ```json
        {
          "Status" : 400,
          "Errors": {
            "Quantity": "La cantidad de recursos solicitados no es valido",
            "UnitPrice": "El valalor unitario del recurso no es valido"
          }
        }
        ```
    * `401 Unauthorized`: Si no se proporciona un token JWT o es inválido.
    * `403 Forbidden`: Si el usuario no tiene el permiso `reservationDetails.create`.
   
    * `500 Internal Server Error`: 
      * Para errores inesperados.
        ```json
        {
          "Status" : 500,
          "Title" : "Error inesperado al crear el detalle de la reserva.",
          "Detail" : "Ocurrió un error inesperado en el servidor."
        }
        ```
---

### Endpoint: `PUT /api/v1/reservation-details/id`

* **Descripción:** Permite actualizar el detalle de una reserva.
* **Autenticación:** Requiere token JWT válido. Permiso: `reservations.create`.
* **Parámetros de Consulta (Route Parameters):**
    * `id` (obligatiro, int): Filtra reserva disponible por identificacion unico.
```
PUT /api/v1/reservation-details/1
```
* **Request Body (JSON - Ejemplo):**
    ```json
    {
        "Id" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "Quantity" : 109,
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
        * Si `Quantity` no es un numero valido.
        ```json
        {
          "Status" : 400,
          "Errors": {
            "Quantity": "La cantidad de recursos solicitada debe ser mayor a cero"
          }
        }
        ```
    * `401 Unauthorized`: Si no se proporciona un token JWT o es inválido.
    * `403 Forbidden`: Si el usuario no tiene el permiso `reservationDetailss.create`.
    * `500 Internal Server Error`: 
      * Para errores inesperados.
        ```json
        {
          "Status" : 500,
          "Title" : "Error inesperado al actualizar el detalle de la reserva.",
          "Detail" : "Ocurrió un error inesperado en el servidor."
        }
        ```
---

### Endpoint: `DELETE /api/v1/reservation-details/id`

* **Descripción:** Permite eliminar un detalle de una reserva.
* **Autenticación:** Requiere token JWT válido. Permiso: `reservationDetails.update`.
* **Parámetros de Consulta (Route Parameters):**
    * `id` (obligatiro, int): Filtra recurso disponible por identificacion unico.
```
DELETE /api/v1/reservation-details/1
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
          "Detail" : "El ID del detalle de la reserva debe ser valido."
        }
        ```
    * `401 Unauthorized`: Si no se proporciona un token JWT o es inválido.
    * `403 Forbidden`: Si el usuario no tiene el permiso `reservationDetails.delete`.
    * `500 Internal Server Error`: 
      * Para errores inesperados.
        ```json
        {
          "Status" : 500,
          "Title" : "Error inesperado al eliminar el detalle de la reserva.",
          "Detail" : "Ocurrió un error inesperado en el servidor."
        }
        ```
---
