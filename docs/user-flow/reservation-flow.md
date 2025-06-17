---
sidebar_position: 1
title: Flujo de Reservas
---


## Consulta de todas las Reservas

**Descripción del Flujo**

Este flujo describe el proceso para consultar las reservas. Existen dos rutas para realizar la consulta, la que usa el administrador utilizando su dashboard de administracicon y la del cliente desde su respectivo dashboard enfocado en su actividad en la aplicacion.

### El Administrador Consulta las reservas
1. **Accesso al dashboard de administrador**
   - El Administrador debe inciar sesion para poder acceder al panel.
   - La opcion para acceder al panel solo se muestra en el menu si la sesion es activa
2. **Mostrar el panel administrativo**
   - El adminitrador accede al panel administrativo con una vista practiva y resumida de las reservas.
   - Visualiza la lista de reservas.
3. **Acciones disponibles**
   - En la lista de reservas cada una permite al administrador acceder a los detalles de la misma.

**Diagrama**
```mermaid
graph TD;
    A[Inicio] --> B[Administrador accede a la aplicación]
    B --> C{¿Sesión activa?}
    C -->|No| D[Redirige a inicio de sesión]
    D --> B
    C -->|Sí| E[Muestra menú con opción de administración]
    E --> F[Selecciona opción: Panel de reservas]
    F --> G[Muestra dashboard con lista de reservas]
    G --> J[Fin]

```

### El Cliente Consulta las reservas
1. **Accesso al dashboard de cliente**
   - El cliente debe inciar sesion para poder acceder al panel.
   - La opcion para acceder al panel solo se muestra en el menu si la sesion es activa
2. **Visualizacion del dashboard de cliente**
   - Inicalmente se muestra todas las reservas realizadas, inicando por la ultima.
   - El usuario podra filtrar las reservas por estado, fecha, tipo de recurso.
3. **Acciones para cada reserva**
   - Cada reserva mostrar las siguientes acciones permitidas para el usuario
      - Editar: al seleccionarlo se mostrara el formulario con los datos que se permiten editar.
      - Eliminar: Al seleccionarlo, este mostrara un modal para confirmar la accion de eliminacion.
      - Detalle: Se redirigira al usuario a la pangina con los detalles de la reseva.

**Diagrama**
```mermaid
graph TD;
    A[Inicio] --> B[Cliente accede a la aplicación]
    B --> C{¿Sesión activa?}
    C -->|No| D[Redirige a inicio de sesión]
    D --> B
    C -->|Sí| E[Muestra menú con opción de reservas]
    E --> F[Selecciona opción: Panel de reservas]
    F --> G[Muestra dashboard con lista de reservas ]
    G --> H[Cliente aplica filtros]
    H --> I[Selecciona una reserva]

    I --> J{¿Acción seleccionada?}
    J -->|Editar| K[Muestra formulario editable con datos permitidos]
    J -->|Eliminar| L[Muestra modal de confirmación de eliminación]
    J -->|Detalle| M[Redirige a la página de detalle de la reserva]

    K --> N[Regresa al dashboard]
    L --> N
    M --> N
    N --> O[Fin]

```
---
## Cosultar el detalle de Reserva

**Descripción del Flujo**

Este flujo describe el proceso para realizar la consulta al detalle de una reserva en especifico en el sistema.

### El Administrador Consulta el detalle de una reserva
1. **Accesso al dashboard de administracion**
   - El Administrador debe inciar sesion para poder acceder al panel.
   - La opcion para acceder al panel solo se muestra en el menu si la sesion es activa
2. **Mostrar al dashboard administrativo**
   - El adminitrador accede al panel administrativo con una vista practiva y resumida de las reservas.
   - Visualiza la lista de reservas.
3. **Seleccionar una reserva**
   - Al usar la accion de `detalle` se mostrara la pagina de detalle para la reserva seleccionada.
4. **Mostrar informacion**
   - Se mostrar toda la informacion relacionada con la reserva, incluyendo recursos, catidad, costos, imagenes, estado, etc.
   - Tambien se permitira cambiar el estado de la reserva.
5. **Regresa al dashboard**
   - Al terminar de revisar los detalles de la reserva, se puede regresar a la lista de reservas.

