# 🚀 Event Reservation App - Documentación Centralizada

¡Bienvenido al repositorio de documentación centralizada para la **Aplicación de Reserva de Recursos para Eventos**!

Este proyecto personal está diseñado para mi portafolio y tiene como objetivo demostrar la capacidad de construir una aplicación completa, desde el frontend hasta el backend, con una sólida base de documentación.

---

## 🎯 Visión General del Proyecto

La `Event Reservation App` es una aplicación que permite a los usuarios **reservar recursos** (como salas, equipos, personal, etc.) para diferentes tipos de eventos. Está pensada para ser una solución integral que simplifique la gestión y disponibilidad de estos recursos.

### Arquitectura General

El proyecto está modularizado en **tres repositorios principales** para una mejor gestión, escalabilidad y separación de responsabilidades:

1.  **Frontend (`React`):** La interfaz de usuario intuitiva para la interacción con la aplicación.
2.  **Backend (`.NET`):** La lógica de negocio y la API que gestiona las operaciones de reserva.
3.  **Documentación (`Docusaurus`):** Este mismo repositorio, que actúa como el centro de conocimiento del proyecto.

---

## 📂 Estructura de Repositorios

A continuación, puedes encontrar los enlaces directos a cada componente del proyecto:

* **[EventReservationAppFrontend](https://github.com/jose-angell/EventReservationAppFrontend)**
    * **Tecnología:** React
    * **Descripción:** Contiene el código fuente de la interfaz de usuario, los componentes y la lógica de presentación.
* **[EventReservationAppBackend](https://github.com/jose-angell/EventReservationAppBackend)**
    * **Tecnología:** .NET
    * **Descripción:** Contiene la API RESTful, la lógica de negocio, la conexión a la base de datos y la autenticación.
* **[EventReservationAppDocs](https://github.com/jose-angell/EventReservationAppDocs)** (¡Estás aquí!)
    * **Tecnología:** Docusaurus
    * **Descripción:** Centro de documentación, guías de contribución, arquitectura, planificación y detalles técnicos.

---

## 📖 Navegando por la Documentación

Este repositorio utiliza **Docusaurus** para generar un sitio web de documentación interactivo y fácil de navegar. Aquí encontrarás:

* **Introducción al Proyecto:** Objetivos, alcance y tecnologías utilizadas.
* **Guías de Configuración:** Cómo poner en marcha el frontend y el backend localmente.
* **Diagramas de Arquitectura:** Visualizaciones de la estructura del sistema y los flujos de datos.
* **Documentación de la API:** Detalles de los endpoints del backend, peticiones y respuestas.
* **Guías de Contribución:** Normas de estilo de código, proceso de Pull Requests y cómo colaborar.
* **Decisiones de Diseño:** Explicación de las elecciones tecnológicas y arquitectónicas.
* **Roadmap:** Próximas funcionalidades y planificación a futuro.

---

### Cómo Acceder a la Documentación Completa

Puedes acceder al sitio de documentación publicado en GitHub Pages a través del siguiente enlace:

🔗 **[Visitar la Documentación](https://jose-angell.github.io/EventReservationAppDocs/)**

---

## 🤝 Contribución

¡Las contribuciones son bienvenidas! Si deseas colaborar con el desarrollo de la aplicación o mejorar la documentación, por favor, consulta nuestras **[Guías de Contribución](docs/contributing.md)** *(o el path que decidas dentro de Docusaurus)*.

### ¿Cómo contribuir a la documentación de Docusaurus?

Si necesitas modificar o añadir contenido a este sitio de documentación, sigue estos pasos:

#### Pre-requisitos para Desarrollar Docusaurus

* **Node.js** (versión 16 o superior) y **npm** o **Yarn**.
* **Git** instalado en tu sistema.
* Una **cuenta de GitHub**.


#### Configuración Local de Docusaurus

1.  **Clonar el repositorio:**
    ```bash
    git clone [https://github.com/jose-angell/EventReservationAppDocs.git](https://github.com/jose-angell/EventReservationAppDocs.git)
    cd EventReservationAppDocs
    ```
2.  **Instalar dependencias:**
    ```bash
    npm install # o yarn install
    ```
3.  **Iniciar el servidor de desarrollo:**
    ```bash
    npm run start
    ```
    Accede a `http://localhost:3000/EventReservationAppDocs/` para ver el sitio.

#### Despliegue en GitHub Pages

Para desplegar los cambios en el sitio público de GitHub Pages, utiliza el script de despliegue de Docusaurus:

```bash
$env:GIT_USER = "tu-usuario-de-github"; npm run deploy
```

> (Asegúrate de reemplazar "tu-usuario-de-github" con tu nombre de usuario de GitHub.)

Este comando generará el sitio estático y lo subirá a la rama gh-pages de este repositorio.

---
✨ Tecnologías Utilizadas
  - Frontend: React
  - Backend: .NET
  - Documentación: Docusaurus, Markdown, Mermaid (para diagramas)
  - Control de Versiones: Git, GitHub

---

### 📧 Contacto
Para cualquier pregunta o sugerencia, no dudes en abrir un Issue en GitHub o contactarme directamente a través de **gallardocordovajoseangel@gmail.com**.

---

