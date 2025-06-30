---
sidebar_position: 1
title: 01-overview
---

## 🚀 Feature: `<Nombre de la feature>`

> Archivo: `01-overview.md`

### 1. Resumen de la feature

#### 1.1 Propósito
**Definición:** Es la frase o párrafo breve que explica **por qué** existe la funcionalidad. Debe responder a:  
- ¿Qué problema de negocio soluciona?  
- ¿Qué valor aporta al usuario o la organización?

**Ejemplo:**  
> “Permite a un organizador consultar la disponibilidad de recursos en tiempo real, evitando llamadas manuales al equipo de inventario.”
---

#### 1.2 Objetivos y métricas de éxito  
**Definición:** Metas cuantificables que indican cuándo la feature cumple su propósito. Incluye:  
- Objetivos SMART (específicos, medibles, alcanzables, relevantes y con tiempo).  
- Métricas (KPIs) para validar adopción y rendimiento.

**Ejemplo:**  
- **Objetivo 1:** Reducir en un 70 % las consultas manuales de stock en un mes.  
- **Objetivo 2:** Mantener latencia de respuesta < 100 ms.  
- **Métricas:**  
  - `#` de llamadas al endpoint vs. tickets de inventario.  
  - `%` de peticiones con tiempo de respuesta < 100 ms.

---

#### 1.3. Contexto de negocio
**Definición:** Describe el flujo actual y en qué punto se inserta la feature:  
- Flujo manual o sistémico previo.  
- Qué procesos o actores intervenían.  
- Cómo cambia el flujo una vez implementada.
Ubica esta feature dentro del flujo general:  
1. **Flujo actual**: Listado manual → Email → Respuesta.  
2. **Flujo propuesto**: App → `GET /api/resources` → Respuesta instantánea.  
3. Beneficios:  
   - Menos puntos de fallo (sin email).  
   - Mejor trazabilidad en logs y auditoría.

---

#### 1.4 Alcance (In-Scope / Out-of-Scope)  
**Definición:** Lista todo lo que **incluye** esta entrega y lo que **queda fuera** para evitar confusiones.  
- In-Scope: desarrollos, integraciones y pruebas que se harán ahora.  
- Out-of-Scope: funcionalidades o tareas que se postergan para otra fase.

**Ejemplo:**  
- **Incluye:** endpoint REST, caché en memoria de 30 s.  
- **No incluye:** cambios UI (irán en docs/frontend), reports históricos.
---

### 2. Actores involucrados
<!-- Lista quiénes participan en este proceso -->
#### Definición  
Un **actor** es un usuario o sistema externo que interactúa con la feature. **Stakeholders** son roles o equipos interesados en el correcto funcionamiento.

| Actor / Stakeholders   | Rol                                       |
|------------------------|-------------------------------------------|
| Organizador de Eventos | Inicia la reserva                         |
| AdminPrincipal         | Supervisa y gestiona permisos             |
| …                      | …                                         |

### 3. Historias de usuario y criterios de aceptación

#### 3.1 User Story  
**Definición:** Frase que describe la necesidad desde la perspectiva del usuario. Tiene tres partes:  
- **Quién** (rol/actor).  
- **Qué** (capacidad o acción).  
- **Para qué** (valor o beneficio).

**Formato:**  
> “Como `<Actor>`, quiero `<acción>` para `<beneficio>`.”

**Ejemplo:**  
> “Como Organizer, quiero consultar recursos disponibles para planificar sin depender de emails.”

---
#### 3.2 Criterios BDD (Given–When–Then)  
**Definición:** Escenarios que describen comportamientos esperados de forma estructurada:  
- **Given** (Dado que): precondiciones o contexto inicial.  
- **When** (Cuando): acción o evento que desencadena la lógica.  
- **Then** (Entonces): resultado o cambio observable en el sistema.

Puedes añadir escenarios alternativos para errores o caminos distintos.

- **Given** un organizador autenticado con permiso `resources.read`  
- **When** llama `GET /api/resources?date=2025-07-15`  
- **Then** recibe `200 OK` con un array de objetos `{ id, name, quantity }`  
- **And** (si es neceario)

**Escenarios alternativos:**  
- Given fecha inválida → Then `400 Bad Request`  
- Given fallo inventario externo → Then `503 Service Unavailable` 
---