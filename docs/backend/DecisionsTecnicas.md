---
sidebar_position: 2
title: Decisiones Técnicas
---

## Decisiones y Justificaciones Técnicas - Backend

### 1. Elección de la Plataforma Base: .NET (ASP.NET Core)

#### Fundamento Principal: Eficiencia en el Desarrollo y Dominio Tecnológico

La elección de **.NET** como plataforma para el backend se fundamentó en mi **sólido conocimiento y amplia experiencia** con este ecosistema. Esta familiaridad me permitió **acelerar significativamente** el ciclo de desarrollo, concentrándome en la implementación de la lógica de negocio compleja y las características distintivas de la aplicación de reservas, en lugar de dedicar tiempo a la curva de aprendizaje de nuevas tecnologías.

Esta decisión **aseguró una alta productividad y eficiencia**, lo cual fue crucial para entregar un producto robusto y bien estructurado en un marco de tiempo optimizado para un proyecto de portafolio.

#### Beneficios de .NET para el Proyecto

* **Rendimiento y Escalabilidad:** ASP.NET Core es conocido por su **alto rendimiento**, un factor crítico para una aplicación de reservas. Esto permite manejar un número significativo de solicitudes concurrentes y picos de tráfico sin degradación, asegurando una experiencia de usuario fluida y una respuesta rápida de la API.
* **Robustez y Seguridad:** La plataforma .NET proporciona un conjunto maduro de herramientas y patrones para construir **aplicaciones seguras y robustas**. Esto es fundamental para proteger los datos de los usuarios, la información de las reservas y asegurar la integridad de todas las transacciones.
* **Desarrollo de API Eficiente:** Con ASP.NET Core Web API, la creación de **servicios RESTful limpios, bien definidos y eficientes** se simplifica enormemente, facilitando una comunicación fluida y confiable con el frontend en React.
* **Mantenibilidad y Extensibilidad:** La estructura orientada a objetos y los patrones de diseño comunes en .NET facilitan la creación de **código organizado y fácil de mantener**, lo que es esencial para futuras expansiones y la adición de nuevas funcionalidades al sistema de reservas.

---

### 2. Elección de Componentes y Librerías Específicas del Ecosistema .NET

#### Entity Framework Core (ORM)

Para la interacción con la base de datos, se eligió **Entity Framework Core (EF Core)** como el ORM (Object-Relational Mapper). Esta decisión se basó en su **eficiencia, madurez y la productividad** que ofrece al mapear objetos de C# a la base de datos relacional.

#### ASP.NET Core Identity (Gestión de Usuarios y Autenticación)

Para la gestión de usuarios, autenticación y autorización, se optó por **ASP.NET Core Identity**. Esta elección se justificó por ser una **solución probada, segura y altamente configurable** que cumple con los estándares de seguridad modernos (como el hashing de contraseñas y la gestión de roles), integrándose de forma nativa y simplificando la implementación de estos aspectos críticos.

---

### 3. Consideración de Alternativas (Opcional, pero recomendable brevemente)

Aunque el panorama de desarrollo backend ofrece excelentes alternativas como Node.js (con Express), Python (con Django/Flask) o Java (con Spring Boot), la elección de .NET me permitió **capitalizar al máximo mis habilidades existentes**. Esto se tradujo directamente en la capacidad de construir una **solución robusta y eficiente**, priorizando la calidad del producto final y la demostración de mis capacidades en un entorno familiar y de alto rendimiento, ideal para los objetivos de este proyecto de portafolio.