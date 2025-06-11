---
sidebar_position: 1
title: Realizar una reserva
---

### Caso de Uso: "Realizar una Reserva"

**Actor:** Cliente  
**Flujo Básico:**
1. El cliente inicia sesión.
2. Consulta la disponibilidad de recursos para una fecha determinada.
3. Selecciona uno o varios recursos y añade detalles (número de personas, horario).
4. Envía la solicitud de reserva.
5. El sistema notifica al administrador.
6. El administrador revisa y aprueba o rechaza la reserva.
7. Se envía una notificación de confirmación o rechazo al cliente.

### Diagramas


```mermaid
sequenceDiagram
    participant Cliente
    participant Sistema
    participant Administrador

    Cliente->>Sistema: Inicia sesión
    Sistema-->>Cliente: Confirma autenticación

    Cliente->>Sistema: Consulta disponibilidad de recursos
    Sistema-->>Cliente: Muestra recursos disponibles

    Cliente->>Sistema: Selecciona recursos y añade detalles
    Cliente->>Sistema: Envía solicitud de reserva
    Sistema->>Administrador: Notifica solicitud de reserva

    Administrador->>Sistema: Revisa y aprueba/rechaza reserva
    Sistema-->>Cliente: Envía confirmación o rechazo

```

