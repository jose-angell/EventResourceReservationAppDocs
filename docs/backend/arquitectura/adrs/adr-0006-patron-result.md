---
sidebar_position: 1
title: ADR 0006:Implementación del Patrón Result
---

# ADR 0006:   Implementación del Patrón Result

* **Fecha:** 2025-07-29
* **Estado:** [Aceptada]

## Contexto

En una aplicación, las operaciones pueden fallar por diversas razones: errores de validación, dependencias de bases de datos que no responden, o violaciones de reglas de negocio. La forma tradicional de manejar estos fallos es lanzando excepciones. Sin embargo, el uso excesivo de excepciones para flujos de error esperados (como una validación que no pasa) tiene varios inconvenientes:
    - **Sobrecarga de rendimiento:** Las excepciones son costosas en términos de recursos.
    - **Flujos de código menos claros:** Obligan al desarrollador a usar bloques try-catch, lo que puede ocultar el flujo de control normal de la aplicación y dificultar el entendimiento del código.
    - **Acoplamiento de la lógica:** El manejo de excepciones a menudo obliga a la capa de aplicación a depender de detalles de implementación de las capas internas para saber qué excepciones capturar.
    - **Manejo de errores inconsistente:** Cada método puede lanzar diferentes tipos de excepciones, lo que lleva a un manejo de errores irregular en todo el código.
Para EventResourceReservationApp, se necesita un enfoque consistente, robusto y explícito para manejar los flujos de error que son parte del dominio de la aplicación (ej., "el nombre de la categoría ya existe").

## Decisión

Se ha decidido implementar el **Patrón Result** en toda la capa de **Aplicación**.
Este patrón se basa en devolver un objeto que representa el resultado de una operación, en lugar de lanzar una excepción en caso de fallo. Este objeto `OperationResult<T>` o simplemente `OperationResult` encapsulará el éxito o el fracaso de una operación, junto con los datos resultantes en caso de éxito y los detalles del error en caso de fallo.
    - **Estructura del `OperationResult`:** La implementación básica consistirá en una clase o struct que contenga:
        - Un booleano `IsSuccess` para indicar si la operación fue exitosa.
        - Un valor genérico `TValue` para los datos de éxito.
        - Una colección de `Error` que contendrá información detallada sobre el fallo (ej., código de error, mensaje descriptivo).
        - Un codigo de error `ErrorCode` para facilitarr el manejo de los errores en la capa de presentacion.
    - **Uso del Patrón:**
        - **Creación:** Los servicios de la capa de Aplicación y los casos de uso devolverán un objeto `OperationResult` o `OperationResult<T>`.
        - **Manejo de errores:** El código cliente (ej., un Controller) verificará la propiedad `IsSuccess` del objeto `OperationResult`. Si es false, leerá el ErrorCode para decidir cómo responder (ej., un HTTP 400 Bad Request con el mensaje de error).

## Alternativas Consideradas

* **Uso extensivo de Excepciones:** Se descartó debido a los problemas de rendimiento, la falta de claridad en el flujo del código y la inconsistencia en el manejo de errores que introduce.
* **Uso de tuplas (`(bool isSuccess, TValue value, string errorMessage)`):** Se consideró, pero se descartó por ser un enfoque menos explícito y más propenso a errores. El patrón Result proporciona una clase dedicada y semánticamente rica que encapsula mejor la intención del diseño.

## Consecuencias

Describe los pros y los contras de la decisión tomada. ¿Qué se gana y qué se pierde? ¿Qué implicaciones tiene para el futuro?

### Positivas (+)
* **Código más explícito:** La intención del código es clara, ya que el resultado esperado es un Result que puede fallar. Esto elimina la ambigüedad de si un método puede lanzar una excepción inesperada.
* **Rendimiento mejorado:** Se evita la sobrecarga de rendimiento que conllevan las excepciones, ya que los errores esperados se manejan como un flujo de control normal.
* **Manejo de errores consistente:** Al usar un tipo Result unificado, se estandariza el manejo de errores en toda la aplicación, haciendo el código más predecible y fácil de mantener.
* **Integración con la API:** Facilita la traducción de errores de la lógica de negocio a respuestas HTTP (códigos de estado y cuerpos de error), ya que la información del fallo está explícita en el objeto `OperationResult`.

### Negativas (-)
* **Más verbosidad:** La sintaxis puede ser ligeramente más verbosa que simplemente lanzar una excepción. Se requiere más código para crear y verificar el objeto `OperationResult`.
* **Curva de aprendizaje:** Puede haber una curva de aprendizaje inicial para los desarrolladores que no están familiarizados con este patrón.
* **Propagación del error:** Si un método no verifica el `OperationResult` de una operación interna, un error puede propagarse silenciosamente a través del sistema si no se maneja correctamente, a diferencia de las excepciones que detienen la ejecución inmediatamente. Sin embargo, se pueden aplicar buenas prácticas de propagación del resultado para mitigar este riesgo.

---