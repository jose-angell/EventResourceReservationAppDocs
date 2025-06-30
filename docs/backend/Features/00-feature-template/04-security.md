---
sidebar_position: 4
title: 04-security
---
# 🔒 Seguridad y validaciones

> Archivo: `04-security.md`

## 1. Roles y permisos
<!-- ¿Qué roles pueden usar esta feature? -->
- `Role1`  
- `Role2`

## 2. Claims o scopes requeridos
<!-- JWT scopes, claims, cabeceras especiales -->
- `scope:<feature>.<acción>`

## 3. Validaciones de entrada
<!-- Regla de negocio o formato de datos -->
- Campo `<X>` debe ser no nulo y longitud ≤ 100.  
- Fecha debe estar en el futuro.  

## 4. Reglas de autorización
<!-- Quién puede leer, actualizar o eliminar -->
- Solo el owner o `AdminPrincipal` puede modificar.