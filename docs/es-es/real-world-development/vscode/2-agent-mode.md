---
title: "Ejercicio 2 - Añadir funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. Como desarrollador, tu flujo de trabajo suele consistir en localizar los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta y volver a actualizarlo. Y así sucesivamente.

El modo de agente de Copilot está pensado para trabajar de forma más autónoma en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, aplica los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentre. Veamos cómo utilizarlo para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque sus nombres se parecen, el modo de agente y cloud agent están diseñados para experiencias distintas. El modo de agente trabaja en el IDE y ofrece ciclos de feedback rápidos e interacción. Cloud agent trabaja de forma asíncrona como un compañero del equipo y se comunica mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- pedir al modo de agente de Copilot que explore el proyecto e identifique los archivos pertinentes;
- usar el modo de agente de GitHub Copilot para implementar funcionalidades en la interfaz y la capa de datos;
- revisar los cambios y las pruebas generadas por Copilot antes de combinar el código.

## Escenario

A medida que crece la lista de juegos, quieres que los usuarios puedan filtrar por categoría y editorial. En el ejercicio anterior ya añadiste un helper de editoriales. Ahora completarás con Copilot Agent Mode los cambios restantes en la capa de datos, la interfaz y las pruebas.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para familiarizarte con sus funciones actuales.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Es una aplicación Astro única que genera las páginas como HTML estático durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde su frontmatter mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz del lado del cliente. Los helpers reutilizables de acceso a datos están en `src/lib/`, y la interactividad se implementa con pequeños elementos `<script>` de Astro que utilizan las API estándar del DOM.

### Iniciar el sitio web

La tarea `npm run dev` inicia el servidor de desarrollo Astro. Sigue estos pasos para ejecutarla en GitHub Codespace:

1. Vuelve a tu Codespace. Deberías estar en `main` después del ejercicio anterior.
2. Abre un terminal nuevo en Codespace con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo Astro esté listo, aparecerá un mensaje parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pide abrir `http://localhost:4321` en una ventana del navegador, ciérralo seleccionando **x**.

5. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona en el terminal la dirección del servidor Astro, `http://localhost:4321`.

> [!NOTE]
> En Codespaces, al seleccionar la URL local desde el terminal se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado hacia tu Codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Cuando el sitio esté en ejecución, explora sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con título, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, aparece una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando corresponda.

La implementación inicial del sitio funciona, pero queremos añadir nuevas capacidades. Empecemos por revisar el backlog. Al crear el proyecto desde la plantilla se generó un backlog de incidencias; pide a GitHub Copilot que te muestre sus elementos.

1. Selecciona **Agent** en el selector de agentes de la vista Chat. El agente puede planificar e implementar cambios en varios archivos, ejecutar comandos del terminal e invocar herramientas.

   ![Captura del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no expresan ninguna preferencia por un modelo. Durante su creación utilizamos Claude Sonnet 4.5 para que las sugerencias de código fueran relativamente coherentes y la experiencia resultara adecuada. Sin embargo, como los modelos de lenguaje son probabilísticos, las sugerencias pueden diferir de las que se describen aquí. Es normal y esperable.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje, Copilot puede usar un comando MCP distinto y, aun así, completar la tarea.

3. Pide a Copilot que te muestre el backlog con este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot ha priorizado los elementos según cuáles considera más útiles para los usuarios.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que le proporcionará contexto. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidad al sitio web.

1. En tu Codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa sus directrices generales para añadir funcionalidad:
   - información general sobre la arquitectura;
   - principios de diseño de componentes, capacidad de prueba y accesibilidad;
   - enlaces a instrucciones para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así se pueden reutilizar directrices, dividir conjuntos extensos en archivos más pequeños y proporcionar ejemplos.

## Implementar la funcionalidad de filtrado

Para completar el filtrado hacen falta al menos tres cambios independientes:

- añadir o perfeccionar la lógica de filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento del filtrado;
- actualizar la página de lista de juegos para incorporar la interfaz de filtrado.

Además, las pruebas deben ejecutarse correctamente antes de combinar los cambios. Copilot Agent Mode puede encargarse de estas tareas.

1. Continúa con la conversación actual de Copilot o inicia otra con **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de la selección del archivo de instrucciones UI](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el selector de agentes de la vista Chat.

   ![Captura del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que sigue seleccionado el modelo **Claude Sonnet 4.5**.
5. Pide a Copilot que implemente la funcionalidad conforme a la incidencia relacionada del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos pertinentes. Debería encontrar tanto los helpers de acceso a datos como la interfaz y las pruebas. Después, modificará los archivos y ejecutará las pruebas.

   ![Captura de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos según la complejidad y el tamaño del código base. Mientras tanto, pueden aparecer los botones **Keep** y **Undo** en el editor. Cuando termine, tendrás la opción de conservar o deshacer cada cambio; no hace falta que los selecciones mientras el agente sigue trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un cuadro de diálogo de Copilot Chat que pregunta si se pueden ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que el proceso se detenga en algunos momentos o que algunas pruebas fallen. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y no detectar errores.

   ![Captura de una sesión completa de Copilot Agent Mode](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examina el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se ejecuta el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a utilizar GitHub Copilot Agent Mode para añadir capacidades a Tailspin Toys:

- el modo de agente puede implementar funcionalidades en la interfaz y la capa de datos;
- puede explorar el proyecto, identificar archivos pertinentes y coordinar cambios;
- puedes revisar los cambios y las pruebas que genera antes de combinarlos.

Ahora [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar la paginación

A medida que crece la lista de juegos, será necesario habilitar la paginación. Usa lo aprendido en este ejercicio y pide a Copilot que la implemente en el sitio. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas, incluidos los archivos de instrucciones existentes;
- decide si quieres que el usuario elija el tamaño de página o si debe ser fijo;
- proporciona a Copilot toda la información necesaria para desarrollar la solución que buscas;
- puede que tengas que iterar y pedir cambios, proporcionando contexto. Es el flujo normal al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
