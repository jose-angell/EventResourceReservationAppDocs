---
sidebar_position: 5
title: ADR 0004:Adopción de Clean Architecture
---

# ADR 0004: Adopción de Clean Architecture

* **Fecha:** 2025-07-18
* **Estado:** [Aceptada]

## Contexto

A medida que el proyecto `Event Resource Reservation App Backend` crece, necesitamos una estructura de aplicación que promueva la mantenibilidad, la escalabilidad, la testeabilidad y la independencia de las tecnologías externas y los frameworks. Es crucial establecer una base arquitectónica robusta que facilite la evolución del software, la adición de nuevas características y la colaboración de futuros equipos de desarrollo.

## Decisión

Decidimos adoptar e implementar **Clean Architecture** (o Arquitectura Limpia) como el patrón arquitectónico principal para el desarrollo del backend de la `Event Resource Reservation App`. Esta arquitectura se basará en el principio de inversión de dependencias, organizando el código en capas donde las dependencias fluyen siempre hacia el interior

## Alternativas Consideradas

- **Arquitectura Tradicional de N-Capas (Ej. 3 Capas: UI/API, BLL, DAL):**
    - **Ventajas:** Familiar para muchos desarrolladores, configuración inicial más rápida.
    - **Desventajas:** A menudo conduce a un acoplamiento más estrecho entre capas, dificultando el cambio de frameworks o bases de datos sin afectar otras partes. La lógica de negocio puede mezclarse con detalles de infraestructura, reduciendo la testeabilidad y mantenibilidad.
- **Arquitectura Monolítica Simple (sin capas definidas explícitas):**
    - **Ventajas:** Rapidez en el inicio para proyectos muy pequeños.
    - **Desventajas:** Rápida acumulación de deuda técnica, muy difícil de mantener y escalar a medida que el proyecto crece, baja testeabilidad, y un alto acoplamiento que impide la refactorización sin alto riesgo.
- **Microservicios:**
    - **Ventajas:** Máxima escalabilidad, independencia de despliegue y tecnología por servicio, aislamiento de fallos.
    - **Desventajas:** Mayor complejidad operativa (despliegue, monitoreo, comunicación distribuida), sobre-ingeniería para el tamaño inicial de este proyecto de portafolio, overhead en la comunicación entre servicios. Se consideró que la complejidad inicial superaba los beneficios para la fase actual.

## Consecuencias

### Positivas (+)
* El código del dominio y la lógica de negocio central (reglas empresariales) son independientes de frameworks, bases de datos o interfaces de usuario, lo que permite cambiarlos con mínimo impacto.
*  Al separar claramente las preocupaciones, especialmente la lógica de negocio de la infraestructura, se facilita la escritura de pruebas unitarias robustas y rápidas.
* La clara separación de responsabilidades y la dirección de las dependencias hacen que el código sea más fácil de entender, mantener y modificar a lo largo del tiempo.
* Aunque es un monolito, la organización de Clean Architecture facilita el futuro desacoplamiento en microservicios si fuera necesario, ya que las fronteras lógicas están bien definidas.
* Cada capa tiene un propósito bien definido, lo que ayuda a los desarrolladores a saber dónde colocar cada pieza de código.

### Negativas (-)
* Requiere una comprensión más profunda de los principios de diseño y una configuración inicial más estructurada (más proyectos en la solución, más interfaces). Esto puede ralentizar el inicio para desarrolladores no familiarizados.
* Puede requerir más archivos y carpetas, y la creación de interfaces y modelos de datos específicos para cada capa, lo que puede parecer excesivo para funcionalidades muy pequeñas.
* Mantener múltiples proyectos en la solución (.NET) puede añadir una ligera complejidad a la configuración del IDE y la gestión de referencias.
