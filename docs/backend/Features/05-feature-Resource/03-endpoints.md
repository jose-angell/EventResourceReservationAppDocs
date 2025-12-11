---
sidebar_position: 3
title: 03-endpoints
---

# 🔌 Contrato API

> Archivo: `03-endpoints.md`

Este documento detalla los **endpoints** expuestos por el backend para la feature `Resource`. Incluye el método HTTP, la ruta, una descripción, así como ejemplos completos de solicitudes (requests) y respuestas (responses) para cada operación, incluyendo posibles códigos de error.

---

## 1. Resumen de Endpoints
Esta tabla proporciona una vista rápida de todos los endpoints relacionados con esta feature.

| Método HTTP | Ruta de la API                     | Descripción Breve                                        |
| :---------- | :--------------------------------- | :------------------------------------------------------- |
| `GET`       | `/api/v1/resources`                | Obtiene una lista de todos los recursos disponibles.     |
| `GET`       | `/api/v1/resources/{id}`           | Obtiene los detalles de un recurso específica por su ID. |
| `GET`       | `/api/v1/resources/date-range/{id}`| Obtiene los detalles de un recurso específica por su ID y un rango de fechas. |
| `POST`      | `/api/v1/resources`                | Crea un nuevo recurso para un recurso.                  |
| `PUT`       | `/api/v1/resources/{id}`           | Actualiza completamente un recurso existente.           |
| `DELETE`    | `/api/v1/resources/{id}`           | Elimina un recurso específica por su ID.                |

---
## 2. Detalle de Endpoints (Request & Response)

Para cada endpoint, se especifican los detalles de la solicitud, los parámetros, el cuerpo de la respuesta esperada y los posibles códigos de error.


### Endpoint: `GET /api/v1/resources`
* **Descripción:** Permite obtener un listado de todas los recurso disponibles para mostrar al usuario.
* **Autenticación:** Requiere token JWT válido. Permiso: `resources.read`.
* **Parámetros de Consulta (Query Parameters):**
    * `Name` (opcional, string): Filtra recursos disponibles por coincidencia de caracteres en el nombre.
    * `StartTime` (obligatorio, DateTime): Filtra la cantidad de recursos disponibles por rango de fecha.
    * `EndTime` (obligatorio, DateTime): Filtra la cantidad de recursos disponibles por rango de fecha.
    * `CategoryId` (opcional, int): Filtra los recursos disponibles por categoria.
    * `StatusId` (opcional, int): Filtra los recursos disponibles por estatus.
    * `MinPrice` (opcional, decimal):Filtra los recursos disponibles por rango de precios.
    * `MaxPrice` (opcional, decimal): Filtra los recursos disponibles por rango de precios.
    * `CreatedByUserIdFilter` (opcional, Guid): Filtra por usuario de creacion.
    * `OrderBy` (opcional, string): ordena recursos ascendete por nombre o fecha de creacion (ej. `"Name_asc"`, `"CreatedAt_asc"`).
* **Request (Ejemplo):**

```
GET /api/v1/resources
```

* **Response (HTTP 200 Ok - Ejemplo de éxito):**
    ```json
    [
      {
        "Id" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "StatusId" : 1,
        "StatusDescription" : "Disponible",
        "Name" : "Decoraciones luz blanca",
        "Description" : "Decoraciones y luz para exterior blanca.",
        "Price" : 230.99,
        "Quantity" : 109,
        "QuantityInUse": 19,
        "AvailableQuantity": 109,
        "CategoryId" : 3,
        "CategoryName" : "Iluminacion",
        "LocationId" : 4,
        "LocationDescription" : "colonia del bosque",
        "AuthorizationType" : 1,
        "Created" : "2025-08-23"
      },
      {
        "Id" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "StatusId" : 1,
        "StatusDescription" : "Disponible",
        "Name" : "Decoraciones luz blanca",
        "Description" : "Mesas y sillas para exterior blanca.",
        "Price" : 230.99,
        "Quantity" : 109,
        "QuantityInUse": 19,
        "AvailableQuantity": 109,
        "CategoryId" : 3,
        "CategoryName" : "Muebles",
        "LocationId" : 4,
        "LocationDescription" : "colonia del bosque",
        "AuthorizationType" : 1,
        "Created" : "2025-08-23"
      }
    ]
    ```
* **Códigos de Error Posibles:**
    * `500 Internal Server Error"`: Si la operacion falla.
        ```json
        {
          "Status": 500,
          "Title": "Error inesperado al consultar los recursos.",
          "Detail": "Ocurrió un error inesperado en el servidor."
        }
        ```
