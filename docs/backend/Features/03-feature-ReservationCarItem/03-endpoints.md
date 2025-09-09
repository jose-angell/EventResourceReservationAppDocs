---
sidebar_position: 3
title: 03-endpoints
---

# 🔌 Contrato API

> Archivo: `03-endpoints.md`

Este documento detalla los **endpoints** expuestos por el backend para la feature `ReservationCarItem`. Incluye el método HTTP, la ruta, una descripción, así como ejemplos completos de solicitudes (requests) y respuestas (responses) para cada operación, incluyendo posibles códigos de error.

---

## 1. Resumen de Endpoints
Esta tabla proporciona una vista rápida de todos los endpoints relacionados con esta feature.

| Método HTTP | Ruta de la API                     | Descripción Breve                                        |
| :---------- | :--------------------------------- | :------------------------------------------------------- |
| `GET`       | `/api/v1/ReservationCarItems`   | Obtiene una lista de todos los items en el carrito de reservas disponibles para el usuario.     |
| `POST`      | `/api/v1/ReservationCarItems`   | Crea un nuevo item para el carrito de reservas.                  |
| `PUT`       | `/api/v1/ReservationCarItems/{id}`    | Actualiza la cantidad de un item existente.           |
| `DELETE`    | `/api/v1/ReservationCarItems/{id}`    | Elimina una item específica por su ID.                |

---
## 2. Detalle de Endpoints (Request & Response)

Para cada endpoint, se especifican los detalles de la solicitud, los parámetros, el cuerpo de la respuesta esperada y los posibles códigos de error.


### Endpoint: `GET /api/v1/ReservationCarItems`
* **Descripción:** Permite obtener un listado de todos los items en el carrito de reservas disponibles para un usuario.
* **Autenticación:** Requiere token JWT válido. Permiso: `ReservationCarItem.read`.
* **Parámetros de Consulta (Query Parameters):**
    * `ClientId` (obligatorio, string): Filtra los elementos disponibles por coincidencia de id con el del usuario.
   
* **Request (Ejemplo):**

```
GET /api/v1/ReservationCarItems
```

* **Response (HTTP 200 Ok - Ejemplo de éxito):**
    ```json
    [
      {
        "Id": "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "Clientid": "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "ResourceId": "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "Quantity": 12,
        "AddedAt": "2025-08-03"
      },
      {
        "Id": "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "Clientid": "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "ResourceId": "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "Quantity": 12,
        "AddedAt": "2025-08-03"
      }
    ]
    ```
* **Códigos de Error Posibles:**
    * `500 Internal Server Error"`: Si la operacion falla.
        ```json
        {
          "Status": 500,
          "Title": "Error interno del servidor",
          "Detail": "Ocurrió un error inesperado. Por favor, inténtelo de nuevo más tarde."
        }
        ```
---        

### Endpoint: `POST /api/v1/ReservationCarItems`

* **Descripción:** Permite crear un nuevo elemento a la lista de recursos para el carrito de reservas.
* **Autenticación:** Requiere token JWT válido. Permiso: `ReservationCarItems.create`.
* **Request Body (JSON - Ejemplo):**
    ```json
    {
        "ClientId": "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "ResourceId": "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "Quantity": 12
    }
    ```
* **Response (HTTP 201 Created - Ejemplo de éxito):**
    ```json
    {
      "Id":  "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
      "ClientId": "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
      "ResourceId": "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
      "Quantity": 12
    }
    ```
* **Códigos de Error Posibles:**
    * `400 Bad Request`:
        * Si `ClientId` no es UUID válido.
        * Si `ResourceId` no es UUID válidol.
        * Si `Quantity ` es menor o igual a cero.
        
        ```json
        {
          "Status" : 400,
          "Errors": {
            "Quantity": "La cantidad debe ser mayor a cero."
          }
        }
        ```
    * `401 Unauthorized`: Si no se proporciona un token JWT o es inválido.
    * `403 Forbidden`: Si el usuario no tiene el permiso `ReservationCarItems.create`.
    * `500 Internal Server Error`: 
      * Para errores inesperados.
        ```json
        {
          "Status" : 500,
          "Title": "Error interno del servidor",
          "Detail": "Ocurrió un error inesperado. Por favor, inténtelo de nuevo más tarde."
        }
        ```
---

### Endpoint: `PUT /api/v1/ReservationCarItems/id`

* **Descripción:** Permite actualizar la cantidad de un item en la lista.
* **Autenticación:** Requiere token JWT válido. Permiso: `ReservationCarItems.create`.
* **Parámetros de Consulta (Route Parameters):**
    * `Id` (obligatiro, int): Filtra el item  disponibles por identificacion unico.
```
PUT /api/v1/ReservationCarItems/1
```
* **Request Body (JSON - Ejemplo):**
    ```json
    {
      "Id": "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
      "Quantity": 43,
      
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
        * Si `Id` no es UUID válido.
        * Si `Quantity` es un valor menor o igual a cero.
        
        ```json
        {
          "Status" : 400,
          "Errors": {
            "Quantity": "La cantidad debe ser mayor a cero."
          }
        }
        ```
    * `401 Unauthorized`: Si no se proporciona un token JWT o es inválido.
    * `403 Forbidden`: Si el usuario no tiene el permiso `ReservationCarItems.create`.
    * `500 Internal Server Error`: 
      * Para errores inesperados.
        ```json
        {
          "Status" : 500,
          "Title": "Error interno del servidor",
          "Detail": "Ocurrió un error inesperado. Por favor, inténtelo de nuevo más tarde."
        }
        ```
---
### Endpoint: `DELETE /api/v1/ReservationCarItems/id`

* **Descripción:** Permite eliminar un item.
* **Autenticación:** Requiere token JWT válido. Permiso: `ReservationCarItems.update`.
* **Parámetros de Consulta (Route Parameters):**
    * `Id` (obligatiro, int): Filtra el item  disponibles por identificacion unico.
```
PUT /api/v1/ReservationCarItems/1
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
          "Detail" : "El ID del item del carrito de reservas debe ser valido."
        }
        ```
    * `401 Unauthorized`: Si no se proporciona un token JWT o es inválido.
    * `403 Forbidden`: Si el usuario no tiene el permiso `locations.delete`.
    * `500 Internal Server Error`: 
      * Para errores inesperados.
        ```json
        {
          "Status" : 500,
          "Title": "Error interno del servidor",
          "Detail": "Ocurrió un error inesperado. Por favor, inténtelo de nuevo más tarde."
        }
        ```
---
