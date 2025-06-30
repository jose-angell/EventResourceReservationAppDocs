---
sidebar_position: 2
title: 02-database
---

# 📦 Modelo de datos

> Archivo: `02-database.md`

## 1. Entidades nuevas o modificadas
<!-- Define las tablas o agregados con su esquema SQL o clases EF -->
```sql
CREATE TABLE <Entidad> (
  Id UUID PRIMARY KEY,
  <Campo> <Tipo> NOT NULL,
  …
);
```

## 2. Migraciones
<!-- Nombre y objetivo de cada migración -->
    - YYYYMMDD_`<Descripción>`: …

## 3. Relaciones e índices
| Columna | Tipo (FK/INDEX)	 | 	Propósito |
|---------|------------------|------------|
|`<Tabla>.<Campo>` | FK -> OtraTabla(Id) | Integridad referencial |
|`<Tabla>.<Campo2>` | INDEX | Optimizar consultas X |

