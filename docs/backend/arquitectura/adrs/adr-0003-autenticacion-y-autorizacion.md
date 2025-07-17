---
sidebar_position: 4
title: ADR 0003:Autenticación y Autorización con JWT y ASP.NET Core Identity
---

# ADR 0003: Autenticación y Autorización con JWT y ASP.NET Core Identity

* **Fecha:** 2025-07-16
* **Estado:** [Aceptada]

## Contexto

Existe la necesidad de implementar un sistema para manejar la **autenticación y autorización** de forma **segura, escalable** y que cumpla con el principio de `sin estado` de la arquitectura `REST`. Esto incluye la gestión de usuarios (`registro`, `login`, y `perfiles`), la protección de los `endpoints` de la `API`, y la diferenciación de permisos de usuarios según los `roles` existentes.

## Decisión

Decidimos implementar un sistema de autenticación y autorización basado en la combinación de `JSON Web Tokens (JWT) `para la autenticación de API y `ASP.NET Core Identity` para la gestión de usuarios, roles y la lógica de autorización.

## Alternativas Consideradas

* **Implementación de un sistema de usuarios/roles desde cero:** Descartado por complejidad, tiempo, seguridad - ASP.NET Core Identity ya lo hace de forma robusta.
* **Otro Identity Provider (ej. Auth0, Okta):** Descartado por sobre-complicación para un proyecto de portafolio, costo, o por querer demostrar implementación propia.
* **Autenticación basada en Sesiones/Cookies:** Descartado para APIs RESTful por ser con estado, menos escalable para múltiples servicios, problemas de CORS con SPAs.
* **OAuth 2.0 (completo flow):** Descartado por ser más complejo de implementar para el alcance inicial, aunque JWT puede ser parte de OAuth.

## Consecuencias

### Positivas (+)
* ASP.NET Core Identity proporciona un sistema probado y seguro para la gestión de credenciales y usuarios, mientras que JWT protege las comunicaciones.
* JWTs son sin estado, lo que facilita la escalabilidad horizontal de la API al no requerir sesiones en el servidor.
* Identity maneja la identidad del usuario y JWT la autenticación de la petición.
* JWT es un estándar que puede ser consumido fácilmente por diversas plataformas (frontend React, aplicaciones móviles, otros servicios).
* Ambas soluciones se integran muy bien con el ecosistema de ASP.NET Core.
* ASP.NET Core Identity permite implementar autorización basada en roles o políticas de forma efectiva.

### Negativas (-)
* Configurar y entender JWT y ASP.NET Core Identity a fondo puede tener una curva de aprendizaje inicial.
* Por ser sin estado, la revocación de tokens antes de su expiración requiere soluciones adicionales (listas negras, tokens de refresco).
* ASP.NET Core Identity requiere una base de datos para almacenar usuarios y roles.
* La configuración de ambos sistemas y la coordinación de su interacción puede ser más compleja que soluciones más simples.