**Diagrama**
```mermaid
graph TD;
    A[Inicio] --> B[Administrador accede a la aplicación]
    B --> C{¿Sesión activa?}
    C -->|No| D[Redirige a inicio de sesión]
    D --> B
    C -->|Sí| E[Muestra menú con opción de administración]
    E --> F[Selecciona opción: Panel de reservas]
    F --> G[Muestra dashboard con lista de reservas]
    G --> H[Selecciona acción: Ver detalle]
    H --> I[Muestra información completa de la reserva]
    I --> J{¿Desea cambiar estado de la reserva?}
    J -->|Sí| K[Actualiza estado de la reserva]
    J -->|No| L[Sin cambios]
    K --> M[Regresa al dashboard]
    L --> M
    M --> N[Fin]

```

### El Cliente Consulta el detalle de una reserva
1. **Accesso al dashboard de cliente**
   - El cliente debe inciar sesion para poder acceder al panel.
   - La opcion para acceder al panel solo se muestra en el menu si la sesion es activa
2. **Visualizacion del dashboard de cliente**
   - Inicalmente se muestra todas las reservas realizadas, inicando por la ultima.
   - El usuario podra filtrar las reservas por estado, fecha, tipo de recurso.
3. **Seleccionar una reserva**
   - Al usar la accion de `detalle` se mostrara la pagina de detalle para la reserva seleccionada.
4. **Mostrar informacion**
   - Se mostrar toda la informacion relacionada con la reserva, incluyendo recursos, catidad, costos, imagenes, estado, etc.
   - Tambien se permitira pasar a la pantalla de edicion utilizando un boton.
5. **Regresa al dashboard**
   - Al terminar de revisar los detalles de la reserva, se puede regresar a la lista de reservas.

**Diagrama**
```mermaid
graph TD;
    A[Inicio] --> B[Cliente accede a la aplicación]
    B --> C{¿Sesión activa?}
    C -->|No| D[Redirige a inicio de sesión]
    D --> B
    C -->|Sí| E[Muestra menú con opción de reservas]
    E --> F[Selecciona opción: Panel de reservas]
    F --> G[Muestra dashboard con lista de reservas]
    G --> I[Selecciona una reserva]
    I --> J[Muestra detalle de la reserva ]
    J --> K{¿Desea editar la reserva?}
    K -->|Sí| L[Redirige a pantalla de edición]
    K -->|No| M[Regresa al dashboard]
    L --> M
    M --> N[Fin]

```
---

## Creacion de Reservas

### Crear una reserva individual
**Descripción del Flujo**

Este flujo describe el proceso para que un cliente realice una reserva en el sistema de un unico recurso.

1. **Visualización de la Página Principal**  
   - La página se muestra sin necesidad de iniciar sesión.
   - La pantalla muestra una carga incial de todos los recursos disponibles para ese dia.
   - El cliente ve la opción de buscar el recurso en el inventario disponible.

2. **Búsqueda y Consulta de Disponibilidad**  
   - El cliente selecciona la fecha (obligatorio), la hora (opcional) y el tipo de recurso (opcional).
     - Si hay recursos disponibles, continúa el flujo.
     - Si no, se muestra un mensaje que invita a elegir otra fecha o recurso.

3. **Selección del Recurso y Configuración de la Reserva**  
   - El cliente elige el recurso específico y ajusta parámetros como la cantidad o la ubicación.

4. **Confirmación de la Reserva**  
   - El cliente revisa la información y confirma los datos ingresados.
     - Si el cliente tiene sesión activa, continúa el flujo.
     - Si no tiene sesión, se le solicita quen iicie sesión o se registre (se guardan los datos de la configuracion de la reserva temporalmente).

5. **Envío de la Solicitud de Reserva**  
   - El sistema registra la solicitud.
     - Si el recurso tiene configuración de autorización automática, la reserva se registra con un estado de “autorizado” y se envía una notificación al administrador.
     - Si el recurso requiere autorización manual, la reserva se registra con un estado “pendiente” y se envía para aprobación.

6. **Notificación de Resultado**  
   - El cliente recibe una notificación confirmando que la reserva ha sido registrada, incluyendo los posibles estados de pendiente, aprobación o rechazo.

7. **Regresa a la pantalla principal**
   - El cliente regresa a la pagina principal.


