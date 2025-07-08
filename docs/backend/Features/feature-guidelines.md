---
sidebar_position: 1
title: Guia Creacion de Features
---

# 🛠 Guía para la Creación de Nuevas Features (Backend)

Este documento define el **flujo de trabajo estandarizado** y la **plantilla de documentación** para cada nueva funcionalidad desarrollada en la aplicación **.NET Core**. Seguir esta guía asegura la consistencia, la calidad y la mantenibilidad de cada feature.

---

## 1. Flujo de Trabajo Detallado
A continuación, se describen los pasos para el desarrollo de una nueva feature, desde su definición hasta el despliegue:

### 1.1 Definición y Especificación de la Feature

1. **Definir y Specificar**  
   - **Crea una Carpeta Dedicada:** Dentro de `docs/backend/Features/`, crea un nuevo folder con un nombre descriptivo para tu feature (ej. `docs/backend/Features/gestion-usuarios/`).
   - **Copia y Renombra la Plantilla:** Copia el archivo `01-overview.md` a tu nueva carpeta y pegalo en tu carpeta de la nueva feature.  
   - **Completa el** `01-overview.md`: Rellena este archivo con la información esencial de la feature:
      - **Objetivo:** ¿Qué problema resuelve o qué valor añade esta feature?
      - **Contexto:** Antecedentes relevantes o dependencias.
      - **Criterios de Aceptación (BDD):** Define claramente los escenarios de comportamiento esperados utilizando un formato tipo Gherkin (Given/When/Then).

### 1.2 **Abrir un Issue en GitHub**  
   1. **Crea un Issue:** Abre un nuevo issue en GitHub para la feature.
   2. **Asigna Etiquetas:** Utiliza las etiquetas `backend` y `feature` (y otras relevantes como `enhancement` o `bug`, si aplica).  
   3. **Detalles del Issue:** Incluye en la descripción del issue:
      - Un **título corto y descriptivo** de la feature.
      - Un **enlace directo a la especificación** (`spec`): `docs/backend/Features/<nombre-feature>/01-overview.md`. 
      - Un **checklist "ready for dev"** para asegurar que todos los requisitos previos están cubiertos antes de iniciar la codificación.

### 1.3 **Creación de la Rama de Desarrollo**  
   - **Nombra tu Rama:** Crea una nueva rama de feature desde `develop` (o `main` si es tu rama principal de integración) siguiendo la convención de nomenclatura de ramas:
   ```bash
   git checkout -b feature/<nombre-de-tu-feature>
   ```
### 1.4 **Documentación en Paralelo**

   - **Actualiza Continuamente:** A medida que avanzas en la implementación, actualiza y crea los siguientes archivos dentro de `docs/backend/Features/<nombre-feature>/` para mantener la documentación sincronizada con el código:
     - `02-modelos.md`: Esquema de base de datos, modelos de EF Core y cualquier migración necesaria.
     - `03-api.md`: Definición de endpoints, ejemplos de request/response y consideraciones de seguridad a nivel de API.
     - `04-usecases.md`: Descripción detallada de los casos de uso principales y diagramas de secuencia (UML, si es posible) que ilustren el flujo de la lógica de negocio.
     - `05-seguridad.md`: Aspectos específicos de seguridad para la feature (roles requeridos, validaciones de entrada, protección de datos).
     - `06-tests.md`: Estrategia de pruebas para la feature, incluyendo qué tipos de pruebas se implementarán (unitarias, integración), consideraciones de mocks, etc.

### 1.5 **Implementación del Código**
   - **Desarrollo por Capas:** Implementa la lógica de la feature siguiendo la arquitectura definida:
      - **Domain:**  Nuevas entidades, agregados o servicios de dominio.
      - **Application:** Implementación de interfaces y clases de Casos de Uso (Use Cases) y DTOs específicos de aplicación.
      - **Infrastructure:** mplementaciones de repositorios (EF Core), clientes HTTP externos (ej. Stripe), o servicios de fondo (workers) si aplica.
   - **Añade Pruebas Exhaustivas:**
      - **Implementa pruebas unitarias** en `tests/Unit`.
      - **Implementa pruebas de integración** en `tests/Integration`.

### 1.6 **Creación de Pull Request (PR)**
   - ** Utiliza la Plantilla de PR:** Abre un Pull Request a `develop` (o `main`) y rellena la plantilla de PR, asegurando que incluya:
      - **Issue Resuelto:** Menciona el issue de GitHub que resuelve (ej. `Closes #<número-del-issue>`).
      - **Especificación Actualizada:** Enlaza a los documentos Markdown de la feature que han sido actualizados (ej. `docs/backend/Features/<nombre-feature>/01-overview.md`).
      - **Verificación Local:** Confirma que el código compila correctamente y que todas las pruebas (unitarias y de integración) pasan localmente.
   - **Asigna Revisores:** Asigna a los revisores apropiados y a los code-owners para su revisión.

### 1.7 **Merge y Despliegue**
    - **Aprobación y CI/CD:** Una vez que el PR ha sido aprobado y las pruebas de Integración Continua (CI) han pasado exitosamente, procede a hacer merge en la rama de `develop` o `main`, según el flujo de trabajo del equipo.
    - **Automate CI/CD:** El sistema de Integración Continua/Despliegue Continuo (CI/CD) se encargará de ejecutar las migraciones de base de datos, las pruebas finales y el despliegue de la nueva versión de la aplicación.