---        
### Endpoint: `GET /api/v1/resources/id`
* **Descripción:** Permite obtener un recurso utilizando su Id.
* **Autenticación:** Requiere token JWT válido. Permiso: `resources.read`.
* **Parámetros de Consulta (Route Parameters):**
    * `id` (obligatiro, int): Filtra recurso disponible por identificacion unico.
   
* **Request (Ejemplo):**

```
GET /api/v1/resources/1
```

* **Response (HTTP 200 Ok - Ejemplo de éxito):**
    ```json
      {
        "Id" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "StatusId" : 1,
        "StatusDescription" : "Disponible",
        "Name" : "Decoraciones luz blanca",
        "Description" : "Mesas y sillas para exterior blanca.",
        "Price" : 230.99,
        "Quantity" : 109,
        "QuantityInUse": 0,
        "AvailableQuantity": 109,
        "CategoryId" : 3,
        "CategoryName" : "Muebles",
        "LocationId" : 4,
        "LocationDescription" : "colonia del bosque",
        "AuthorizationType" : 1,
        "Created" : "2025-08-23"
      }
    ```
* **Códigos de Error Posibles:**
    * `404 Not Found`: Si el `id`de la categoria no existe.
        ```json
        {
          "Status": 404,
          "Title": "Recurso no encontrado",
          "Detail": "La operación de consulta falló porque el recurso no existe."
        }
        ```
    * `500 Internal Server Error"`: Si la operacion falla.
        ```json
        {
          "Status": 500,
          "Title": "Error inesperado al consultar el recurso.",
          "Detail": "Ocurrió un error inesperado en el servidor."
        }
        ```
---
### Endpoint: `GET /api/v1/resources/date-range/id`
* **Descripción:** Permite obtener un recurso utilizando su Id dentro de un rango de fechas.
* **Autenticación:** Requiere token JWT válido. Permiso: `resources.read`.
* **Parámetros de Consulta (Route Parameters):**
    * `Id` (obligatiro, int): Filtra recurso disponible por identificacion unico.
    * `StartTime` (obligatiro, int): Filtra la cantdad de recursos disponible por rango de fechas.
    * `EndTime` (obligatiro, int): Filtra la cantdad de recursos disponible por rango de fechas.

* **Request (Ejemplo):**

```
GET /api/v1/resources/date-range/1
```

* **Response (HTTP 200 Ok - Ejemplo de éxito):**
    ```json
    [
      {
        "Id" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "StatusId" : 1,
        "StatusDescription" : "Disponible",
        "Name" : "Decoraciones luz blanca",
        "Description" : "Mesas y sillas para exterior blanca.",
        "Price" : 230.99,
        "Quantity" : 109,
        "QuantityInUse": 19,
        "AvailableQuantity": 90,
        "CategoryId" : 3,
        "CategoryName" : "Muebles",
        "LocationId" : 4,
        "LocationDescription" : "colonia del bosque",
        "AuthorizationType" : 1,
        "Created" : "2025-08-23"
      }
    ]
    ```
* **Códigos de Error Posibles:**
    * `500 Internal Server Error"`: Si la operacion falla.
        ```json
        {
          "Status": 500,
          "Title": "Error inesperado al consultar el recuros.",
          "Detail": "Ocurrió un error inesperado en el servidor."
        }
        ```
---
### Endpoint: `POST /api/v1/resources`

* **Descripción:** Permite crear un nuevo recurso.
* **Autenticación:** Requiere token JWT válido. Permiso: `resources.create`.
* **Request Body (JSON - Ejemplo):**
    ```json
    {
        "StatusId" : 1,
        "Name" : "Decoraciones luz blanca",
        "Description" : "Mesas y sillas para exterior blanca.",
        "Price" : 230.99,
        "Quantity" : 109,
        "CategoryId" : 3,
        "LocationId" : 4,
        "AuthorizationType" : 1
    }
    ```
* **Response (HTTP 201 Created - Ejemplo de éxito):**
    ```json
    {
      "Id" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
      "StatusId" : 1,
      "StatusDescription" : "Disponible",
      "Name" : "Decoraciones luz blanca",
      "Description" : "Mesas y sillas para exterior blanca.",
      "Price" : 230.99,
      "Quantity" : 109,
      "QuantityInUse": 19,
      "AvailableQuantity": 90,
      "CategoryId" : 3,
      "CategoryName" : "Muebles",
      "LocationId" : 4,
      "LocationDescription" : "colonia del bosque",
      "AuthorizationType" : 1,
      "Created" : "2025-08-23"
    }
    ```
