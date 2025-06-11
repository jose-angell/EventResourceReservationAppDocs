---
sidebar_position: 2
title: Gestión de Recursos
---


### Caso de Uso: "Gestión de Recursos"

**Actor:** Administrador  
**Flujo Básico:**
1. El administrador inicia sesión.
2. Accede al dashboard de gestión.
3. Agrega, actualiza o elimina un recurso.
4. El sistema actualiza la disponibilidad en tiempo real.

``` mermaid
sequenceDiagram
    participant Administrador
    participant Sistema

    Administrador->>Sistema: Inicia sesión
    Sistema-->>Administrador: Confirma autenticación

    Administrador->>Sistema: Accede al dashboard
```