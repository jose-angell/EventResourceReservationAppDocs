

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


---
## Especificación de Endpoints de Autenticación

Detalle de los endpoints disponibles para la gestión de acceso de usuarios.

1. **Registro `(POST /api/auth/register)`**

| Parámetro | Tipo | Descripción | Requerido |
| :--- | :--- | :--- | :--- |
| `email` | `string` | Correo electrónico único del usuario. | Sí |
| `password` | `string` | Contraseña del usuario (sujeta a las políticas de complejidad). | Sí |
| `firstName` | `string` | Primer nombre del usuario. | Sí |
| `lastName` | `string` | Apellido paterno. | Sí |
| `secondLastName` | `string` | Apellido materno (opcional en algunos sistemas). | Sí |

**Respuestas**

| Código | Descripción | Cuerpo de Respuesta (Body) | 
| :--- | :--- | :--- | 
| `200 OK` | Registro exitoso. Devuelve el token JWT para iniciar la sesión inmediatamente.| `AuthResponse` (incluye `Token`, `UserId`, `Roles`) |
| `400 Bad Request` | Datos inválidos o usuario ya existe.| Lista de errores de validación de Identity (e.g., contraseña débil, email ya registrado). |


2. **Login `(POST /api/auth/login)`**

Permite el inicio de sesión con credenciales válidas.

| Parámetro | Tipo | Descripción | Requerido |
| :--- | :--- | :--- | :--- |
| `email` | `string` |Correo electrónico del usuario. | Sí |
| `password` | `string` | Contraseña del usuario. | Sí |

**Respuestas**

| Código | Descripción | Cuerpo de Respuesta (Body) | 
| :--- | :--- | :--- | 
| `200 OK` | Inicio de sesión exitoso.| `AuthResponse` (incluye `JWT`, `UserId`, `Roles`) |
| `401 Unauthorized` | Credenciales inválidas (email o contraseña incorrectos).|`{ "message": "Credenciales inválidas." }` |


## Detalle del Uso y Estructura del JWT

Esta sección amplía cómo se usa el token en el cliente y sus propiedades internas.

1. **Estructura del Token (Claims Clave)**

El JWT generado es una cadena codificada que contiene la siguiente información de identidad:

| Claim | Mapeo C# | Descripción | 
| :--- | :--- | :--- | 
| `sub` | `ClaimTypes.NameIdentifier` | El ID único del usuario (tipo `Guid`). |
| `email` | `ClaimTypes.Email` | El correo electrónico del usuario. |
| `role` |`ClaimTypes.Role `| Uno o varios roles asignados al usuario (e.g., Client, Admin). |
| **`exp`** | **N/A** |Timestamp de expiración del token.|

2. **Inclusión en Peticiones Protegidas**

* **Método:** Todas las solicitudes a endpoints protegidos (`[Authorize]`) deben incluir el token.

* **Encabezado:** Se debe añadir el encabezado `Authorization` con el prefijo `Bearer` y el token:

```JSON
Authorization: Bearer <El.Token.Completo.Aquí>
```

3. **Política de Expiración**

* **Vigencia:** La validez del token se define en la configuración del servidor, bajo la clave `JwtSettings:TokenExpirationInMinutes` en `appsettings.json`.
* **Manejo:** Una vez expirado, el token resultará en una respuesta **HTTP 401 Unauthorized** y el usuario deberá volver a iniciar sesión para obtener un nuevo token.