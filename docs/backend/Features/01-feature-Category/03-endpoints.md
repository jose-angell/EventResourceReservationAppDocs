---
sidebar_position: 3
title: 03-endpoints
---

# 🔌 Contrato API

> Archivo: `03-endpoints.md`

Este documento detalla los **endpoints** expuestos por el backend para la feature `<Nombre de la Feature>`. Incluye el método HTTP, la ruta, una descripción, así como ejemplos completos de solicitudes (requests) y respuestas (responses) para cada operación, incluyendo posibles códigos de error.

---

## 1. Resumen de Endpoints
Esta tabla proporciona una vista rápida de todos los endpoints relacionados con esta feature.

| Método HTTP | Ruta de la API                     | Descripción Breve                                        |
| :---------- | :--------------------------------- | :------------------------------------------------------- |
| `GET`       | `/api/v1/categories`                | Obtiene una lista de todas las categorias disponibles.     |
| `GET`       | `/api/v1/categories/{id}`           | Obtiene los detalles de una categoria específica por su ID. |
| `GET`       | `/api/v1/categories/list`           | Obtiene una lista con solo el id y el nombre de todas las categorias disponibles. |
| `POST`      | `/api/v1/categories`             | Crea una nueva categoria para un recurso.                  |
| `PUT`       | `/api/v1/categories/{id}`        | Actualiza completamente una categoria existente.           |
| `DELETE`    | `/api/v1/categories/{id}`        | Elimina una categoria específica por su ID.                |

---
## 2. Detalle de Endpoints (Request & Response)

Para cada endpoint, se especifican los detalles de la solicitud, los parámetros, el cuerpo de la respuesta esperada y los posibles códigos de error.


### Endpoint: `GET /api/v1/categories`
* **Descripción:** Permite obtener un listado de todas las categorias disponibles para los recursos.
* **Autenticación:** Requiere token JWT válido. Permiso: `categories.read`.
* **Parámetros de Consulta (Query Parameters):**
    * `NameFilter` (opcional, string): Filtra categorias disponibles por coincidencia de caracteres en el nombre.
    * `CreatedByUserIdFilter` (opcional, Guid): Filtra por usuario de creacion.
    * `OrderBy` (opcional, string): ordena categoria ascendete por nombre o fecha de creacion (ej. `"Name_asc"`, `"CreatedAt_asc"`).
* **Request (Ejemplo):**

```
GET /api/v1/categories
```

* **Response (HTTP 200 OK - Ejemplo de éxito):**
    ```json
    [
      {
        "Id": 1,
        "Name": "Cristaleria",
        "Description": "copas para vino y bebidas.",
        "CreateAt": "2025-08-03",
        "CreatedByUserId": "f5e4d3c2-b1a0-9876-5432-10fedcba9876"
      },
      {
        "Id": 2,
        "Name": "Iluminacion",
        "Description": "Decoraciones y luz para exterior.",
        "CreateAt": "2025-08-23",
        "CreatedByUserId": "f5e4d3c2-b1a0-9876-5432-10fedcba9876"
      }
    ]
    ```
* **Códigos de Error Posibles:**
    * `400 Bad Request`: Si la operacion falla.
        ```json
        {
          "Status": "Categories.BadRequest",
          "Title": "Error en la operación",
          "Detail": "Ocurrio un error al consultar la informacion."
        }
        ```
    * `401 Unauthorized`: Si no se proporciona un token JWT o es inválido.
    * `403 Forbidden`: Si el usuario no tiene el permiso `categories.read`.
    * `500 Internal Server Error`: Para errores inesperados del servidor.

### Endpoint: `POST /api/v1/categories`

* **Descripción:** Permite crear una nueva categoria.
* **Autenticación:** Requiere token JWT válido. Permiso: `categories.create`.
* **Request Body (JSON - Ejemplo):**
    ```json
    {
      "Name": "Cristaleria",
      "Description": "copas para vino y bebidas.",
      "CreatedByUserId": "1a2b3c4d-5e6f-7890-1234-567890abcdef"
    }
    ```
* **Response (HTTP 201 Created - Ejemplo de éxito):**
    ```json
    {
      "id": "1",
      "Name": "Cristaleria",
      "Description": "copas para vino y bebidas.",
      "CreatedByUserId": "1a2b3c4d-5e6f-7890-1234-567890abcdef",
      "createdAt": "2025-07-14T15:30:00Z"
    }
    ```
* **Códigos de Error Posibles:**
    * `400 Bad Request`:
        * Si `CreatedByUserId` no es UUID válido.
        * Si `Name` ya se encuentra registrada en la base de datos o excede el tamaño maximo permitido (100 careacteres).
        * Si `Description` excede el tamaño maximo permitido (5000 careacteres).
        ```json
        {
          "errorCode": "Categories.Name",
          "message": "El nombre ya se encuentra en uso"
        }
        ```
    * `401 Unauthorized`: Si no se proporciona un token JWT o es inválido.
    * `403 Forbidden`: Si el usuario no tiene el permiso `reservations.create`.
    * `404 Not Found`: Si el `CreatedByUserId` no existe.
    * `409 Conflict`: Si el nombre de la categoria solapa con otra existente para el la base de datos.
    * `500 Internal Server Error`: Para errores inesperados.
