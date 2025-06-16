---
sidebar_position: 1
title: Flujo de Usuarios
---


## Consultar todos los administradores

**Descripción del Flujo:**

Este flujo describe el proceso para que el administrador principal consulte a todos los administradores secundarios.

1. **Accesso al dashboard de administracion**
   - El Administrador debe inciar sesion para poder acceder al panel.
   - La opcion para acceder al panel solo se muestra en el menu si la sesion es activa
2. **Mostrar al dashboard administrativo**
   - El adminitrador accede al dashboard con una vista practiva y resumida de los administradores secundarios
   - Visualiza la lista de los administradores secundarios.
3. **Acciones disponibles**
   - En la lista de administradoers cada uno permite al administrador acceder a los detalles de la misma.

**Diagrama**
```mermaid
graph TD;
    A[Inicio] --> B[Administrador accede a la aplicación]
    B --> C{¿Sesión activa?}
    C -->|No| D[Redirige a inicio de sesión]
    D --> B
    C -->|Sí| E[Muestra menú con opción de administración]
    E --> F[Selecciona opción: Panel de administración]
    F --> G[Muestra dashboard administrativo]
    G --> H[Visualiza lista de administradores secundarios]
    H --> I[Selecciona administrador para ver detalles]
    I --> J[Muestra información detallada del administrador]
    J --> K[Fin]
```
---
## Consultar el detalle del administrador secundario

**Descripción del Flujo:**

Este flujo describe el proceso para que el administrador principal consulte los detalles del perfil de los administradores secundarios.

1. **Accesso al dashboard de administracion**
   - El Administrador debe inciar sesion para poder acceder al panel.
   - La opcion para acceder al panel solo se muestra en el menu si la sesion es activa
2. **Mostrar el dashboard administrativo**
   - El adminitrador accede al panel administrativo con una vista practiva y resumida de los administradores secundarios.
   - Visualiza la lista de los administradores secundarios.
3. **Seleccionar a un administrador**
   - Al usar la accion de `detalle` se mostrara la pagina de detalle para el administrador seleccionada.
4. **Mostrar informacion**
   - Se mostrar toda la informacion relacionada con el administrador
   - Tambien se permitira cambiar el estado del administrador.
5. **Regresa al dashboard**
   - Al terminar de revisar los detalles del administrador se puede regresar al dashboard.

**Diagrama**
```mermaid
graph TD;
    A[Inicio] --> B[Administrador accede a la aplicación]
    B --> C{¿Sesión activa?}
    C -->|No| D[Redirige a inicio de sesión]
    D --> B
    C -->|Sí| E[Muestra menú con opción de administración]
    E --> F[Selecciona opción: Panel de administración]
    F --> G[Muestra dashboard con lista de administradores secundarios]
    G --> H[Selecciona acción: Ver detalle]
    H --> I[Muestra información del administrador seleccionado]
    I --> M[Regresa al dashboard]
    M --> N[Fin]

```
---

## modificar el estado del administrador

**Descripción del Flujo:**

Este flujo describe el proceso para que el administrador principal midifique el estado de los administradores secundarios.

1. **Accesso al dashboard de administracion**
   - El Administrador debe inciar sesion para poder acceder al panel.
   - La opcion para acceder al panel solo se muestra en el menu si la sesion es activa
2. **Mostrar el dashboard administrativo**
   - El adminitrador accede al panel administrativo con una vista practiva y resumida de los administradores secundarios.
   - Visualiza la lista de los administradores secundarios.
3. **Seleccionar a un administrador**
   - Al usar la accion de `detalle` se mostrara la pagina de detalle para el administrador seleccionada.
4. **Mostrar informacion**
   - Se mostrar toda la informacion relacionada con el administrador
   - Tambien se permitira cambiar el estado del administrador.
5. **Cambiar estado**
   - Al seleccionar cambiar el estado se puede poner en activo o inactivo el perfil de un administrador
      - Si se escoge Activo el perfil podra entrar a la aplicacion y realizar todas sus actividades.
      - Si se escoge Inactivo el perfil no tendra acceso a la aplicacion y no podra inciar sesion.
5. **Regresa al dashboard**
   - Al terminar de revisar los detalles del administrador se puede regresar al dashboard.

**Diagrama**
```mermaid
graph TD;
    A[Inicio] --> B[Administrador accede a la aplicación]
    B --> C{¿Sesión activa?}
    C -->|No| D[Redirige a inicio de sesión]
    D --> B
    C -->|Sí| E[Muestra menú con opción de administración]
    E --> F[Selecciona opción: Panel de administración]
    F --> G[Muestra dashboard con lista de administradores secundarios]
    G --> H[Selecciona acción: Ver detalle]
    H --> I[Muestra información del administrador seleccionado]
    I --> J[Selecciona opción: Cambiar estado]
    J --> K{¿Nuevo estado seleccionado?}
    K -->|Activo| L[Actualiza estado a Activo]
    K -->|Inactivo| M[Actualiza estado a Inactivo]
    L --> N[Regresa al dashboard]
    M --> N
    N --> O[Fin]
```