---
sidebar_position: 2
title: Contenedores
---
<!-- File: arquitectura/02-contenedores-c4.md -->

# 2. Contenedores (C4 – Nivel 2)

## 2.1 Descripción general  
En este nivel detallamos cada aplicación o servicio desplegable que compone EventReservationApp. Cada contenedor es autónomo, ejecutable por separado y se comunica mediante APIs o colas de mensajes.

| Contenedor         | Tecnología               | Responsabilidad                                                                                       |
|--------------------|--------------------------|-------------------------------------------------------------------------------------------------------|
| Web SPA            | React + Vite             | Interfaz de usuario: self-service para organizadores, logística y administración.                     |
| API REST           | ASP.NET Core 8           | Exposición de endpoints REST, orquestación de casos de uso, validaciones, seguridad (JWT) y lógica.  |
| Base de datos      | PostgreSQL               | Persistencia de usuarios, recursos, reservas y datos de auditoría.                                    |
| Worker de Tareas   | .NET Worker Service      | Procesos asíncronos: limpieza de reservas expiradas, sincronización con ERP de inventario y colas.   |

Sistemas externos:
- **Pasarela de Pago (Stripe)**  
- **ERP de Inventario**  
- **Servicio de Email**  

---