``` mermaid
graph TD;
    A[Inicio] --> B[Visualización de la Página Principal]
    B --> C[Muestra recursos disponibles para el día]
    C --> D[Cliente busca en el inventario]

    D --> E[Selecciona fecha, hora y tipo de recurso]
    E --> F{¿Hay recursos disponibles?}
    F -->|No| G[Muestra mensaje de error o sugiere otra fecha]
    G --> D
    F -->|Sí| H[Selecciona recurso y ajusta parámetros]

    H --> I[Confirma la reserva]
    I --> J{¿Cliente tiene sesión activa?}
    J -->|No| K[Guarda configuración temporalmente]
    K --> L[Solicita inicio de sesión o registro]
    L --> M[Envía solicitud de reserva]
    J -->|Sí| M

    M --> N{¿Autorización automática?}
    N -->|Sí| O[Reserva autorizada, notifica al administrador]
    N -->|No| P[Reserva pendiente, espera aprobación]

    O --> Q[Notifica al cliente]
    P --> Q
    Q --> R[Regresa a la pantalla principal]
    R --> S[Fin]

```

### Crear una reserva desde el carrito de compras

**Descripción del Flujo:**

Este flujo describe el proceso para que un cliente realice una reserva de los recursos, incluyendo el acceso al carrtio y la edicion del mismo.


1. **Visualización de la Página Principal**  
   - La página se muestra sin necesidad de iniciar sesión.
   - La pantalla muestra una carga incial de todos los recursos disponibles para ese dia.
   - El cliente ve la opción de buscar el recurso en el inventario disponible.
2. **Búsqueda y Consulta de Disponibilidad**  
   - El cliente selecciona la fecha (obligatorio), la hora (opcional) y el tipo de recurso (opcional).
     - Si hay recursos disponibles, continúa el flujo.
     - Si no, se muestra un mensaje que invita a elegir otra fecha o recurso.
3. **Selección del Recurso y Configuración de la Reserva**  
   - El cliente elige el recurso específico y ajusta parámetros como la cantidad o la ubicación.
4. **Accion de Agreega a carrito**  
   - El cliente revisa la información y confirma los datos ingresados.
     - Si el cliente tiene sesión activa, continúa el flujo.
     - Si no tiene sesión, se le solicita quen iicie sesión o se registre (se guardan los datos de la configuracion de la reserva temporalmente).
5. **Envio a carrito** 
   - Despues de configurar las especificaciones del recurso a reservar se puede agregar al carrito, para que se reserve en grupo
6. **Notificación de Resultado**  
   - El cliente recibe una notificación confirmando que la reserva ha sido agregada al carrito.
7. **Regresa a la pantalla principal**
   - El cliente regresa a la pagina principal.
8. **Acceso al carrito**
   -El carrito se muestra en la pagina de inicio, pero solo se puede acceder si la sesion esta activa
9. **Mostrar Carrito** 
   - Al entrar en la pagina de carrito de compra se mostrar la lista de recursos guardados y las cantidades (si un recurso ya no tiene la cantidad indicada en el carrito disponible se mostrar un mensaje al susario para que lo ajuste)
   - cada uno tendra la opcion para eliminar o cambiar la cantidad, asi como agregar algun comentario
10. **Configuración de la Reserva**  
   - El cliente elige el recurso específico y ajusta parámetros como la cantidad o la ubicación.
11. **Validacion de campos y envio de reserva**
   - Se validan los campos requeridos y la disponibilidad de los recuros al momento de enviar la solicitud de reserva
12. **Notificación de Resultado**  
   - El cliente recibe una notificación confirmando que la reserva ha sido registrada, incluyendo los posibles estados de pendiente, aprobación o rechazo.
13. **Baciar el carrito**
   - Al crear la reserva con los recursos del carrito, este eliminina todos los recurso asociados al carrito del cliente.
14. **Regresa a la pantalla principal**
   - El cliente regresa a la pagina principal.

