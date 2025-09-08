---
sidebar_position: 3
title: 03-endpoints
---

# 🔌 Contrato API

> Archivo: `03-endpoints.md`

Este documento detalla los **endpoints** expuestos por el backend para la feature `Location`. Incluye el método HTTP, la ruta, una descripción, así como ejemplos completos de solicitudes (requests) y respuestas (responses) para cada operación, incluyendo posibles códigos de error.

---

## 1. Resumen de Endpoints
Esta tabla proporciona una vista rápida de todos los endpoints relacionados con esta feature.

| Método HTTP | Ruta de la API                     | Descripción Breve                                        |
| :---------- | :--------------------------------- | :------------------------------------------------------- |
| `GET`       | `/api/v1/locations`                | Obtiene una lista de todas las ubicaciones disponibles.     |
| `GET`       | `/api/v1/locations/{id}`           | Obtiene los detalles de una ubicacion específica por su ID. |
| `GET`       | `/api/v1/locations/list`           | Obtiene una lista con solo el id y el nombre de todas las categorias disponibles. |
| `POST`      | `/api/v1/locations`             | Crea una nueva ubicacion para un recurso.                  |
| `PUT`       | `/api/v1/locations/{id}`        | Actualiza completamente una ubicacion existente.           |
| `DELETE`    | `/api/v1/locations/{id}`        | Elimina una ubicacion específica por su ID.                |

---
## 2. Detalle de Endpoints (Request & Response)

Para cada endpoint, se especifican los detalles de la solicitud, los parámetros, el cuerpo de la respuesta esperada y los posibles códigos de error.


### Endpoint: `GET /api/v1/locations`
* **Descripción:** Permite obtener un listado de todas las ubicaicones disponibles para los usuarios.
* **Autenticación:** Requiere token JWT válido. Permiso: `locations.read`.
* **Parámetros de Consulta (Query Parameters):**
    * `City` (opcional, string): Filtra ubicaciones disponibles por coincidencia de caracteres en el nombre de la ciudad.
    * `CreatedByUserIdFilter` (opcional, Guid): Filtra por usuario de creacion.
    * `OrderBy` (opcional, string): ordena ubicacion ascendete por nombre de ciudad o fecha de creacion (ej. `"City_asc"`, `"CreatedAt_asc"`).
* **Request (Ejemplo):**

```
GET /api/v1/locations
```

* **Response (HTTP 200 Ok - Ejemplo de éxito):**
    ```json
    [
      {
        "Id": 1,
        "Country": "Mexico",
        "City": "Veracruz",
        "ZipCode": 5876,
        "Street": "calle 23",
        "Neighborhood": "centro",
        "ExteriorNumber": "1223",
        "InteriorNumber": "A",
        "CreateAt": "2025-08-03",
        "CreatedByUserId": "f5e4d3c2-b1a0-9876-5432-10fedcba9876"
      },
      {
        "Id": 2,
        "Country": "Mexico",
        "City": "Oaxaca",
        "ZipCode": 4211,
        "Street": "calle 12",
        "Neighborhood": "centro",
        "ExteriorNumber": "1223",
        "InteriorNumber": "A",
        "CreateAt": "2025-08-03",
        "CreatedByUserId": "f5e4d3c2-b1a0-9876-5432-10fedcba9876"
      }
    ]
    ```
* **Códigos de Error Posibles:**
    * `500 Internal Server Error"`: Si la operacion falla.
        ```json
        {
          "Status": 500,
          "Title": "Error inesperado al consultar las ubicaciones.",
          "Detail": "Ocurrió un error inesperado en el servidor."
        }
        ```
---        
### Endpoint: `GET /api/v1/locations/id`
* **Descripción:** Permite obtener un listado de todas las ubicacoines disponibles para los recursos o reservas.
* **Autenticación:** Requiere token JWT válido. Permiso: `locaitons.read`.
* **Parámetros de Consulta (Route Parameters):**
    * `id` (obligatiro, int): Filtra ubicaciones disponibles por identificacion unico.
   
* **Request (Ejemplo):**

```
GET /api/v1/locations/1
```

* **Response (HTTP 200 Ok - Ejemplo de éxito):**
    ```json
      {
        "Id": 1,
        "Country": "Mexico",
        "City": "Veracruz",
        "ZipCode": 5876,
        "Street": "calle 23",
        "Neighborhood": "centro",
        "ExteriorNumber": "1223",
        "InteriorNumber": "A",
        "CreateAt": "2025-08-03",
        "CreatedByUserId": "f5e4d3c2-b1a0-9876-5432-10fedcba9876"
      }
    ```
* **Códigos de Error Posibles:**
    * `404 Not Found`: Si el `id`de la ubicación no existe.
        ```json
        {
          "Status": 404,
          "Title": "Recurso no encontrado",
          "Detail": "La operación de consulta falló porque la ubicación no existe."
        }
        ```
    * `500 Internal Server Error"`: Si la operacion falla.
        ```json
        {
          "Status": 500,
          "Title": "Error inesperado al consultar las ubicación.",
          "Detail": "Ocurrió un error inesperado en el servidor."
        }
        ```
