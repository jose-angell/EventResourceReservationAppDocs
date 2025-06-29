
# 🛠 Guía para creación de nuevas Features (Backend)

Este documento define el flujo de trabajo y plantilla de documentación para cada nueva feature de la aplicación .NET Core.

---

## 1. Flujo de trabajo

1. **Definir y Specificar**  
   - Crea un folder en `docs/backend/Features/<nombre-feature>/`.  
   - Copia el template `00-template.md` y renómbralo a `01-overview.md`.  
   - Rellena `01-overview.md` con objetivo, contexto y criterios BDD.

2. **Abrir un Issue en GitHub**  
   - Usa la etiqueta `backend`, `feature`.  
   - En la descripción incluye:  
     - Título corto y descriptivo.  
     - Link al spec: `docs/backend/Features/<nombre-feature>/01-overview.md`.  
     - Checklist de “ready for dev”.

3. **Crear rama de Feature**  
   ```bash
   git checkout -b feature/<nombre-feature>
   ```
4. **Documentar en paralelo**

   - A medida que avances, actualiza los archivos dentro de `docs/backend/Features/<nombre-feature>/`:
     - 02-modelos.md (esquema BD y migraciones)
     - 03-api.md (endpoints, request/response)
     - 04-usecases.md (casos de uso y diagrama de secuencia)
     - 05-seguridad.md (roles, validaciones)
     - 06-tests.md (estrategia de pruebas)

5. **Implementar el código**

    - Domain: nuevas entidades o agregados.
    - Application: interfaces y clases de UseCases.
    - Infrastructure: repositorios EF, clientes HTTP/Stripe, worker si aplica.
    - Añade tests unitarios en tests/Unit, y tests de integración en tests/Integration.

6. **Crear Pull Request**

    - En la plantilla de PR menciona:
       - Issue resuelto (Closes #`<número>`).
       - Spec actualizado (links a MD).
       - Verificación local: builds, tests verdes.
    - Asigna revisores y code-owners.

7. **Merge & Deploy**
    - Tras aprobación y CI verde, haz merge en develop o main según tu flujo.
    - CI/CD ejecutará migraciones, tests y despliegue.