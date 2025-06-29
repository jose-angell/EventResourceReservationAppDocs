---
sidebar_position: 1
title: templete overview
---

# Feature: `<nombre-feature>`

 **Carpeta:** `docs/backend/Features/<nombre-feature>/`

---

## 1. Overview  
- **Objetivo**:  
- **Contexto de negocio**:  
- **Actores**:  
- **Criterios BDD**:  
  - Given …  
  - When …  
  - Then …

---

## 2. Modelo de datos  
- Entidades/tablas nuevas o modificadas.  
- Esquema SQL o clases EF Core.  
- Migraciones: nombre y propósito.

---

## 3. Contrato API  
| Método | Ruta                  | Descripción           |
|--------|-----------------------|-----------------------|
| POST   | `/api/<recurso>`      | Crea …                |
| GET    | `/api/<recurso>/{id}` | Obtiene …             |

- **Request** (ejemplo JSON)  
- **Response** (ejemplo JSON)  
- **Códigos HTTP**: 200, 201, 400, 404, 500.

---

## 4. Lógica de negocio  
- Interfaz y clase de UseCase:  
  ```csharp
  public interface I<Feature>UseCase { … }
  public class <Feature>UseCase : I<Feature>UseCase { … }
  ```
