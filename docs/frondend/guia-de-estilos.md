---
sidebar_position: 2
title: Guia de estilos
---

## 📘 Guía de estilos y accesibilidad

Este documento reúne las convenciones de CSS/Tailwind, naming y buenas prácticas de accesibilidad para el frontend.

---

### 1. Estructura de estilos

- **Tailwind CSS**  
  - Utiliza clases utilitarias para layout, espaciado y tipografía.  
  - Configura las extensiones básicas en `tailwind.config.js`.  
  - Evita estilos inline redundantes.

- **SCSS Modules**  
  - Para estilos muy específicos de componente, usa módulos SCSS (`Component.module.scss`).  
  - Nombra clases en kebab-case: `.reservation-form`, `.btn-primary`.

---

### 2. Naming de clases y componentes

- **React Components**  
  - Archivos y folders en PascalCase: `ReservationForm.tsx`, `Button/`.  
  - Componentes exportados como default en su carpeta.

- **CSS Classes**  
  - Tailwind: prefijos claros (`bg-`, `text-`, `p-`, `m-`).  
  - SCSS: `.ComponentName__element--modifier` (BEM-like).

---

### 3. Accesibilidad (a11y)

- **Etiquetas semánticas**: usa `<button>`, `<nav>`, `<header>`.  
- **Roles y ARIA**: añade `role="dialog"` a modales, `aria-label` en iconos sin texto.  
- **Contraste**: mínimo AA (4.5:1) para texto normal.  
- **Teclado**: todos los elementos interactivos deben ser foco-navegables (`tabindex`).  
- **Formularios**: asocia `<label>` a `<input>` con `htmlFor` y `id`.

---

### 4. Temas y tokens de diseño

Define variables en CSS o tema de Tailwind para colores y tipografía:

```js
// tailwind.config.js
module.exports = {
  theme: {
    colors: {
      primary: '#4D4DFF',
      secondary: '#FFB800',
      danger: '#E02424',
      // ...
    },
    screens: {
      sm: '640px',
      md: '768px',
      lg: '1024px'
    }
  }
}
```

---

### 5. Acciones de lint y formateo
    - ESLint + Prettier integrados.
    - Script en package.json:
```Bash
npm run lint
npm run format
```