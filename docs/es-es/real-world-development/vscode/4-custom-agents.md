---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados y adaptados a tareas o ámbitos concretos del flujo de desarrollo. Al definirlos mediante archivos Markdown en la carpeta `.github/agents` del repositorio, puedes proporcionar a Copilot instrucciones, buenas prácticas, patrones de programación y conocimientos específicos que le ayuden a realizar ciertos tipos de trabajo con mayor eficacia. Los equipos pueden codificar su experiencia en agentes reutilizables: un agente de accesibilidad que aplique [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo contiene frontmatter YAML con al menos `name` y `description`, seguido de un prompt Markdown que define el comportamiento, la especialización y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Hay cierta superposición entre los agentes personalizados y las [habilidades de agente][agent-skills-concept]. Ambos se definen principalmente en archivos Markdown y explican a una IA cómo realizar determinadas operaciones. La forma más sencilla de distinguirlos es esta: un **agente personalizado** es quien realiza el trabajo y las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden orquestar habilidades e incluso otros agentes. En este taller, el agente personalizado de accesibilidad revisará y actualizará el sitio según las directrices de accesibilidad; durante el trabajo podría utilizar habilidades, como una que gestione el flujo de una solicitud de incorporación de cambios o ejecute pruebas.

> [!NOTE]
> No existe una única forma «correcta» de crear un agente personalizado. Como ocurre con cualquier herramienta de IA, prueba e itera para descubrir qué funciona mejor en tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente;
- utilizarás el agente de accesibilidad en Copilot Chat para implementar un modo de alto contraste.

## Escenario

Tailspin Toys se compromete a que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de su capacidad visual o sus preferencias. Comentarios recientes indican que a algunas personas les cuesta leer el tema oscuro actual debido al escaso contraste entre el texto y el fondo. Para resolver este problema de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es fundamental, quieres implementar esta función cuanto antes. Utilizarás un agente personalizado para generar la funcionalidad.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

1. Vuelve a tu Codespace, abre un terminal y crea una rama nueva desde `main` para el trabajo de accesibilidad. Así mantendrás separado el trabajo de filtrado del ejercicio 3:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. Abre `.github/agents/accessibility.md`.
3. Observa el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

4. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible;
   - las buenas prácticas de accesibilidad;
   - ejemplos de código HTML, CSS y JavaScript;
   - una lista de errores habituales.

## Usar el agente personalizado en Copilot Chat

VS Code muestra en el selector de agentes de Copilot Chat todos los agentes personalizados definidos en `.github/agents`. Puedes seleccionar uno para que sus instrucciones y herramientas se apliquen a esa sesión.

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando corresponda.

1. Selecciona **Agent** en el selector de agentes de la vista Chat, si aún no está seleccionado.

   ![Captura del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona el selector de agentes de la parte inferior de la vista Chat; muestra el agente activo, que de forma predeterminada es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes disponibles.
4. Envía este prompt al agente de accesibilidad:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat explorará el código base, propondrá cambios y los aplicará a los archivos del proyecto. Cada edición aparecerá en el chat junto con la ruta del archivo y un diff que podrás revisar.
6. El indicador **Files changed** se actualizará a medida que el agente modifique los archivos.

> [!NOTE]
> El proceso puede tardar unos minutos. Copilot modificará archivos reales del repositorio, como componentes Astro, CSS y pruebas relacionadas.

Revisarás y orientarás este trabajo en curso en el siguiente ejercicio.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados en tareas y ámbitos concretos. Permiten codificar los conocimientos y estándares del equipo en agentes reutilizables que orientan el trabajo de Copilot.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados;
- cómo usar un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y guiarás el trabajo del agente][next-lesson], revisarás los cambios mientras se realizan y añadirás un modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Ejercicio anterior: Probar la funcionalidad con Playwright MCP][previous-lesson] | [Siguiente ejercicio: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