**Diagrama**
```mermaid
graph TD;
    A[Inicio] --> B[Visualización de la Página Principal]
    B --> C[Muestra recursos disponibles para el día]
    C --> D[Cliente busca en el inventario]
    D --> E[Selecciona fecha, hora y tipo de recurso]
    E --> F{¿Hay recursos disponibles?}
    F -->|No| G[Muestra mensaje: elige otra fecha o recurso]
    G --> D
    F -->|Sí| H[Selecciona recurso y ajusta parámetros]
    H --> I[Confirma configuración del recurso]
    I --> J{¿Sesión activa?}
    J -->|No| K[Solicita inicio de sesión o registro]
    K --> L[Guarda configuración temporalmente]
    L --> M[Agrega recurso al carrito]
    J -->|Sí| M
    M --> N[Notifica al cliente que se agregó al carrito]
    N --> O[Regresa a la página principal]
    O --> P[Cliente accede al carrito]
    P --> Q[Muestra lista de recursos en el carrito]
    Q --> R{¿Cantidad disponible?}
    R -->|No| S[Muestra mensaje para ajustar cantidad]
    S --> Q
    R -->|Sí| T[Cliente edita cantidad o elimina recurso]
    T --> U[Cliente confirma configuración final]
    U --> V[Valida campos y disponibilidad]
    V --> W[Envía solicitud de reserva]
    W --> X[Notifica al cliente el resultado]
    X --> Y[Vacía el carrito del cliente]
    Y --> Z[Regresa a la página principal]
    Z --> AA[Fin]


```
---
## Editar una Reserva

**Descripción del Flujo:**

Este flujo describe el proceso para que un cliente realice una edicion de la reserva en el sistema.

1. **Acceso a la pangina de edicion**
   - Existe dos forma de acceder a la edicion de una reserva
      - Seleccionando la accion en el listado de reservas.
      - Desde el detalle de la reserva.
2. **Visualizacion del formulario de edicion**
   - Dentro de cada campo del formulario se mostrara la informacion guardada.
3. **Confirmar o cancelar Edicion** 
   - En caso de editar algun campo del formaulario se puede guardar los cambio en la base de datos.
   - Si se cancela la edicion solo se regresa a la pantalla de origen, ya sea la lista de reservas o la pantalla de detalle
4. **Notificación de Resultado**  
   - El cliente recibe una notificación confirmando que la reserva ha sido editada.
   - El administrador recibe una notificacion sobre el cambio de la reserva.
5. **Regresa a la pantalla de origen**
   - El cliente regresa a la pagina de origen, que podria se la lista de reservas o la pantalla de detalle.

**Diagrama**
```mermaid
graph TD;
    A[Inicio] --> B{¿Desde dónde accede el cliente?}
    B -->|Desde lista de reservas| C[Selecciona acción: Editar]
    B -->|Desde detalle de reserva| D[Selecciona botón: Editar]
    C --> E[Visualiza formulario con datos actuales]
    D --> E

    E --> F{¿Cliente edita algún campo?}
    F -->|No| G[Selecciona cancelar edición]
    G --> H[Regresa a pantalla de origen]
    F -->|Sí| I[Selecciona guardar cambios]
    I --> J[Actualiza reserva en la base de datos]
    J --> K[Notifica al cliente]
    K --> L[Notifica al administrador]
    L --> H
    H --> M[Fin]

```

---
## Cancelar una Reserva

**Descripción del Flujo:**

Este flujo describe el proceso para que un cliente realice una cancelacion de una reserva en especifico en el sistema.

1. **Visualizacion del dashboard de cliente**
   - Inicalmente se muestra todas las reservas realizadas, inicando por la ultima.
2. **Busqueda de la reserva**
   - El cliente buscala reserva que quiere eliminar.
   - Puede utilizar los filtro o buscarla manualmente.
3. **Cancelacion de reserva**
   - En la reserva excite una accion para cancelar la reserva
4. **Mostrar mensaje de confirmacion**
   - Se mostrar un mensaje para que el usuario confirme si desea cancelar la reserva
      - Si confirma la cancelacion esta se reflejara en la base de datos.
      - Si cancela la accion el mensaje solo se cerrara y no se guardara ningun cambio.
5. **Notificacion de cambios**
   - El sistema notifica al administrador de la cancelacion de la reserva.
   - Tambien se liberaran los recursos.
6. **Ocultar mensaje**
   - Se cerrar el mensaje y se mostrar de nuevo la lista de reservas

**Diagrama**
```mermaid
graph TD;
    A[Inicio] --> B[Cliente accede a la aplicación]
    B --> C{¿Sesión activa?}
    C -->|No| D[Redirige a inicio de sesión]
    D --> B
    C -->|Sí| E[Muestra dashboard con lista de reservas]
    E --> F[Cliente busca la reserva ]
    F --> G[Selecciona acción: Cancelar reserva]
    G --> H[Muestra mensaje de confirmación]
    H --> I{¿Confirma la cancelación?}
    I -->|No| J[Cierra mensaje sin cambios]
    I -->|Sí| K[Actualiza estado de la reserva a cancelada]
    K --> L[Libera recursos asociados]
    L --> M[Notifica al administrador]
    J --> N[Muestra nuevamente la lista de reservas]
    M --> N
    N --> O[Fin]

```

