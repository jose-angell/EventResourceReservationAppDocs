---
sidebar_position: 3
title: 03-endpoints
---

# 🔌 Contrato API

> Archivo: `03-endpoints.md`

## 1. Endpoints
<!-- Tabla de rutas y métodos -->
| Método | Ruta                         | Descripción breve                      |
|--------|------------------------------|----------------------------------------|
| GET    | `/api/<recurso>`             | Lista recursos                         |
| POST   | `/api/<recurso>`             | Crea un nuevo recurso                  |
| …      | …                            | …                                      |

## 2. Request & Response
### Endpoint: METHOD `/api/<ruta>`
- **Request**  
```json
{
"<campo>": "<valor>",
…
}
```

-  **Response 200/201**
```json
{
"<campo>": "<valor>",
…
}
```
-  Códigos de error
    - 400 Bad Request: …
    - 401 Unauthorized: …
    - 404 Not Found: …
    - 409 Conflict: …