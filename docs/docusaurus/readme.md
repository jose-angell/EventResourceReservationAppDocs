# Guía Completa y detallada para la crear, Configurar y Desplegar Docusaurus para documentar un proyecto(en GitHub Pages)

Esta guía te llevará paso a paso desde la creación del proyecto en Docusaurus hasta su despliegue en GitHub Pages, incluyendo la creación manual de la rama `gh-pages`.

---

## 1. Pre-requisitos

- **Node.js** (versión 16 o superior) y **npm** o **Yarn**.
- **Git** instalado en tu sistema.
- Una **cuenta de GitHub**.

---

## 2. Crear el Proyecto Docusaurus

Abre la terminal y ejecuta:

```bash
npx create-docusaurus@latest mi-documentacion classic
```
Esto creará una carpeta llamada mi-documentacion con la estructura básica de Docusaurus. Luego, ingresa al directorio:

```
cd mi-documentacion
```

3. Configurar docusaurus.config.js
Abre el archivo docusaurus.config.js y edita las siguientes propiedades según corresponda:

- url: La URL base de tu GitHub Pages.

- baseUrl: La subruta donde se alojará tu sitio (incluye barra al inicio y al final).

- organizationName: Tu usuario u organización en GitHub.

- projectName: El nombre de tu repositorio.

Ejemplo de configuración:

``` js
module.exports = {
  title: 'Mi Documentación',
  tagline: 'Documentación de ejemplo con Docusaurus',
  url: 'https://tu-usuario.github.io', // Reemplaza "tu-usuario" según corresponda
  baseUrl: '/mi-documentacion/',         // Debe coincidir con el nombre de tu repositorio
  onBrokenLinks: 'throw',
  onBrokenMarkdownLinks: 'warn',
  favicon: 'img/favicon.ico',

  organizationName: 'tu-usuario',   // Usuario o organización de GitHub
  projectName: 'mi-documentacion',   // Nombre del repositorio en GitHub

  // Otras configuraciones...
};

```

4. Probar el Sitio Localmente
Modo Desarrollo
Inicia el servidor de desarrollo:

```
npm run start
```

Accede a http://localhost:3000/mi-documentacion/ para ver el sitio (recuerda usar la ruta que incluya el baseUrl).

Generar Build para Producción
Construye el sitio y pruébalo localmente:
```
npm run build
npm run serve
```

5. Configurar el Repositorio en GitHub
Inicializar Git y Hacer el Primer Commit

```
git init
git add .
git commit -m "Primer commit: configuración inicial de Docusaurus"
```

Crear el Repositorio en GitHub
1.  Ve a GitHub y crea un nuevo repositorio llamado, por ejemplo, mi-documentacion.

Vincular el Repositorio Remoto y Subir el Código

```
git remote add origin https://github.com/tu-usuario/mi-documentacion.git
git branch -M main
git push -u origin main
```
Reemplaza tu-usuario y mi-documentacion con tus datos.

6. Crear Manualmente la Rama gh-pages
Docusaurus utiliza la rama gh-pages para almacenar el sitio estático. Si aún no existe en el remoto, créala manualmente:

1. Crear una rama huérfana llamada gh-pages:
``` 
git checkout --orphan gh-pages
 ```
2. Eliminar todos los archivos existentes:
``` 
git rm -rf .
```
3. Crear el archivo .nojekyll:

Para evitar que GitHub Pages procese el contenido con Jekyll, crea un archivo vacío llamado .nojekyll:
```
echo "" > .nojekyll
git add .nojekyll
git commit -m "Inicializar rama gh-pages"
```

4. Subir la rama gh-pages al repositorio remoto:
```
git push origin gh-pages
```
5. Volver a la rama principal:
```
git checkout main
```

7. Desplegar el Sitio en GitHub Pages
Docusaurus provee un script de despliegue para generar el build y subirlo a la rama gh-pages.

Ejecuta:
```
 $env:GIT_USER = "user-name"; npm run deploy
```

El comando hará lo siguiente:

- Generará el build (carpeta build).

- Clonará la rama gh-pages desde el remoto.

- Actualizará el contenido con el resultado del build.

Una vez completado, tu sitio estará disponible en:
```
https://tu-usuario.github.io/mi-documentacion/
```

Espera unos minutos para que GitHub Pages actualice el contenido.


### Agregar Diagragas
1. Instalar el plugin:
```
npm install --save @docusaurus/theme-mermaid
```

2. Habilitar Mermaid en la configuración En tu archivo ```docusaurus.config.js```, agrega:

```
export default {
  markdown: { mermaid: true },
  themes: ['@docusaurus/theme-mermaid'],
};

```
