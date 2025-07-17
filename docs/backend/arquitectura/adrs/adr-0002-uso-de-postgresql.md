---
sidebar_position: 3
title: ADR 0002:Uso de PostgreSQL  
---

# ADR 0002: Uso de PostgreSQL   

* **Fecha:** 2025-07-15
* **Estado:** [Aceptada]

## Contexto

La **Aplicación de Reserva de Recursos para Eventos** requiere un sistema de almacenamiento de datos **estructurado, transaccional y confiable** para manejar información crítica como:
    * **Reservas de recursos:** Fechas, horarios, usuarios, recursos específicos.
    * **Gestión de usuarios:** Perfiles, roles, autenticación.
    * **Inventario de recursos:** Detalles de salas, equipos, capacidad, disponibilidad.
    * **Eventos:** Tipos de eventos, descripciones.

## Decisión

Decidimos utilizar **PostgreSQL** como nuestro motor de base de datos relacional principal. Esta elección se basa en su **solidez, cumplimiento de estándares SQL, naturaleza de código abierto y su reputación de confiabilidad y rendimiento**, lo que lo hace ideal para gestionar las relaciones y la integridad de datos requeridas por la aplicación de reservas.

## Alternativas Consideradas

* **SQL Server:** Un motor de base de datos relacional robusto y confiable, ampliamente utilizado en entornos empresariales. Si bien es potente, fue descartado para este proyecto de portafolio debido a su naturaleza **propietaria y los costos de licenciamiento** asociados, lo cual no se alinea con la filosofía de un desarrollo de código abierto o un despliegue sin costos adicionales.
* **MongoDB:** Un motor de base de datos NoSQL orientado a documentos, conocido por su flexibilidad de esquema y alta velocidad en lecturas/escrituras. Sin embargo, no es la opción ideal para este proyecto debido a la falta de un esquema estricto por defecto y la dificultad en mantener la **integridad referencial** lo hacen menos adecuado para un sistema donde la consistencia de datos es crítica.

## Consecuencias

### Positivas (+)

*  El modelo relacional y las características de PostgreSQL aseguran la integridad y consistencia transaccional de los datos, crucial para las reservas.
* Excelente soporte y herramientas para Object-Relational Mappers (ORMs) como Entity Framework Core en .NET, lo que facilita el desarrollo de operaciones CRUD y la interacción con la base de datos.
* PostgreSQL es conocido por su capacidad de escalar y ofrecer un rendimiento robusto para cargas de trabajo variadas.
* Amplia comunidad de soporte, excelente documentación y un rico ecosistema de herramientas.
* Facilita la evolución del modelo de datos a lo largo del tiempo de forma controlada.
* Alineado con la filosofía de proyectos de portafolio y open source.

### Negativas (-)

* Para desarrolladores no familiarizados con PostgreSQL o bases de datos relacionales, puede haber una curva de aprendizaje inicial en términos de SQL, optimización de consultas y características específicas.
* Comparado con soluciones NoSQL o bases de datos integradas, la configuración inicial de un servidor PostgreSQL puede requerir más pasos (instalación, gestión de usuarios, configuración de red).
* Aunque es un pro para la integridad, la necesidad de definir y migrar esquemas para cambios estructurales puede ser más laboriosa que la flexibilidad de un esquema sin esquema de NoSQL para iteraciones muy rápidas.