---

### Endpoint: `POST /api/v1/locations`

* **Descripción:** Permite crear una nueva ubicación.
* **Autenticación:** Requiere token JWT válido. Permiso: `locations.create`.
* **Request Body (JSON - Ejemplo):**
    ```json
    {
        "Country": "Mexico",
        "City": "Veracruz",
        "ZipCode": 5876,
        "Street": "calle 23",
        "Neighborhood": "centro",
        "ExteriorNumber": "1223",
        "InteriorNumber": "A",
        "CreateAt": "2025-08-03",
        "CreatedByUserId": "f5e4d3c2-b1a0-9876-5432-10fedcba9876"
    }
    ```
* **Response (HTTP 201 Created - Ejemplo de éxito):**
    ```json
    {
      "id": "1",
      "City": "Veracruz",
      "ZipCode": 5876,
      "Street": "calle 23",
      "Neighborhood": "centro",
      "ExteriorNumber": "1223",
      "InteriorNumber": "A",
      "CreateAt": "2025-08-03",
      "CreatedByUserId": "f5e4d3c2-b1a0-9876-5432-10fedcba9876"
    }
    ```
* **Códigos de Error Posibles:**
    * `400 Bad Request`:
        * Si `CreatedByUserId` no es UUID válido.
        * Si `City` Se encuetra vacia o con un valor null.
        * Si `Country ` Se encuetra vacia o con un valor null.
        * Si `zipCode  ` Se encuetra con un valor invalido o negativo.
        * Si `street ` Se encuetra vacia o con un valor null.
        * Si `exteriorNumber ` Se encuetra vacia o con un valor null.
        ```json
        {
          "Status" : 400,
          "Errors": {
            "City": "El nombre supera los 100 caracteres."
          }
        }
        ```
    * `401 Unauthorized`: Si no se proporciona un token JWT o es inválido.
    * `403 Forbidden`: Si el usuario no tiene el permiso `locations.create`.
    * `500 Internal Server Error`: 
      * Para errores inesperados.
        ```json
        {
          "Status" : 500,
          "Title" : "Error inesperado al crear la ubiccion.",
          "Detail" : "Ocurrió un error inesperado en el servidor."
        }
        ```
---

### Endpoint: `PUT /api/v1/locations/id`

* **Descripción:** Permite actualizar una ubicaciones.
* **Autenticación:** Requiere token JWT válido. Permiso: `locations.create`.
* **Parámetros de Consulta (Route Parameters):**
    * `id` (obligatiro, int): Filtra ubicaciones disponibles por identificacion unico.
```
PUT /api/v1/locations/1
```
* **Request Body (JSON - Ejemplo):**
    ```json
    {
      "City": "Veracruz",
      "ZipCode": 5876,
      "Street": "calle 23",
      "Neighborhood": "centro",
      "ExteriorNumber": "1223",
      "InteriorNumber": "A",
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
        * Si `CreatedByUserId` no es UUID válido.
        * Si `City` Se encuetra vacia o con un valor null.
        * Si `Country ` Se encuetra vacia o con un valor null.
        * Si `zipCode  ` Se encuetra con un valor invalido o negativo.
        * Si `street ` Se encuetra vacia o con un valor null.
        * Si `exteriorNumber ` Se encuetra vacia o con un valor null.
        ```json
        {
          "Status" : 400,
          "Errors": {
            "City": "El nombre supera los 100 caracteres."
          }
        }
        ```
    * `401 Unauthorized`: Si no se proporciona un token JWT o es inválido.
    * `403 Forbidden`: Si el usuario no tiene el permiso `locations.create`.
    * `500 Internal Server Error`: 
      * Para errores inesperados.
        ```json
        {
          "Status" : 500,
          "Title" : "Error inesperado al actualizar la ubicion.",
          "Detail" : "Ocurrió un error inesperado en el servidor."
        }
        ```
---
### Endpoint: `DELETE /api/v1/locations/id`

* **Descripción:** Permite eliminar una ubicaion.
* **Autenticación:** Requiere token JWT válido. Permiso: `locations.update`.
* **Parámetros de Consulta (Route Parameters):**
    * `id` (obligatiro, int): Filtra ubicaciones disponibles por identificacion unico.
```
PUT /api/v1/locations/1
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
          "Detail" : "El ID de la ubicaion debe ser mayor que cero."
        }
        ```
    * `401 Unauthorized`: Si no se proporciona un token JWT o es inválido.
    * `403 Forbidden`: Si el usuario no tiene el permiso `locations.delete`.
    * `500 Internal Server Error`: 
      * Para errores inesperados.
        ```json
        {
          "Status" : 500,
          "Title" : "Error inesperado al eliminar la ubicación.",
          "Detail" : "Ocurrió un error inesperado en el servidor."
        }
        ```
---
