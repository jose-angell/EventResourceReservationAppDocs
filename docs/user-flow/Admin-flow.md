---
sidebar_position: 1
title: flujo creacion de Reservas
---



1. Consultar todos los administradores (secundarios)
2. consultar detalle del perfil de administrador
3. modificar estatus de administrador

### Flujo de Usuario: "Flujo de Reservas"

**Descripción del Flujo:**

Este flujo describe el proceso para que un cliente realice una reserva en el sistema.

1. **Visualización de la Página Principal:**  
   - La página se muestra sin necesidad de iniciar sesión.
   - La pantalla muestra una carga incial de todos los recursos disponibles para ese dia.
   - El cliente ve la opción de buscar el recurso en el inventario disponible.

2. **Búsqueda y Consulta de Disponibilidad:**  
   - El cliente selecciona la fecha (obligatorio), la hora (opcional) y el tipo de recurso (opcional).
     - Si hay recursos disponibles, continúa el flujo.
     - Si no, se muestra un mensaje que invita a elegir otra fecha o recurso.

3. **Selección del Recurso y Configuración de la Reserva:**  
   - El cliente elige el recurso específico y ajusta parámetros como la cantidad o la ubicación.

4. **Confirmación de la Reserva:**  
   - El cliente revisa la información y confirma los datos ingresados.
     - Si el cliente tiene sesión activa, continúa el flujo.
     - Si no tiene sesión, se le solicita que inicie sesión o se registre (se guardan los datos de la configuracion de la reserva temporalmente) .

5. **Envío de la Solicitud de Reserva:**  
   - El sistema registra la solicitud.
     - Si el recurso tiene configuración de autorización automática, la reserva se registra con un estado de “autorizado” y se envía una notificación al administrador.
     - Si el recurso requiere autorización manual, la reserva se registra con un estado “pendiente” y se envía para aprobación.

6. **Notificación de Resultado:**  
   - El cliente recibe una notificación confirmando que la reserva ha sido registrada, incluyendo los posibles estados de pendiente, aprobación o rechazo.

7. **Regresa a la pantalla principal:**
   - El cliente regresa a la pagina principal.


``` mermaid
graph TD;
    A[Inicio] --> B[Visualización de la Página Principal]
    B --> C[Muestra recursos disponibles para el día]
    C --> D[Cliente busca en el inventario]

    D --> E[Selecciona fecha, hora y tipo de recurso]
    E -->F{¿Hay disponibilidad?}
    F -->|Si| H[Selecciona recurso y ajusta parámetros]
    F -->|No| G[Muestra mensaje de error o sugiere otra fecha]
    G --> D

    H --> I[Confirma la reserva]
    I -->J{¿Cliente tiene sesión activa?}
    J -->|No| K[Guarda configuración de la reserva temporalmente]
    K -->L[Solicita inicio de sesión o registro]
    L --> M

    J -->|Si| M[Envía solicitud de reserva]
    M --> N{¿Autorización automática?} 
    N -->|No| O[Reserva pendiente, espera aprobación]
    N -->|Si| P[Reserva autorizada, notifica al administrador]
    O --> Q

    P --> Q[Notificación de registro de reserva]
    Q --> R[Regresa a la pantalla principal]
    R --> B
```