* **Códigos de Error Posibles:**
    * `400 Bad Request`:
        * Si `CreatedByUserId` no es UUID válido.
        * Si `CategoryId`  esta vacio.
        * Si `StatusId`  esta vacio.
        * Si `AuthorizationType` esta vacio.
        * Si `LocationId` esta vacio.
        * Si `Name` el nombre debe tener entre 1 y 200 caracteres. .
        * Si `Description` excede el tamaño maximo permitido (2000 careacteres).
        * Si `Price` El precio debe ser mayor que cero.
        ```json
        {
          "Status" : 400,
          "Errors": {
            "name": "El nombre supera los 100 caracteres."
          }
        }
        ```
    * `401 Unauthorized`: Si no se proporciona un token JWT o es inválido.
    * `403 Forbidden`: Si el usuario no tiene el permiso `resources.create`.
    * `409 Conflict`: 
      * Si el nombre del esta duplicado en la misma ubicación.
        ```json
        {
          "Status" : 409,
          "Title" : "Conflicto al crear el recurso",
          "Detail" : "Ya existe un recurso con el nombre '{request.Name}' en la ubicación especificada."
        }
        ```
    * `500 Internal Server Error`: 
      * Para errores inesperados.
        ```json
        {
          "Status" : 500,
          "Title" : "Error inesperado al crear el recurso.",
          "Detail" : "Ocurrió un error inesperado en el servidor."
        }
        ```
---

### Endpoint: `PUT /api/v1/resources/id`

* **Descripción:** Permite actualizar un recurso.
* **Autenticación:** Requiere token JWT válido. Permiso: `resources.create`.
* **Parámetros de Consulta (Route Parameters):**
    * `id` (obligatiro, int): Filtra recurso disponible por identificacion unico.
```
PUT /api/v1/resources/1
```
* **Request Body (JSON - Ejemplo):**
    ```json
    {
      "Id" : "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
      "StatusId" : 1,
      "Name" : "Decoraciones luz blanca",
      "Description" : "Mesas y sillas para exterior blanca.",
      "Price" : 230.99,
      "Quantity" : 109,
      "CategoryId" : 3,
      "LocationId" : 4,
      "AuthorizationType" : 1,
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
        * Si `Name` ya se encuentra registrada en la base de datos o excede el tamaño maximo permitido (200 careacteres).
        * Si `Description` excede el tamaño maximo permitido (2000 careacteres).
        ```json
        {
          "Status" : 400,
          "Errors": {
            "name": "El nombre supera los 200 caracteres."
          }
        }
        ```
    * `401 Unauthorized`: Si no se proporciona un token JWT o es inválido.
    * `403 Forbidden`: Si el usuario no tiene el permiso `resources.create`.
    * `409 Conflict`: 
      *  Si el nombre del esta duplicado en la misma ubicación.
        ```json
        {
          "Status" : 409,
          "Title" : "Conflicto al crear el recurso",
          "Detail" : "Ya existe un recurso con el nombre '{request.Name}' en la ubicación especificada."
        }
        ```
    * `500 Internal Server Error`: 
      * Para errores inesperados.
        ```json
        {
          "Status" : 500,
          "Title" : "Error inesperado al actualizar el recurso.",
          "Detail" : "Ocurrió un error inesperado en el servidor."
        }
        ```
---
### Endpoint: `DELETE /api/v1/resources/id`

* **Descripción:** Permite eliminar una categoria.
* **Autenticación:** Requiere token JWT válido. Permiso: `resources.update`.
* **Parámetros de Consulta (Route Parameters):**
    * `id` (obligatiro, int): Filtra recurso disponible por identificacion unico.
```
PUT /api/v1/resources/1
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
          "Detail" : "El ID del recurso debe ser valido."
        }
        ```
    * `401 Unauthorized`: Si no se proporciona un token JWT o es inválido.
    * `403 Forbidden`: Si el usuario no tiene el permiso `resource.delete`.
    * `500 Internal Server Error`: 
      * Para errores inesperados.
        ```json
        {
          "Status" : 500,
          "Title" : "Error inesperado al eliminar el recurso.",
          "Detail" : "Ocurrió un error inesperado en el servidor."
        }
        ```
---
