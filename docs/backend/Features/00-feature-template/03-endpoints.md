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
| `GET`       | `/api/v1/resources`                | Obtiene una lista de todos los recursos disponibles.     |
| `GET`       | `/api/v1/resources/{id}`           | Obtiene los detalles de un recurso específico por su ID. |
| `POST`      | `/api/v1/reservations`             | Crea una nueva reserva para un recurso.                  |
| `PUT`       | `/api/v1/reservations/{id}`        | Actualiza completamente una reserva existente.           |
| `DELETE`    | `/api/v1/reservations/{id}`        | Elimina una reserva específica por su ID.                |
| `PATCH`     | `/api/v1/reservations/{id}/status` | Actualiza parcialmente el estado de una reserva.         |
| `...`       | `...`                              | `...`                                                    |

---
## 2. Detalle de Endpoints (Request & Response)

Para cada endpoint, se especifican los detalles de la solicitud, los parámetros, el cuerpo de la respuesta esperada y los posibles códigos de error.


### Endpoint: `GET /api/v1/resources`
* **Descripción:** Permite obtener un listado de todos los recursos disponibles para reserva.
* **Autenticación:** Requiere token JWT válido. Permiso: `resources.read`.
* **Parámetros de Consulta (Query Parameters):**
    * `date` (opcional, string, formato `YYYY-MM-DD`): Filtra recursos disponibles para una fecha específica.
    * `type` (opcional, string): Filtra recursos por tipo (ej. `"room"`, `"equipment"`).
* **Request (Ejemplo):**

```
GET /api/v1/resources?date=2025-07-15&type=room
```

* **Response (HTTP 200 OK - Ejemplo de éxito):**
    ```json
    [
      {
        "id": "a1b2c3d4-e5f6-7890-1234-567890abcdef",
        "name": "Sala de Conferencia Alpha",
        "description": "Sala equipada para reuniones grandes.",
        "capacity": 20,
        "type": "room",
        "isAvailable": true
      },
      {
        "id": "f5e4d3c2-b1a0-9876-5432-10fedcba9876",
        "name": "Proyector Epson EB-X05",
        "description": "Proyector XGA con 3300 lúmenes.",
        "capacity": 1,
        "type": "equipment",
        "isAvailable": false
      }
    ]
    ```
* **Códigos de Error Posibles:**
    * `400 Bad Request`: Si el formato de `date` es inválido.
        ```json
        {
          "errorCode": "Validation.InvalidDateFormat",
          "message": "El formato de la fecha proporcionada es inválido. Use YYYY-MM-DD."
        }
        ```
    * `401 Unauthorized`: Si no se proporciona un token JWT o es inválido.
    * `403 Forbidden`: Si el usuario no tiene el permiso `resources.read`.
    * `500 Internal Server Error`: Para errores inesperados del servidor.

### Endpoint: `POST /api/v1/reservations`

* **Descripción:** Permite crear una nueva reserva de un recurso para un usuario.
* **Autenticación:** Requiere token JWT válido. Permiso: `reservations.create`.
* **Request Body (JSON - Ejemplo):**
    ```json
    {
      "resourceId": "a1b2c3d4-e5f6-7890-1234-567890abcdef",
      "userId": "1a2b3c4d-5e6f-7890-1234-567890abcdef",
      "startTime": "2025-07-15T10:00:00Z",
      "endTime": "2025-07-15T12:00:00Z"
    }
    ```
* **Response (HTTP 201 Created - Ejemplo de éxito):**
    ```json
    {
      "id": "xyz98765-4321-abcd-efgh-ijklmnopqrstuv",
      "resourceId": "a1b2c3d4-e5f6-7890-1234-567890abcdef",
      "userId": "1a2b3c4d-5e6f-7890-1234-567890abcdef",
      "startTime": "2025-07-15T10:00:00Z",
      "endTime": "2025-07-15T12:00:00Z",
      "status": "Pending",
      "createdAt": "2025-07-14T15:30:00Z"
    }
    ```
* **Códigos de Error Posibles:**
    * `400 Bad Request`:
        * Si `resourceId` o `userId` no son UUIDs válidos.
        * Si `startTime` o `endTime` están en formato incorrecto o el `endTime` es anterior a `startTime`.
        * Si el recurso no está disponible en el horario solicitado (validación de superposición).
        ```json
        {
          "errorCode": "Reservation.TimeConflict",
          "message": "El recurso no está disponible en el horario solicitado."
        }
        ```
    * `401 Unauthorized`: Si no se proporciona un token JWT o es inválido.
    * `403 Forbidden`: Si el usuario no tiene el permiso `reservations.create`.
    * `404 Not Found`: Si el `resourceId` o `userId` no existen.
    * `409 Conflict`: Si la reserva se solapa con otra existente para el mismo recurso.
    * `500 Internal Server Error`: Para errores inesperados.
