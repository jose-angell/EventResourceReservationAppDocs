---
sidebar_position: 1
title: Introduccion
---

## 🚀 Introducción al Backend

¡Bienvenido a la documentación del **Backend** de **ResourceReservationApp**! Aquí encontrarás todo lo necesario para entender, evaluar y contribuir al servicio **ASP.NET Core** que expone la API de reservas.

---

### 1. ¿Qué encontrarás en esta carpeta?
  - **Arquitectura:** Diagramas detallados (C4 Nivel 2 y Nivel 3) que muestran la estructura de contenedores y componentes, junto con los **ADRs (Architecture Decision Records)** que justifican las decisiones clave de diseño.
  - **Decisiones Técnicas:** Un documento dedicado a explicar las **elecciones tecnológicas y arquitectónicas** del backend, incluyendo el porqué de .NET, Entity Framework Core y PostgreSQL, entre otros.
  - **Features:** Especificaciones detalladas de cada funcionalidad, incluyendo rutas de API, interacción con la base de datos, casos de uso y estrategias de pruebas. 
  - **Convenciones:** Una guía exhaustiva sobre estilos de código, convenciones de nombres, patrones de API, uso de bases de datos, manejo de logs y metodologías de testing. 
  - **Operaciones (Ops):** Scripts esenciales para migraciones de base de datos, procesos de despliegue y configuraciones de `health checks`.


---

### 2. Estructura del Repositorio

La carpeta `backend/` de este repositorio es el hogar de esta documentación "viviente":

```text
.
└─ backend/
   ├─ arquitectura/            # Diagramas y ADRs
   ├─ DecisionsTecnicas.md     # Explicación del porqué de nuestras elecciones
   ├─ Features/                # Especificaciones detalladas de cada funcionalidad
   ├─ Introduccion.md          # Este archivo
   ├─ Guia-de-convenciones.md  # Guías de estilo y patrones de desarrollo
   └─ ops/                     # Scripts de despliegue y operaciones
```

---

### 3. Tecnologías Principales

El backend ha sido construido utilizando un stack robusto y moderno:
    - **.NET 8 / ASP.NET Core 8:** El framework principal para construir la API.
    - **Entity Framework Core:** ORM para la persistencia de datos (enfoque Code-First con Migrations).
    - **PostgreSQL:** Base de datos relacional para almacenamiento de información.
    - **xUnit / Moq:** Herramientas para la implementación de pruebas unitarias y de integración.
    - **OpenAPI / Swagger:** Para la generación automática y visualización de la documentación de la API.

### 4. ¿Cómo Usar Esta Documentación?
Para una contribución efectiva y coherente con el proyecto, te recomendamos seguir estos pasos:
    1. **Antes de codificar:** Dedica tiempo a leer esta `Introducción` y la `Guía de Convenciones`.
    2. **Entender las decisiones:** Revisa el documento de Decisiones Técnicas (`DecisionsTecnicas.md`) para comprender las bases de nuestra arquitectura.
    3. **Explorar la arquitectura:** Consulta los diagramas de arquitectura en `docs/arquitectura/` para una visión global del sistema.
    4. **Trabajar en una `feature`:** Si vas a implementar una nueva funcionalidad, inicia revisando `docs/Features/00-feature-template` y su `feature-guidelines.md`.
    5. **Registrar decisiones:** Para cualquier decisión arquitectónica relevante, utiliza la plantilla de ADRs ubicada en `docs/arquitectura/ADRs/`.
    6. **Antes de cada Pull Request (PR):** Asegúrate de que tu código y las especificaciones estén completamente alineados con las convenciones aquí descritas.

