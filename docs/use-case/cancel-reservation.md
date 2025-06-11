---
sidebar_position: 3
title: Cancela una reserva
---

### Caso de Uso: "Realizar una Reserva"

**Actor:** Cliente  
**Flujo Básico:**
1. El cliente inicia sesión.
2. Consulta las reservas realizadas. 
3. Seleciona la reserva a cancelar.
4. valida los datos y confima cancelacion.
5. El sistema notifica al administrador.
6. Se envía una notificación de confirmación cliente.

### Diagramas

```mermaid
sequenceDiagram
    participant Cliente
    participant Sistema
    participant Administrador

    Cliente->>Sistema: Inicia sesión
    Sistema-->>Cliente: Confirma autenticación

    Cliente->>Sistema: Consulta reservas
    Sistema-->>Cliente: Muestra reservas disponibles

    Cliente->>Sistema: Selecciona reserva a cancelar
    Cliente->>Sistema: Valida datos y confirma cancelación
    Sistema->>Administrador: Notifica cancelación
    Sistema-->>Cliente: Envía confirmación de cancelación

```