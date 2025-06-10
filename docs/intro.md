---
sidebar_position: 1
---

# Introducción

Este documento describe el análisis de requisitos y los flujos de usuario para el desarrollo de una aplicación de reservas de eventos.  

**Objetivo:** Validar y definir el alcance para construir un MVP en el que el Administrador gestione los recursos y reservas, con la posibilidad de evolucionar hacia una solución multi-tenant en el futuro.

## Objetivos del Proyecto

- Desarrollar una aplicación web para reservar recursos destinados a eventos (mesas, sillas, iluminación, salones, etc.).
- Permitir que un único administrador gestione los recursos y las reservas.
- Sentar las bases para una futura extensión en la que proveedores externos puedan gestionar sus propios recursos.

## Alcance del Proyecto

**Funcionalidades Incluidas:**
- Registro e inicio de sesión para usuarios (cliente y administrador).
- Consulta y visualización de disponibilidad de recursos.
- Creación, modificación y cancelación de reservas.
- Gestión del inventario de recursos por parte del administrador.
- Procesamiento de pagos y confirmación de reservas (fase posterior o prototipo básico).

**Exclusiones (Por el momento):**
- Gestión de múltiples proveedores externos.
- Integración avanzada con pasarelas de pago (puede simplificarse en el MVP).

## Stakeholders y Roles

- **Administrador:**  
  - Rol principal encargado de gestionar los recursos, aprobar reservas y actualizar la disponibilidad.
- **Cliente:**  
  - Usuario final que consulta la disponibilidad y solicita reservas.
- **Futuro Proveedor (Opcional para fase evolutiva):**  
  - Rol que en versiones futuras permitirá a terceros gestionar su propio catálogo de recursos.


## Requisitos Funcionales

- **Autenticación y Autorización:**
  - Registro de usuarios.
  - Inicio de sesión seguro.
- **Gestión de Recursos:**
  - CRUD para recursos (mesas, sillas, salones, etc.).
  - Visualización del estado (disponible, reservado).
- **Gestión de Reservas:**
  - Creación de reservas: Selección de fecha, hora y recursos.
  - Modificación y cancelación de reservas.
  - Notificaciones de confirmación o cancelación.
- **Dashboard Administrativo:**
  - Visualización de reservas y recursos.
  - Gestión de incidencias y actualización de disponibilidad.

## Requisitos No Funcionales

- **Usabilidad:**  
  - Interfaz intuitiva con tiempos de respuesta rápidos.
- **Seguridad:**  
  - Implementación de protocolos para proteger datos personales (por ejemplo, JWT para la autenticación).
- **Escalabilidad:**  
  - Arquitectura modular que permita la adición de nuevos roles (proveedores) en el futuro.
- **Compatibilidad:**  
  - Accesible desde diferentes dispositivos (responsive design).