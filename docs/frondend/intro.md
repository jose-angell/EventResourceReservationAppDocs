---
sidebar_position: 1
title: Introduccion
---

## 🚀 Introducción al Frontend


Este documento describe la aplicación cliente (Web SPA) de ResourceReservationApp. Aquí encontrarás las tecnologías, la estructura del proyecto y cómo navegar la documentación específica del frontend.

---

### 1. ¿Qué encontrarás en esta carpeta?

- **Introducción**  
  Descripción general del stack, estructura y cómo usar estos docs.

- **Guía de estilos**  
  Convenciones de Tailwind/SCSS, naming de clases, accesibilidad.

- **Componentes**  
  Catálogo de React Components y UI primitives reutilizables.

- **Features**  
  Spec detallada de cada funcionalidad: overview, wireframes, estado, API, tests.

---

### 2. Estructura del repositorio frontend

```text
frontend/
├─ Introduccion.md       Este archivo  
├─ Guia-de-estilos.md    Normas de CSS/Tailwind y accesibilidad  
├─ Componentes/          Specs de componentes reutilizables  
└─ Features/             Specs por feature  

```

---
### 3. Tecnologías principales
    - React 18 con TypeScript
    - Vite como bundler rápido
    - Tailwind CSS + SCSS para estilos
    - Zustand o Redux Toolkit para estado global
    - React Router v6 para rutas
    - Jest + React Testing Library para tests
    - MSW para mocks de API en desarrollo y tests

### 4. ¿Cómo usar esta doc?
    - Lee Introducción y Guía de estilos antes de escribir código.
    - Para un componente nuevo, crea docs/frontend/Componentes/`<ComponentName>`.md.
    - Para una feature nueva, crea docs/frontend/Features/`<feature>`/ con los archivos 1-overview.md…5-tests.md.