---
## Pagar una Reserva

**Descripción del Flujo:**

Este flujo describe el proceso para que un cliente realice el pago de una reserva en especifico en el sistema.

1. **Visualizacion del dashboard de cliente**
   - Inicalmente se muestra todas las reservas realizadas, inicando por la ultima.
2. **Busqueda de la reserva**
   - El cliente buscala reserva que quiere eliminar.
   - Puede utilizar los filtro o buscarla manualmente.
3. **Seleccionar una reserva**
   - Al usar la accion de `detalle` se mostrara la pagina de detalle para la reserva seleccionada.
4. **Seleccionar la accion de pago**
   - Dentro de la pantalla de detalle se muestra la accion de pagar.
      - Se llevara al usuario a la pasarela de pago.
5. **Notificación de Resultado**  
   - El cliente recibe una notificación confirmando que proceso de pago ha terminado, incluyendo los posibles estados de aprobación o rechazo.
   - Se notificara el pago al administrador.
6. **Regresa a la pantalla principal**
   - El cliente regresa a la pagina principal.

**Diagrama**
```mermaid
graph TD;
    A[Inicio] --> B[Cliente accede a la aplicación]
    B --> C{¿Sesión activa?}
    C -->|No| D[Redirige a inicio de sesión]
    D --> B
    C -->|Sí| E[Muestra dashboard con lista de reservas]
    E --> F[Cliente busca la reserva ]
    F --> G[Selecciona acción: Ver detalle]
    G --> H[Muestra detalle de la reserva]
    H --> I[Selecciona acción: Pagar]
    I --> J[Redirige a pasarela de pago]
    J --> K{¿Pago aprobado?}
    K -->|Sí| L[Notifica al cliente y al administrador]
    K -->|No| M[Notifica rechazo al cliente]
    L --> N[Regresa a la pantalla principal]
    M --> N
    N --> O[Fin]

```

---
## Modificar estado de la Reserva

**Descripción del Flujo:**

Este flujo describe el proceso para que un administrador realice el cambio de estado de una reserva en especifico en el sistema.

1. **Visualizacion del dashboard de administracion**
   - inicalmente se muestra todas las reservas realizadas por todos los clientes, inicando por la ultima.
   - El usuario podra filtrar las reservas por estado, fecha, tipo de recurso.
2. **Seleccionar una reserva**
   - Al usar la accion de `detalle` se mostrara la pagina de detalle para la reserva seleccionada.
3. **Mostrar informacion**
   - Se mostrar toda la informacion relacionada con la reserva, para que el administrador evalue la reserva.
4. **Aprobar o rechazar reserva**
   - El administrador tiene la opcion de rechazar o aprobar la reserva.
      - En caso de aprorbar la reserva puede agregar un comentario o observacion.
      - Si la reserva es rechazada se puede agregar el motivo del rechazo.
5. **Notificación de Resultado**  
   - El cliente recibe una notificación confirmando el estado de la reserva.
6. **Regresa a la pantalla principal**
   - El administrador regresa a la pagina de reservas.

**Diagrama**
```mermaid
graph TD;
    A[Inicio] --> B[Administrador accede a la aplicación]
    B --> C{¿Sesión activa?}
    C -->|No| D[Redirige a inicio de sesión]
    D --> B
    C -->|Sí| E[Muestra dashboard con lista de reservas]
    E --> F[Administrador aplica filtros]
    F --> G[Selecciona acción: Ver detalle]
    G --> H[Muestra información completa de la reserva]
    H --> I{¿Desea aprobar o rechazar la reserva?}
    I -->|Aprobar| J[Actualiza estado a Aprobada]
    I -->|Rechazar| K[Actualiza estado a Rechazada]
    J --> L[Agrega comentario u observación]
    K --> M[Agrega motivo del rechazo]
    L --> N[Notifica al cliente]
    M --> N
    N --> O[Regresa a la lista de reservas]
    O --> P[Fin]

```