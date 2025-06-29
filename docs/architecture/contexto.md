---
sidebar_position: 1
title: Contexto
---
# 1. Contexto del Sistema (C4 – Nivel 1)

## 1.1 Visión general
ResourceReservationApp es la plataforma central para planificar eventos mediante la reserva y gestión de recursos (sillas, mesas, luces, etc.). Facilita:
- Una experiencia self-service para los organizadores.  
- Control de stock y disponibilidad en tiempo real.  
- Flujo de pago, confirmación y notificaciones automatizadas.  

## 1.2 Objetivos
- Reducir el tiempo de aprovisionamiento de recursos en un 50 %.  
- Asegurar una disponibilidad 24/7 con un uptime ≥ 99.9 %.  
- Escalar horizontalmente ante picos de demanda sin cambios de código.  

## 1.3 Requisitos no funcionales
- **Rendimiento:** 95 % de respuestas < 200 ms.  
- **Seguridad:** OWASP Top 10, cifrado de datos sensibles y JWT para sesiones.  
- **Escalabilidad:** soporte para ≥ 1 000 usuarios concurrentes.  
- **Disponibilidad:** tolerancia a fallos en pasarela de pago e inventario externo.  

## 1.4 Actores
| Actor                      | Descripción                                                                 |
|----------------------------|-----------------------------------------------------------------------------|
| Organizador de Eventos     | Cliente final: reserva recursos y consulta disponibilidad desde la UI web.  |
| Administrador principal    | Crea/gestiona perfiles de logística y organizadores; gestiona permisos.     |
| Personal de Logística      | Consulta reservas, actualiza stock y coordina el despliegue de recursos.    |
| Pasarela de Pago (Stripe*) | Sistema externo que procesa pagos y emite recibos.                          |
| Sistema de Inventario (*)  | ERP externo que mantiene niveles de stock en tiempo real.                   |
| Servicio de Email (*)      | Envía confirmaciones y recordatorios a organizadores y logística.           |

\* Sistemas externos

## 1.5 Diagrama de Contexto
```mermaid
C4Context
    title Contexto – ResourceReservationApp
    Person(org, "Organizador", "Reserva recursos para eventos")
    Person(admin, "Admin Principal", "Gestiona perfiles y permisos")
    Person(log, "Personal de Logística", "Maneja stock y despliegue de recursos")
    
    System_Boundary(app, "ResourceReservationApp") {
      System(spa, "Web SPA", "React + Vite", "Interfaz de usuario")
      System(api, "API REST", "ASP.NET Core", "Lógica de negocio y orquestación")
      SystemDb(db, "PostgreSQL", "Persistencia de datos")      
    }

    System_Ext(pay, "Pasarela de Pago", "Stripe", "Procesa pagos")
    System_Ext(inv, "Sistema de Inventario", "ERP externo", "Control de stock")
    System_Ext(email, "Servicio de Email", "SendGrid/SMTP", "Envía notificaciones")

    Rel(org, spa, "Usa")
    Rel(admin, spa, "Usa")
    Rel(log, spa, "Usa")
    Rel(spa, api, "Consume REST API")
    Rel(api, db, "Lee/Escribe datos")
    Rel(api, pay, "Solicita pago")
    Rel(api, inv, "Sincroniza stock")
    Rel(api, email, "Envía correos")
