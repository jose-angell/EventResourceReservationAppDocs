---
sidebar_position: 2
title: Gestión de Recursos
---


### Caso de Uso: "Gestión de Recursos"

## Descripción General
Este caso de uso describe la capacidad del administrador para gestionar los recursos disponibles en el sistema. Incluye la creación, modificación y eliminación de recursos.

## Actores
- **Cliente:** Usuario que realiza reservas de recursos disponibles en el sistema.
- **Administrador:** Responsable de gestionar las solicitudes de reserva y controlar la disponibilidad.

---

**Actor:** Administrador  

**Flujo Principal**
1. El administrador inicia sesión en el sistema.
2. Accede al panel de administración de recursos.
3. Puede seleccionar si quiere agregar, modificar o eliminar un recurso.
4. Según la acción seleccionada, sigue el flujo correspondiente.

``` mermaid
sequenceDiagram
    participant Administrador
    participant Sistema

    Administrador->>Sistema: Inicia sesión
    Sistema-->>Administrador: Confirma autenticación

    Administrador->>Sistema: Accede al panel de administración de recursos
    Sistema-->>Administrador: Muestra opciones de gestión

    Administrador->>Sistema: Selecciona acción (Agregar, Modificar, Eliminar)

    Sistema-->>Administrador: Actualiza disponibilidad en tiempo real

```

---

## Subcaso de Uso: Agregar un Recurso
**Flujo Básico**
1. El administrador accede al formulario para agregar un nuevo recurso.
2. Ingresa los datos del recurso (nombre, tipo, disponibilidad, descripción).
3. Confirma y guarda el nuevo recurso.
4. El sistema actualiza la lista de recursos disponibles.
5. Se muestra una notificación de éxito en el dashboard.

```mermaid
sequenceDiagram
    participant Administrador
    participant Sistema

    Administrador->>Sistema: Accede al formulario de nuevo recurso
    Sistema-->>Administrador: Muestra formulario

    Administrador->>Sistema: Ingresa datos (nombre, tipo, disponibilidad, descripción)
    Sistema-->>Administrador: Valida datos ingresados

    Administrador->>Sistema: Confirma y guarda el recurso
    Sistema-->>Administrador: Guarda en la base de datos

    Sistema-->>Administrador: Actualiza lista de recursos disponibles
    Sistema-->>Administrador: Muestra notificación de éxito en el dashboard
```
---

## Subcaso de Uso: Modificar un Recurso
**Flujo Básico**
1. El administrador accede al listado de recursos.
2. Selecciona el recurso que desea modificar.
3. Realiza los cambios necesarios en el formulario de edición.
4. Confirma la actualización.
5. El sistema guarda los cambios y actualiza el estado del recurso.

```mermaid
sequenceDiagram
    participant Administrador
    participant Sistema

    Administrador->>Sistema: Accede al listado de recursos
    Sistema-->>Administrador: Muestra recursos disponibles

    Administrador->>Sistema: Selecciona recurso a modificar
    Sistema-->>Administrador: Muestra formulario de edición

    Administrador->>Sistema: Realiza cambios en el recurso
    Sistema-->>Administrador: Valida datos ingresados

    Administrador->>Sistema: Confirma actualización
    Sistema-->>Administrador: Guarda cambios en la base de datos

    Sistema-->>Administrador: Actualiza estado del recurso
    Sistema-->>Administrador: Muestra notificación de éxito
```
--- 

## Subcaso de Uso: Eliminar un Recurso

**Flujo Básico**
1. El administrador accede al listado de recursos.
2. Selecciona el recurso que desea eliminar.
3. Confirma la acción de eliminación.
4. El sistema borra el recurso y lo remueve del inventario.
5. Se muestra una notificación de éxito en el dashboard.

```mermaid
sequenceDiagram
    participant Administrador
    participant Sistema

    Administrador->>Sistema: Accede al listado de recursos
    Sistema-->>Administrador: Muestra recursos disponibles

    Administrador->>Sistema: Selecciona recurso a eliminar
    Sistema-->>Administrador: Muestra opción de confirmación

    Administrador->>Sistema: Confirma eliminación
    Sistema-->>Administrador: Borra recurso de la base de datos

    Sistema-->>Administrador: Actualiza inventario
    Sistema-->>Administrador: Muestra notificación de éxito en el dashboard
```