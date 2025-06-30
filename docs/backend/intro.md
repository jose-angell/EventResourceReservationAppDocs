---
sidebar_position: 1
title: Introduccion
---
## 🚀 Introducción al Backend

Bienvenido a la documentación del **Backend** de ResourceReservationApp. Aquí encontrarás todo lo necesario para entender, evaluar y contribuir al servicio ASP.NET Core que expone la API de reservas.
---

### 1. ¿Qué encontrarás en esta carpeta?
- **Arquitectura**  
  Diagrama de contenedores (C4 Nivel 2), componentes (C4 Nivel 3) y ADRs.  
- **Features**  
  Specs detalladas de cada funcionalidad (ruta, DB, use cases, tests).  
- **Convenciones**  
  Guía de estilos, naming, patrones de API, bases de datos, logs y testing.  
- **Operaciones (ops)**  
  Scripts de migraciones, despliegue y health checks.


---

### 2. Estructura del repositorio

```text
.
└─ backend/                    Documentación viviente  
   ├─ arquitectura/            C4 diagrams y ADRs  
   ├─ Features/                Carpeta con specs de cada feature  
   ├─ Introduccion.md          Este archivo  
   ├─ Guia-de-convenciones.md  Guías de estilo y patrones  
   └─ ops/                     Pasos de despliegue y operaciones  

```

---

### 3. Tecnologías principales
    - .NET 7 / ASP.NET Core 7
    - Entity Framework Core (Code-First, Migrations)
    - PostgreSQL (persistencia relacional)
    - xUnit / Moq (unit & integration tests)
    - OpenAPI / Swagger (documentación de API)

### 4. ¿Cómo usar esta documentación?
    1. Lee la Introducción y la Guía de convenciones antes de tocar código.
    2. Para un feature nuevo, ve a docs/Features/00-feature-template y abre feature-guidelines.md.
    3. Revisa los diagramas de arquitectura en docs/arquitectura/.
    4. Sigue la plantilla de ADRs (docs/arquitectura/ADRs/) para registrar decisiones.
    5. Antes de cada PR, asegúrate de que tu código y tu spec estén alineados con las convenciones aquí descritas.