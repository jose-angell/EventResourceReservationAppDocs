---
sidebar_position: 4
title: 05-tests
---

# 🧪 Pruebas

> Archivo: `05-tests.md`

## 1. Unit tests
<!-- Lista de clases y escenarios clave -->
| Clase de test               | Escenario                                       |
|-----------------------------|-------------------------------------------------|
| `<Feature>UseCaseTests`     | - Caso de éxito                                 |
|                             | - Excepción por validación                     |
|                             | - Error de integración con servicios externos  |

## 2. Integration tests
<!-- Endpoints, flujos completos, bases de datos in-memory -->
- Test de endpoint METHOD `/api/<ruta>` sin token → `401`.  
- Test con payload válido → `200` + datos.  
- Flujos de error (pago rechazado, sin stock).

## 3. Cobertura
<!-- Meta mínima de cobertura -->
> Objetivo: ≥ 80 % en use cases y controllers de esta feature.

---
