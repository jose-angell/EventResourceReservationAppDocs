---
sidebar_position: 4
title: 04-security
---
# 🔒 Seguridad y validaciones

> Archivo: `04-security.md`

## 1. Roles y Permisos Requeridos
Aquí se especifican los roles de usuario que están autorizados para interactuar con esta feature y los permisos específicos asociados a cada operación.
    - `AdminPrincipal`: Permite la gestión completa de recursos y reservas (CRUD total). 
    - `Organizer`: Permite crear, leer y actualizar sus propias reservas, y consultar todos los recursos disponibles.

---

## 2.  Claims o Scopes JWT Requeridos
Se listan los claims (afirmaciones sobre la identidad del usuario) o scopes JWT (ámbitos de acceso) que el token de autenticación debe contener para permitir el acceso a las operaciones de esta feature. Estos definen granularmente lo que un usuario puede hacer.
    - `scope:<feature>.<acción>`
    - `scope:reservations.create`: Requerido para crear nuevas reservas.
    - `scope:reservations.read`: Requerido para consultar detalles de reservas (propias o generales, según el rol).
    - `scope:resources.read`: Requerido para consultar la lista de recursos disponibles.
    - `userId: <UUID del usuario>`: Claim para identificar al usuario que realiza la solicitud, usado en reglas de autorización por "ownership".

## 3. Validaciones de Entrada (Input Validations)
Esta sección describe las reglas de negocio y formato de datos que deben cumplirse para que una solicitud sea válida. Cualquier solicitud que no cumpla estas validaciones debe ser rechazada con un `HTTP 400 Bad Request`.
    - **Campos Obligatorios:** El campo `<NombreCampo>` es obligatorio y no puede ser nulo o vacío.
    - **Formato de Fechas/Horas:** Las fechas y horas (`StartTime`, `EndTime`) deben seguir el formato ISO 8601 (ej. `2025-07-15T10:00:00Z`) y representar un rango válido (EndTime > StartTime).
    - **Longitud de Cadenas:** El campo `<NombreCampo>` no debe exceder los 100 caracteres.
    - **Rangos Numéricos:** Los campos numéricos (`Capacity`, `Quantity`) deben estar dentro de un rango permitido (ej. `> 0` y `< 1000`).
    - **UUIDs Válidos:** Los campos `ResourceId` y `UserId` deben ser UUIDs válidos.
    - **Estado de Reserva:** El campo `Status` debe ser uno de los valores predefinidos (`Pending`, `Confirmed`, `Cancelled`).

## 4. Reglas de Autorización Específicas

Aquí se detallan las **reglas de negocio específicas** que determinan quién puede realizar qué acción sobre los datos, más allá de los permisos básicos del claim. Esto a menudo implica verificar la relación del usuario con el recurso (`ownership`).
    - **SCreación de Reservas:** Cualquier usuario con el permiso reservations.create puede crear una reserva.
    - **Lectura de Reservas:**
        - Un `Organizer` o `User` solo puede leer sus propias reservas.
        - Un `AdminPrincipal` puede leer todas las reservas en el sistema.
    - **Modificación/Eliminación de Reservas:**
        - Solo el **propietario** de la reserva (el `UserId` de la reserva coincide con el `userId` del token) puede modificarla o eliminarla.
        - Un `AdminPrincipal` también puede modificar o eliminar cualquier reserva.
    - **Consistencia de Datos:** No se puede reservar un recurso si ya está ocupado en el mismo rango de tiempo.

