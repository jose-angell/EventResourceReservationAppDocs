# Autorización

> **Archivo:** `authorization.md`

---

La **Autorización** es el proceso de determinar **qué acciones puede realizar** un usuario autenticado. Una vez que la identidad de un usuario es confirmada (Autenticación), la Autorización define sus permisos en el sistema.

## Contenido Clave

* **Esquema Principal:** La API utiliza la **Autorización Basada en Roles (Role-Based Access Control - RBAC)**. Los permisos son otorgados a través de los roles asignados al usuario en el JWT.
* **Roles Definidos:** Lista los roles de seguridad sembrados en la base de datos y su alcance funcional:

| Rol | Descripción | Permisos Clave |
| :--- | :--- | :--- |
| **Admin** | Control total sobre la API y la seguridad. | **CRUD** en todas las entidades (incluyendo gestión de usuarios y roles). |
| **Manager** | Gestión operativa. | **CRUD** en Recursos y Reservas (no puede gestionar usuarios). |
| **Client** | Usuario final. | Crear y leer sus propias Reservas. |

* **Implementación en .NET Core:** La autorización se implementa mediante el atributo `[Authorize]` que se aplica a *controllers* y métodos individuales de la API.
    * **Acceso Autenticado Básico:** El atributo `[Authorize]` sin parámetros exige que el usuario esté autenticado.
    * **Restricción por Rol:** Se utiliza el argumento `Roles` para restringir el acceso a roles específicos:
        ```csharp
        [Authorize(Roles = "Admin,Manager")]
        public async Task<IActionResult> CreateResource()
        {
            // ... solo accesible si el usuario es Admin o Manager
        }
        ```
    * **Ejemplo:** Un *endpoint* para crear un recurso solo puede ser accedido por usuarios con el rol **Admin** o **Manager**.