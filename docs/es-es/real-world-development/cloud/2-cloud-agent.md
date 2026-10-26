---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan problemas de deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidades que llevan tiempo en el backlog por falta de tiempo para implementarlas. El agente de GitHub Copilot en la nube puede actualizar código y añadir funcionalidad de forma autónoma. Cuando termina, abre un borrador de solicitud de incorporación de cambios (PR) para que lo revise un desarrollador. Así se pueden delegar tareas tediosas, acelerar el desarrollo y permitir que el equipo se centre en asuntos de mayor alcance.

En este ejercicio explorarás cómo:

- personalizar el entorno de generación de código;
- garantizar que las operaciones se realicen de forma segura;
- redactar incidencias con un alcance claro;
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere resolver. Los contratistas que crearon la primera versión del sitio dejaron la documentación en mal estado: en realidad, no hay documentación. Como primer paso, el equipo quiere añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar cómo se descubren los juegos. Quiere mostrar en la página de detalles de cada juego otros juegos de su categoría, para que los usuarios puedan seguir explorando. No necesita una implementación pulida todavía; quiere algo que permita probar la experiencia de usuario. La tarea está bloqueada, pero hay otras incidencias de mayor prioridad.

Son ejemplos de tareas que pueden perder prioridad con facilidad y que se pueden asignar al agente de GitHub Copilot en la nube. El agente puede trabajar en ellas de forma asíncrona mientras tú haces otras tareas; después podrás volver y revisar los cambios.

## Presentación del agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Puedes trabajar con él de varias formas, por ejemplo, [asignándole una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se lo asignas, Copilot crea una solicitud de incorporación de cambios en borrador para hacer seguimiento del progreso, configura un entorno y empieza a trabajar. Puedes examinar la sesión mientras trabaja o cuando termine. Cuando tenga lista la solución propuesta para que la revises, te mencionará en la PR.

## La importancia de definir bien las instrucciones

GitHub Copilot no hace magia: no puedes dejar de pensar. Hasta las tareas que parecen sencillas tienen cierta complejidad cuando se examinan en detalle. Por tanto, [define cuidadosamente el alcance de las tareas del agente de Copilot en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al incorporar IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has aprendido cómo guían a Copilot las instrucciones personalizadas. Si ya has realizado un ejercicio práctico sobre ellas, habrás visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para revisarlo rápidamente. Antes de asignar trabajo al agente en la nube, examina en modo de solo lectura los archivos de instrucciones del repositorio para poder observar su efecto más adelante.

Abre los siguientes archivos en la interfaz web de GitHub o en un Codespace que ya esté en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observa el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes la incidencia **Code lacks documentation** al agente en la nube, observa si la PR resultante incluye comentarios TSDoc, sigue las convenciones de TypeScript y añade encabezados de comentario. Estas características proceden de los archivos de instrucciones; volverás a verlas al revisar las PR en un ejercicio posterior.

## Configurar el entorno de desarrollo del agente en la nube

La creación de código suele requerir un entorno específico y scripts de configuración para instalar dependencias y preparar el proyecto. Lo mismo ocurre cuando se asigna trabajo a Copilot, que opera de forma parecida a un ingeniero de software.

El agente en la nube utiliza [GitHub Actions][github-actions] como entorno de ejecución. Puedes personalizarlo mediante un [flujo de trabajo de configuración][setup-workflow] especial, definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece. Así se instalan las herramientas de desarrollo y las dependencias necesarias. El taller ya incluye este flujo para simplificar los ejercicios y permitirte aprender. Proporciona a Copilot Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y rellenada para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Asigna a estos pasos los permisos mínimos necesarios. Copilot tendrá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio durante la configuración, por ejemplo para instalar dependencias, necesitas el permiso `contents: read`. Si no lo clonas en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la interfaz y la aplicación: Node.js (ahora toda la aplicación usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y rellena la base de datos SQLite local para que las compilaciones y pruebas tengan datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo tiene el aspecto de cualquier flujo de trabajo de GitHub, pero incluye algunos detalles importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. GitHub Actions lo ejecuta antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- Observa el activador `workflow_dispatch`, que permite ejecutar manualmente el flujo desde la pestaña **Actions** del repositorio. Así puedes comprobar que se ejecuta correctamente sin tener que esperar a que Copilot lo use.

## Añadir documentación

Todo el mundo sabe que la documentación es importante, pero muchos proyectos tienen información desactualizada o carecen de ella. Esta deuda técnica ralentiza el trabajo y dificulta mantener el código base o incorporar desarrolladores nuevos. Copilot destaca en la generación de documentación, por lo que esta es una buena incidencia para asignar al agente en la nube. Trabajará en segundo plano y podrás revisar el resultado en un ejercicio posterior.

1. Abre el repositorio en github.com en una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue**.
5. En **Title**, escribe `Code lacks documentation`.
6. En **Description**, escribe:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create**.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

    ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Abre la pestaña **Pull Requests**.
11. Abre la nueva PR, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si no aparece, espera uno o dos minutos y actualiza la ventana.
12. Al cabo de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> Puede que la lista tarde varios minutos en aparecer en la PR. Copilot está creando el entorno, ejecutando el flujo de trabajo anterior, analizando el proyecto y decidiendo cómo abordar el problema.

13. Revisa la lista y las tareas que va a realizar.
14. Desplázate por la cronología de la PR. Debería aparecer una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

    ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa la sesión en directo y cómo Copilot resuelve el problema. Verás que explora el código y analiza su estado, se detiene para pensar y decidir un plan, y crea código.

El proceso puede tardar varios minutos. Una de las ventajas principales del agente en la nube es que permite trabajar de forma asíncrona y dejar que tú te ocupes de otras tareas. Aprovecharemos esta capacidad para asignar otra tarea al agente y, mientras trabaja, añadir una funcionalidad a la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

Una ventaja del agente en la nube es poder repartir el trabajo: tú puedes centrarte en unas tareas mientras el agente se ocupa de otras. Aunque añadir una sección de juegos relacionados para el equipo de diseño no debería llevar mucho tiempo, ese tiempo se puede dedicar a otras tareas. Asignemos el trabajo al agente.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo.
4. Selecciona **Blank issue**.
5. En **Title**, escribe `Show related games on the game details page`.
6. En **Description**, escribe:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create**.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

    ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Al poco tiempo aparecerá un grupo de 👀 en el primer comentario de la incidencia, lo que indica que Copilot está trabajando.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda petición. El agente en la nube funciona de forma parecida a un ingeniero de software: no tienes que supervisarlo continuamente, pero sí revisar el resultado cuando termine. Ahora veremos cómo crear y utilizar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona. Copilot puede resolver deuda técnica, crear funcionalidades o ayudar a migrar código de un framework a otro.

Has explorado estos conceptos:

- personalizar el entorno para generar código;
- garantizar que las operaciones se realicen de forma segura;
- redactar incidencias con un alcance claro;
- asignar incidencias a Copilot.

Mientras el agente trabaja en segundo plano, crearás y utilizarás agentes personalizados. El agente en la nube también puede usar [servidores MCP][cloud-agent-mcp] y dispone de las instrucciones personalizadas que exploraste en módulos anteriores.

## Recursos

- [Acerca del agente de Copilot en la nube][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración del agente][cloud-agent-best-practices]

| [Siguiente ejercicio: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
