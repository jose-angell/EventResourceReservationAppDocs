---
sidebar_position: 4
title: Setup
---

# Event Resource Reservation App Backend
 > Backend de aplicacion de reserva de recursos para eventos

---

Esta aplicación permite a los usuarios reservar mobiliario, equipo de iluminación y otros recursos necesarios para realizar toda clase de eventos de forma rápida y eficiente, optimizando la gestión de disponibilidad para los administradores. Está dirigida a clientes que necesiten un proveedor integral para sus eventos y permite a los organizadores optimizar recursos y mejorar la experiencia del cliente final. 

---

## Características

- CRUD completo (Crear, Leer, Actualizar, Eliminar) para recursos, reservas, etc.
- Gestión de categorías de recusor.
- Autenticación y autorización de usuarios.
- Pasarela de pagos.


## Tecnologías Utilizadas

- **Backend:** .NET 8
- **Base de Datos:** PostgreSQL
- **ORM:** Entity Framework Core
- **Contenerización:** Docker, Docker Compose
- **Autenticación:** JWT (JSON Web Tokens) y ASP.NET Core Identity
- **Documentación API:** Swagger/OpenAPI

---

## Requisitos Previos

Asegúrate de tener instalado lo siguiente:

- **.NET SDK** (versión 8.0 o superior): [Descargar .NET](https://dotnet.microsoft.com/download)
- **Docker Desktop**: [Descargar Docker](https://www.docker.com/products/docker-desktop/)
- **Un editor de código o IDE** (recomendado Visual Studio 2022 o VS Code).
- Opcional: **PostgreSQL** instalado directamente si no usas Docker para la DB.

---
 
### Clonar el Repositorio

1. Abre tu terminal o línea de comandos.
2. Navega hasta el directorio donde deseas guardar el proyecto.
3. Clona el repositorio:
    ```bash
    git clone [https://github.com/jose-angell/EventResourceReservationAppBackend.git](https://github.com/jose-angell/EventResourceReservationAppBackend.git)
    ```
4. Navega al directorio del proyecto clonado:
    ```bash
    cd EventResourceReservationAppBackend
    ```

---

### Configurar la Base de Datos (PostgreSQL)

Este proyecto utiliza Docker Compose para levantar y gestionar la base de datos PostgreSQL, lo que simplifica su configuración local.

1.  Asegúrate de tener **Docker Desktop** en ejecución y de que sus servicios estén funcionando correctamente.
2.  Desde la **raíz del proyecto** (donde se encuentra `docker-compose.yml`), ejecuta el siguiente comando para iniciar el contenedor de PostgreSQL en segundo plano:
    ```bash
    docker-compose up -d
    ```
    Esto creará y levantará el contenedor de base de datos.
    * **Verificación:** Puedes ejecutar `docker ps` para confirmar que el contenedor de `postgres` esté corriendo.

3.  **Aplicar Migraciones de Entity Framework Core:**
    Una vez que el contenedor de la base de datos esté levantado y disponible (dale unos segundos después del `docker-compose up`), necesitas aplicar las migraciones de Entity Framework Core para crear el esquema de la base de datos y/o sembrar datos iniciales.

    Navega a la carpeta de tu proyecto de backend (donde se encuentra el archivo `.csproj` principal de tu API, por ejemplo, `src/EventResourceReservationAppBackend.Api`).
    ```bash
    cd src/EventResourceReservationAppBackend.Api # Ajusta esta ruta si tu .csproj está en otro lugar
    ```
    Luego, ejecuta el comando de migraciones:
    ```bash
    dotnet ef database update
    ```
    Si tienes múltiples proyectos en tu solución o si el comando anterior no funciona, podrías necesitar especificar el proyecto de inicio o el contexto de la base de datos:
    ```bash
    dotnet ef database update --project EventResourceReservationAppBackend.Api --startup-project EventResourceReservationAppBackend.Api # Reemplaza con los nombres reales de tu proyecto
    ```
    *Este comando conectará tu aplicación .NET local al contenedor de PostgreSQL y aplicará las migraciones pendientes, creando la base de datos y sus tablas.*

---

### Configuración de la API

La cadena de conexión a la base de datos se configura en el archivo `appsettings.Development.json` (para desarrollo local) y `appsettings.json` (para producción).

Verifica que la sección `ConnectionStrings` tenga la siguiente estructura (debería coincidir con tu `docker-compose.yml`):

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Host=localhost;Port=5432;Database=nombre_de_tu_db;Username=tu_usuario;Password=tu_contraseña"
  },
  // ...otras configuraciones
}
```
Asegúrate de que Host sea `localhost` si la DB corre en tu `máquina` y `Port`, `Database`, `Username`, `Password` coincidan con lo configurado en tu `docker-compose.yml`.

---

### Ejecutar la API

Una vez configurada la base de datos y la cadena de conexión:

1. Abre tu terminal en la raíz del proyecto (donde está el archivo `.sln` o el `.csproj` de tu API).
2. Restaura las dependencias de NuGet:
    ```bash
    dotnet restore
    ```
3. Compila el proyecto:
    ```bash
    dotnet build
    ```
4. Ejecuta la API:
    ```bash
    dotnet run --project EventResourceReservationAppBackend
    ```
La API se iniciará y estará disponible en `http://localhost:XXXX`.

Alternativamente, puedes ejecutar el proyecto usando Visual Studio: abre la solución (.sln) y presiona `F5` o el botón "Ejecutar".

---
## Uso de la API

Una vez que la API esté en ejecución, puedes acceder a la documentación interactiva de Swagger para probar los endpoints:

- **Swagger UI:** `http://localhost:XXXX/swagger` (reemplaza XXXX con tu puerto)

Aquí algunos ejemplos de endpoints comunes:
- `GET /api/recursos` - Obtener todos los recursos.
- `POST /api/productos` - Crear un nuevo recursos.
- `GET /api/usuarios/{id}` - Obtener un usuario por ID.


---
## Contacto

Si tienes alguna pregunta o sugerencia, no dudes en contactarme:

- **Tu Nombre**
- **Email:** gallardocordovajoseangel@gmail.com
- **LinkedIn:** [Jose Angel](https://www.linkedin.com/in/jose-angel-gallardo-cordova-05a347365)
- **GitHub:** [jose-angell](https://github.com/jose-angell)