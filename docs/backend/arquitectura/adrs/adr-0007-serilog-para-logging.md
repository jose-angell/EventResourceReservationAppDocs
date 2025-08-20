---
sidebar_position: 8
title: ADR 0007:Implementación de Serilog para Logging
---

# ADR 0007:  Implementación de Serilog para Logging

* **Fecha:** 2025-08-18
* **Estado:** [Aceptada]

## Contexto

En una aplicación moderna, un sistema de logging robusto es esencial para la depuración, el monitoreo y la comprensión del comportamiento en producción. La plataforma ASP.NET Core ya tiene un sistema de logging integrado (`Microsoft.Extensions.Logging`) que proporciona una interfaz común (`ILogger<T>`). Sin embargo, su implementación por defecto para la consola o debug no es adecuada para un entorno de producción que requiere formatos estructurados y destinos de log avanzados (como archivos, bases de datos o servicios de agregación).

Para `EventResourceReservationApp`, necesitamos una solución de logging que sea:
    - **Estructurada**: Para facilitar la búsqueda y el análisis.
    - **Flexible**: Para enviar logs a múltiples destinos (consola en desarrollo, un servicio de agregación en producción).
    - **Eficiente**: Con un bajo impacto en el rendimiento.
    - **Desacoplada**: Para no introducir dependencias directas en las capas de Dominio y Aplicación.

## Decisión

Se ha decidido implementar Serilog como el motor de logging subyacente para la aplicación. Serilog será el proveedor concreto del sistema `Microsoft.Extensions.Logging`, lo que nos permitirá usar `ILogger<T>` en toda la aplicación, mientras aprovechamos las potentes características de Serilog.

- Integración con la Arquitectura Limpia:
    - **Capa de Infraestructura y Proyecto de API:** La configuración de Serilog y la instalación de sus paquetes (los llamados sinks) se realizará en el proyecto de la API, que es el punto de entrada de la aplicación. Esto mantiene a Serilog como un detalle de la infraestructura.
    - **Capa de Aplicación:** En los handlers y servicios de la capa de Aplicación, se inyectará la interfaz estándar de .NET, `ILogger<T>`. Esta capa solo sabrá cómo registrar logs, pero no qué tecnología subyacente se utiliza.
- Características Clave de Serilog a Utilizar:
    - **Sinks:** Se configurarán sinks para la consola (en desarrollo) y archivos (en producción). Se considerarán otros sinks como PostgreSQL o Seq en el futuro.
    - **Structured Logging:** Se usará el logging estructurado para registrar objetos y propiedades de forma organizada, lo que es invaluable para el análisis de logs. Por ejemplo, en lugar de `"Creando categoría con nombre: " + category.Name, se usará` `"Creando categoría con nombre: {CategoryName}"`, y Serilog almacenará CategoryName como un campo en el log.

## Alternativas Consideradas

- **Logging por defecto de .NET:** Se descartó porque su implementación básica es insuficiente para la producción. No ofrece un logging estructurado eficiente ni la flexibilidad necesaria para múltiples destinos sin escribir mucho código personalizado.
- **NLog:** Se consideró como una alternativa válida a Serilog. Ambos son excelentes para el logging estructurado. Se optó por Serilog debido a su popularidad y gran cantidad de sinks disponibles, pero la decisión de usar NLog en su lugar no habría afectado negativamente la arquitectura. La elección entre ambos es en gran parte una cuestión de preferencia.

## Consecuencias

Describe los pros y los contras de la decisión tomada. ¿Qué se gana y qué se pierde? ¿Qué implicaciones tiene para el futuro?

### Positivas (+)
- **Desacoplamiento:** El uso de `ILogger<T>` garantiza que las capas de Dominio y Aplicación no tengan dependencias directas de Serilog, permitiendo un cambio de proveedor de logging en el futuro si fuera necesario.
- **Rendimiento:** Serilog está optimizado para la escritura de logs en entornos de alto rendimiento.
- **Análisis de Datos:** El formato de log estructurado facilita enormemente el análisis y la visualización de logs en herramientas externas.
- **Flexibilidad:** La arquitectura de sinks de Serilog permite configurar fácilmente múltiples destinos para los logs sin cambiar el código de la aplicación.

### Negativas (-)
- **Complejidad de Configuración:** La configuración inicial puede ser más compleja que el logging por defecto. Sin embargo, esta es una inversión necesaria para la robustez a largo plazo.
- **Incompatibilidad:** Aunque poco común, puede haber incompatibilidad entre Serilog y algunos sinks si no se mantienen las versiones actualizadas.
-**Sobrecarga de Código:** La inyección de `ILogger<T>` en cada clase puede parecer repetitiva, pero es una práctica estándar que mejora la mantenibilidad y la capacidad de prueba.

---