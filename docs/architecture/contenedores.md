---
sidebar_position: 2
title: Contenedores
---
## 2. Contenedores (C4 – Nivel 2)

### 2.1 Descripción general  
En este nivel describimos cada aplicación, servicio o almacén de datos desplegable que compone el sistema de reservas de recursos.

| Contenedor         | Tecnología            | Responsabilidad                                                      |
|--------------------|-----------------------|----------------------------------------------------------------------|
| Web SPA            | React + Vite          | Interfaz de usuario modular por feature, consumo de la API REST.     |
| API REST           | ASP.NET Core Web API  | Orquestación de casos de uso, validaciones, enrutamiento y DTOs.    |
| Base de datos      | SQL Postgres          | Persistencia de recursos, reservas y usuarios.                       |
| Worker de Tareas   | .NET Worker Service   | Procesos batch y limpieza de reservas expiradas, sincronización con sistema de inventario. |

