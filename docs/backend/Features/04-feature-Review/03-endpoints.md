---
sidebar_position: 3
title: 03-endpoints
---

# 🔌 Contrato API

> Archivo: `03-endpoints.md`

Este documento detalla los **endpoints** expuestos por el backend para la feature `Review`. Incluye el método HTTP, la ruta, una descripción, así como ejemplos completos de solicitudes (requests) y respuestas (responses) para cada operación, incluyendo posibles códigos de error.

---

## 1. Resumen de Endpoints
Esta tabla proporciona una vista rápida de todos los endpoints relacionados con esta feature.

| Método HTTP | Ruta de la API                     | Descripción Breve                                        |
| :---------- | :--------------------------------- | :------------------------------------------------------- |
| `GET`       | `/api/v1/Reviews`   | Obtiene una lista de todas las reseñas disponibles para mostrar.     |
| `GET`       | `/api/v1/Reviews/{id}`   | Obtiene los detalles de una reseña específica por su ID.     |
| `POST`      | `/api/v1/Reviews`   | Crea una nueva reseñas.                  |
| `PUT`       | `/api/v1/Reviews/{id}`    | Actualiza el comentario y el riting de una reseña existente.           |
| `DELETE`    | `/api/v1/Reviews/{id}`    | Elimina una reseña específica por su ID.                |

---
## 2. Detalle de Endpoints (Request & Response)

Para cada endpoint, se especifican los detalles de la solicitud, los parámetros, el cuerpo de la respuesta esperada y los posibles códigos de error.


### Endpoint: `GET /api/v1/Reviews`
* **Descripción:** Permite obtener un listado de todos los items en el carrito de reservas disponibles para un usuario.
* **Autenticación:** Requiere token JWT válido. Permiso: `Reviews.read`.
* **Parámetros de Consulta (Query Parameters):**
    * `ResourceId` (opcional, Guid): Filtra los elementos disponibles por coincidencia de id con el del recurso.
    * `UserId` (opcional, Guid): Filtra los elementos disponibles por coincidencia de id con el del usuario.
    * `Rating` (opcional, int): Filtra los elementos disponibles por coincidencia de el numero de valoraciones.
    * `CreatedAt` (opcional, datetime): Filtra los elementos disponibles por coincidencia fecha de creacion.
    * `OrderBy` (opcional, string): ordemaniento de la consulta (`rating_asc`, `rating_desc`, `date_asc`, `date_desc`).
   
* **Request (Ejemplo):**

```
GET /api/v1/Reviews
```

* **Response (HTTP 200 Ok - Ejemplo de éxito):**
    ```json
    [
      {
        "Id": "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "ResourceId": "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "UserId": "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "Rating": 5,
        "Comment": "Exelente servicio y atencion.",
        "AddedAt": "2025-08-03"
      },
      {
        "Id": "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "ResourceId": "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "UserId": "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "Rating": 5,
        "Comment": "Exelente servicio y atencion.",
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

### Endpoint: `POST /api/v1/Reviews`

* **Descripción:** Permite crear una nueva reseña de los recursos en el sistema.
* **Autenticación:** Requiere token JWT válido. Permiso: `Reviews.create`.
* **Request Body (JSON - Ejemplo):**
    ```json
    {
        "ResourceId": "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "UserId": "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "Rating": 5,
        "Comment": "Exelente servicio y atencion."
    }
    ```
* **Response (HTTP 201 Created - Ejemplo de éxito):**
    ```json
    {
      "Id":  "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
      "ResourceId": "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
      "UserId": "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
      "Rating": 5,
      "Comment": "Exelente servicio y atencion.",
      "AddedAt": "2025-08-03"
    }
    ```
* **Códigos de Error Posibles:**
    * `400 Bad Request`:
        * Si `ClientId` no es UUID válido.
        * Si `ResourceId` no es UUID válidol.
        * Si `Riting ` es menor o igual a cero o mayor a 5.
        * Si `Comment ` esta vacio.
        
        ```json
        {
          "Status" : 400,
          "Errors": {
            "Riting": "La cantidad debe estar entre 1 y 5."
          }
        }
        ```
    * `401 Unauthorized`: Si no se proporciona un token JWT o es inválido.
    * `403 Forbidden`: Si el usuario no tiene el permiso `Review.create`.
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

### Endpoint: `PUT /api/v1/Reviews/id`

* **Descripción:** Permite actualizar la puntuacion y el comentario de la reseña.
* **Autenticación:** Requiere token JWT válido. Permiso: `Review.create`.
* **Parámetros de Consulta (Route Parameters):**
    * `Id` (obligatiro, int): Filtra el item  disponibles por identificacion unico.
* **Request Body (Body Parameters):**
    * `Id` (obligatiro, int): Filtra el item  disponibles por identificacion unico y debe ser el mismo al de la ruta.
    * `Rating` (obligatiro, int): Puntuacion de el recurso.
    * `Comment` (obligatiro, int): Comentario de el recurso.
```
PUT /api/v1/Reviews/1
```
* **Request Body (JSON - Ejemplo):**
    ```json
    {
      "Id": "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
      "Rating": 4,
      "Comment": "buen servicio",
      
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
        * Si `Rating` es un valor menor o igual a cero o mayor a 5.
        * Si `Comment` esta vacio.
        
        ```json
        {
          "Status" : 400,
          "Errors": {
            "Quantity": "La cantidad debe ser mayor a cero."
          }
        }
        ```
    * `401 Unauthorized`: Si no se proporciona un token JWT o es inválido.
    * `403 Forbidden`: Si el usuario no tiene el permiso `Review.update`.
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
### Endpoint: `DELETE /api/v1/Reviews/id`

* **Descripción:** Permite eliminar una reseña.
* **Autenticación:** Requiere token JWT válido. Permiso: `Review.update`.
* **Parámetros de Consulta (Route Parameters):**
    * `Id` (obligatiro, int): Filtra el item  disponibles por identificacion unico.
```
PUT /api/v1/Reviews/1
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
          "Detail" : "El ID del item de la reseña debe ser valido."
        }
        ```
    * `401 Unauthorized`: Si no se proporciona un token JWT o es inválido.
    * `403 Forbidden`: Si el usuario no tiene el permiso `review.delete`.
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
