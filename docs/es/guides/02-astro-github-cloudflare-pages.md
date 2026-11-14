# Guía 02 — Astro, GitHub y Cloudflare Pages

[Inicio](../../../README.es.md) | [Arquitectura](../architecture.md) | [English](../../en/guides/02-astro-github-cloudflare-pages.md)

## 1. Objetivo

Crear, versionar y publicar una web estática mediante Astro, GitHub y Cloudflare Pages.

Esta guía explica un proyecto nuevo y reproducible. JH-Web — Personal Digital Platform es la implementación de referencia, pero su repositorio privado no se distribuye como plantilla.

## 2. Arquitectura de despliegue

Desarrollo local → Git → GitHub → Cloudflare Pages → Visitante.

El dominio personalizado se configurará en una guía posterior. Cloudflare Pages proporciona inicialmente una dirección `pages.dev`.

## 3. Requisitos

- Node.js compatible con la versión de Astro elegida.
- npm y Git instalados.
- Editor de código.
- Cuenta de GitHub.
- Cuenta de Cloudflare.

Comprobar:

```powershell
node --version
npm.cmd --version
git --version
```

Consultar los requisitos actuales en la documentación oficial de Astro. No asumir que una versión específica de Node.js será compatible indefinidamente.

## 4. Crear el proyecto

En Windows PowerShell:

```powershell
npm.cmd create astro@latest
```

Seguir el asistente interactivo, escoger una carpeta y seleccionar las opciones adecuadas.

Cuando termine:

```powershell
Set-Location ".\nombre-del-proyecto"
npm.cmd install
npm.cmd run dev
```

Si el asistente ya instaló las dependencias, no es necesario repetir la instalación.

Abrir la dirección local indicada por Astro, normalmente `http://localhost:4321`.

En macOS o Linux, utilizar `npm` en lugar de `npm.cmd`.

## 5. Estructura básica

Una estructura habitual es:

```text
public/
src/
  pages/
  components/
  layouts/
astro.config.mjs
package.json
package-lock.json
```

La estructura exacta depende de la plantilla y de las integraciones seleccionadas.

- `src/pages`: rutas y páginas.
- `src/components`: componentes reutilizables.
- `src/layouts`: estructuras compartidas.
- `public`: recursos públicos sin procesamiento.
- `package.json`: scripts y dependencias.
- `astro.config.mjs`: configuración, si utiliza ese nombre.

## 6. Contenido y componentes

Crear páginas dentro de `src/pages` y reutilizar componentes cuando corresponda.

Astro admite contenidos Markdown; los mecanismos de organización pueden variar según la versión y las necesidades del proyecto.

Para un sitio bilingüe, planificar las rutas, la navegación, los metadatos y la gestión de traducciones antes de publicar contenidos extensos.

## 7. Compilación local

Ejecutar:

```powershell
npm.cmd run build
npm.cmd run preview
```

El script `build` debe existir en `package.json`.

En un proyecto Astro estático con la configuración habitual, los archivos generados se encuentran en `dist/`.

Comprobar que las páginas, imágenes y enlaces funcionan en la versión compilada, no únicamente durante el desarrollo.

## 8. Control de versiones

Verificar el estado del repositorio:

```powershell
git status
```

Si el proyecto todavía no es un repositorio Git:

```powershell
git init
git branch -M main
```

Revisar `.gitignore` antes de añadir archivos. En particular, excluir dependencias instaladas, archivos de entorno con secretos y otros recursos privados.

Para preparar una publicación, tras revisar los cambios:

```powershell
git add .
git diff --cached --check
git diff --cached --stat
git commit -m "feat: initialize Astro website"
```

`git add .` es apropiado únicamente después de revisar qué archivos se incluirán.

## 9. Publicar en GitHub

Crear un repositorio nuevo en GitHub.

Si el proyecto local ya contiene un commit, utilizar la URL del repositorio propio:

```powershell
git remote add origin https://github.com/USUARIO/REPOSITORIO.git
git push -u origin main
```

Sustituir `USUARIO` y `REPOSITORIO`.

Si `origin` ya existe, revisar su configuración con `git remote -v` antes de modificarlo.

No subir claves, credenciales, archivos `.env` privados ni información de terceros.

## 10. Configurar Cloudflare Pages

Desde el panel de Cloudflare:

1. Abrir Workers & Pages.
2. Crear una aplicación Pages.
3. Seleccionar la integración con Git.
4. Autorizar el acceso al repositorio necesario.
5. Elegir el repositorio.
6. Establecer la configuración de compilación.
7. Guardar y desplegar.

Configuración habitual para Astro estático:

| Parámetro | Valor |
|---|---|
| Rama de producción | `main` |
| Comando de compilación | `npm run build` |
| Directorio de salida | `dist` |
| Directorio raíz | Raíz del proyecto, salvo estructura distinta |

Elegir la versión de Node.js admitida por el proyecto y disponible en el entorno de compilación de Cloudflare.

Si se utiliza un monorepositorio, revisar cuidadosamente el directorio raíz.

## 11. Despliegues automáticos

Con la integración Git habilitada, los commits enviados a la rama de producción desencadenan nuevas compilaciones y despliegues.

Las ramas y solicitudes de incorporación de cambios pueden disponer de vistas previas, según la configuración del proyecto.

Flujo recomendado:

1. Editar localmente.
2. Compilar.
3. Revisar cambios.
4. Crear commit.
5. Enviar a GitHub.
6. Revisar el resultado en Cloudflare.
7. Comprobar la web publicada.

## 12. Validar producción

Verificar:

- Despliegue completado sin errores.
- Página principal y rutas secundarias.
- Enlaces de navegación.
- Imágenes y archivos estáticos.
- Metadatos y títulos.
- Visualización móvil y escritorio.
- Idiomas, si corresponde.
- Ausencia de secretos expuestos.

La aparición del estado de despliegue correcto no sustituye las pruebas funcionales de la web.

## 13. Problemas habituales

| Síntoma | Comprobación |
|---|---|
| `npm` bloqueado en PowerShell | Probar `npm.cmd` sin cambiar innecesariamente la política de ejecución |
| Fallo de dependencias | Revisar versión de Node.js y archivo de bloqueo |
| Compilación fallida | Consultar el registro completo de compilación |
| Página no encontrada | Revisar rutas y generación de páginas |
| Imágenes ausentes | Revisar rutas y diferencias entre mayúsculas y minúsculas |
| Sitio antiguo | Confirmar rama, commit y despliegue activo |
| Directorio incorrecto | Revisar la raíz y la salida configuradas en Pages |

## 14. Seguridad y mantenimiento

- Habilitar MFA en GitHub y Cloudflare.
- Limitar los permisos de integración.
- No almacenar secretos en código público.
- Revisar dependencias y actualizaciones.
- Mantener un historial Git recuperable.
- Evaluar la recuperación de configuraciones y DNS.

Este procedimiento no constituye por sí mismo un plan completo de recuperación ante desastres.

## 15. Resultado esperado

Una web estática accesible mediante Cloudflare Pages, vinculada a un repositorio GitHub y preparada para posteriores actualizaciones.

La siguiente guía cubrirá el dominio personalizado y los DNS.

## 16. Referencias oficiales

- [Instalación de Astro](https://docs.astro.build/es/install-and-setup/)
- [Cloudflare Pages para Astro](https://developers.cloudflare.com/pages/framework-guides/deploy-an-astro-site/)
- [Integración Git de Cloudflare Pages](https://developers.cloudflare.com/pages/get-started/git-integration/)
- [Implementación de referencia JH-Web](https://juanherbozo.com/es/projects/jh-web-personal-digital-platform/)