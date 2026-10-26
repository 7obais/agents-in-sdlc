---
title: "Lección 0 - Requisitos previos"
description: "Crea tu copia de Tailspin Toys y prepara un GitHub Codespace para el taller de Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Antes de empezar las lecciones de Copilot CLI, tienes que preparar todo lo necesario. Crearás tu propia copia del repositorio de Tailspin Toys y abrirás un [Codespace][codespaces]. En la siguiente lección utilizarás su terminal integrado para instalar y ejecutar Copilot CLI.

En esta lección:

- crearás una copia propia del proyecto Tailspin Toys a partir de la plantilla;
- crearás un Codespace y comprobarás que el proyecto está listo.

## Configurar el repositorio del taller

Trabajarás con tu propia copia del proyecto Tailspin Toys. Créala ahora desde el [repositorio de plantilla][tailspin-template]. El nuevo repositorio contiene todos los archivos que necesita el taller.

1. En una ventana nueva del navegador, ve a la [plantilla de Tailspin Toys][tailspin-template].
2. Crea tu copia del repositorio seleccionando **Use this template** y, después, **Create a new repository**.
3. Si participas en un evento organizado por GitHub o Microsoft, sigue las instrucciones de los mentores. De lo contrario, crea el repositorio en una organización en la que tengas acceso a GitHub Copilot.
4. Anota la ruta del repositorio que has creado (`organization-or-user-name/repository-name`); la necesitarás más adelante en el taller.

> [!NOTE]
> Al crear el repositorio desde la plantilla, se genera automáticamente un backlog de incidencias de GitHub. Trabajarás con estas incidencias durante todo el taller; no tienes que crear ninguna.

Usa una copia nueva de la plantilla del taller. Incluye instrucciones del repositorio, el código de la aplicación, pruebas, una habilidad `quality-checks` y el backlog que utilizarás. Si usas una copia antigua, consulta con la persona que imparte el taller para confirmar que contiene los archivos necesarios.

## Crear un Codespace

A continuación, utilizarás un Codespace para completar el taller.

[GitHub Codespaces][codespaces] es un entorno de desarrollo en la nube que permite escribir, ejecutar y depurar código directamente en el navegador. Proporciona un editor completo compatible con varios lenguajes de programación, extensiones y herramientas.

1. Ve al repositorio que acabas de crear.
2. Selecciona **Code**.
3. Selecciona la pestaña **Codespaces** y, después, **Create codespace on main**.
4. Espera a que termine la configuración del Codespace. La plantilla instala las dependencias del proyecto, Playwright Chromium y la base de datos local.
5. Si aparece el mensaje **Do you trust the authors of the files in this folder?**, selecciona **Trust Folder & Continue**.
6. Abre un terminal en la raíz del repositorio e inicia la aplicación:

   ```bash
   npm run dev
   ```

7. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser** y confirma que se carga el sitio de Tailspin Toys.
8. Vuelve al terminal y detén el servidor de desarrollo con <kbd>Ctrl</kbd>+<kbd>C</kbd>.

## Resumen y siguientes pasos

Ya tienes todo listo. En esta lección:

- has creado tu copia del proyecto Tailspin Toys a partir de la plantilla;
- has creado un Codespace y comprobado que el proyecto estaba preparado.

A continuación, [instalarás GitHub Copilot CLI][next-lesson] en tu Codespace y lo autenticarás con tu cuenta de GitHub.

## Recursos

- [Información general sobre GitHub Codespaces][codespaces]
- [Crear un repositorio desde una plantilla][template-repository]
- [Introducción a Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
