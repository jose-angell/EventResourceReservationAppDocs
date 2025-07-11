---
sidebar_position: 4
title: Usuario
---

## Entidad de Dominio: `User`
 >   Archivo: models-domains/User.md

Este documento define la entidad de dominio `User`, sus propiedades, su propósito dentro del sistema y sus relaciones clave con otras entidades. Sirve como la fuente principal de verdad para la estructura de datos relacionada con todos los usuarios registrados en el sistema.

---

### 1. Proposito de la Entidad

La entidad `User` representa la identidad central de cada persona que interactúa con el sistema, ya sea como cliente, administrador o gestor. Su propósito es centralizar toda la información esencial del usuario, como sus datos de contacto, credenciales de acceso y su rol dentro del sistema. Esto permite al sistema aplicar las reglas de negocio y los permisos adecuados, controlando qué funcionalidades son visibles y accesibles para cada tipo de usuario.

---

### 2. Propiedades y Atributos
A continuación, se detallan las propiedades de la entidad `User`, incluyendo su tipo de dato conceptual y una descripcion clara de su propósito.

| Propiedades | Tipo de Dato (conceptual) | Descripción |
|-------------|---------------------------|-------------|
| `Id`  | `UUID` (o `int` si es identidad generada por DB) | Identificador único  para cada usuario. |
|`Nombre` | `string` | Primer nombre del usuario. Considerar separar en `Nombre` y `SegundoNombre` si aplica.|
|`PrimerApellido` | `string` | Primer apellido del usuario.|
|`SegundoApellido`| `string`(opcional) | Segundo apellido del usuario (puede ser nulo).|
|`Correo` | `string` | Correo electrónico único registrado en el sistema. Utilizado para autenticación y comunicación.|
|`PasswordHash` | `string` | Hash seguro de la contraseña de acceso al sistema. Nunca almacenar la contraseña en texto plano.|
|`Telefono` | `string` |Número de teléfono de contacto del usuario.|
|`Rol` | `Enum` (`int`) | Indica el rol del usuario en el sistema. Los valores posibles se definen en el `enum UserRole` en el código (ej. `SuperAdmin=1`, `Gestor=2`, `Cliente=3`).|
| `FechaCreacion` | `DateTime` | Marca de tiempo que registra cuándo se creó el perfil del usuario. |
| `UltimoAcceso` | `DateTime`(opcional) | Fecha y hora del último inicio de sesión del usuario. |
| `Activo` | `bool` | Indica si la cuenta del usuario está activa o inactiva. |

---

### 3. Diagrama de Entidad-Relacion (ERD)

Este diagrama visualiza la estructura de la entidad `User` y sus relaciones clave con otras entidades en el modelo de dominio.

``` mermaid
erDiagram
    Location {
        UUID Id PK
        string DireccionCompleta
        string Ciudad
    }

    User {
        UUID Id PK
        string Nombre
        string PrimerApellido
        string SegundoApellido
        string Correo
        string PasswordHash
        string Telefono
        int Rol      
        datetime FechaCreacion
        datetime UltimoAcceso
        bool Activo
    }

    Location ||--o{ User : "ubicacion_predeterminada"
```