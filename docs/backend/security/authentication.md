

# Autenticacion

> **Archivo:** `authentication.md`

---

## Esquema de Autenticación (JWT Bearer)

El esquema de seguridad de la API se basa en la autenticación sin estado (*stateless*) mediante JSON Web Tokens (JWT).

1. Mecanismo: La API utiliza **JSON Web Tokens (JWT)** para la autenticación sin estado (stateless).
2. Protocolo: Se implementa el esquema **Bearer** (portador) donde el token se envía en el encabezado `Authorization`.
3. Implementación: Se utiliza **ASP.NET Core Identity** para la gestión de usuarios y contraseñas (*hashing*), y el *middleware* JWT Bearer para la validación del token.

---

## Configuracion del servidor

Esta sección describe los componentes clave que habilitan la seguridad en el *backend*.

1. **Clases de Identity**
- `ApplicationUser` y `ApplicationRole`: Tipos personalizados de Identity que usan una clave `Guid` y se mapean a las tablas `AspNetUsers` y `AspNetRoles`.
    - **Herencia**: `IdentityUser<Guid>` / `IdentityRole<Guid>`.

2. **Configuracion en** `Program.cs`

- **Servicios de Identity:** `builder.Services.AddIdentity<ApplicationUser, ApplicationRole>` con la configuración de la complejidad de la contraseña
- **Servicios JWT:** Configuración del middleware `AddJwtBearer` y sus parámetros de validación:
    - **Validacion Critica:** Se requiere que el Issuer, Audience, Lifetime y Signing Key sean validados.
    - **Secret Key:** Obtenida de appsettings.json.

3. **Servicio de Tokens** `(TokenService)`
    - **Responsabilidad:** Creación del JWT.
    - **Contenido del Token (Claims):** El token incluye la identidad necesaria para la autorización:
        * **ID de Usuario:** (`ClaimTypes.NameIdentifier`) 
        * **Email:** (`ClaimTypes.Email`) 
        * **Roles** (`ClaimTypes.Role`)

--- 

## Flujo de Autenticación Paso a Paso

| Paso | Acción del Cliente | Acción del Servidor (API) |
| :--- | :--- | :--- |
| **1. Solicitud** | Envía credenciales (Email/Password) al *endpoint* `POST /api/auth/login`. | El `AuthController` recibe el `LoginRequest`. |
| **2. Verificación** | N/A | El `UserManager` busca el usuario (`FindByEmailAsync`) y verifica la contraseña (`CheckPasswordAsync`). |
| **3. Generación** | N/A | Si la verificación es exitosa, el `TokenService` genera y firma un JWT utilizando la clave secreta del servidor. |
| **4. Respuesta** | Recibe la respuesta HTTP **200 OK**. | La API devuelve el **JWT** (junto con el ID de usuario y roles) dentro del cuerpo del `AuthResponse`. |

---

## Uso del Token (Acceso a Recursos)

Para acceder a métodos protegidos, el cliente debe incluir el token en cada solicitud.

- **Acceso a metodos protegidos**
    - **Método:** Todas las solicitudes a endpoints protegidos (`[Authorize]`) deben incluir el token.
    - **Encabezado:** El cliente debe añadir el encabezado `Authorization` con el prefijo "Bearer" y el token: 
        ```JSON
        Authorization: Bearer <El.Token.Completo.Aquí>
        ```
    - **Validación:** El middleware JWT Bearer intercepta el encabezado, valida la firma, verifica la expiración, y extrae los claims para poblar el objeto ``HttpContext.User`` (el `ClaimsPrincipal`).
        - Si la validación falla (token expirado, firma inválida), la respuesta es `HTTP 401 Unauthorized`.