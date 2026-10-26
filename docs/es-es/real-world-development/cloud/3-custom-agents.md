---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados en tareas o ámbitos concretos del flujo de desarrollo. Al definirlos en archivos Markdown dentro de `.github/agents`, puedes proporcionar a Copilot instrucciones, buenas prácticas, patrones de programación y conocimientos específicos. Los equipos pueden codificar su experiencia en agentes reutilizables: por ejemplo, un agente de accesibilidad que aplique [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown de `.github/agents` del proyecto o, de forma global, en `~/.copilot/agents`. Cada archivo incluye frontmatter YAML con al menos `name` y `description`, seguido de un prompt Markdown que define el comportamiento, la especialización y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Hay cierta superposición entre los agentes personalizados y las [habilidades de agente][agent-skills-concept]. Ambos se definen principalmente en archivos Markdown y explican a una IA cómo realizar operaciones. La forma más sencilla de distinguirlos es esta: un **agente personalizado** es quien realiza el trabajo y las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden orquestar habilidades e incluso otros agentes. En este taller, el agente de accesibilidad revisará y actualizará el sitio según las directrices de accesibilidad; podría usar habilidades como una que gestione el flujo de trabajo de una solicitud de incorporación de cambios o una que ejecute y gestione pruebas.

> [!NOTE]
> No existe una única forma «correcta» de crear un agente personalizado. Como ocurre con cualquier herramienta de IA, prueba e itera para descubrir qué funciona mejor en tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Explorarás los siguientes aspectos de los agentes personalizados:

- cómo se definen;
- cómo se asigna una tarea a uno.

## Escenario

Tailspin Toys se compromete a que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de su capacidad visual o sus preferencias. Comentarios recientes indican que a algunos usuarios les cuesta leer el tema oscuro actual debido al escaso contraste entre el texto y el fondo. Para resolverlo, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es fundamental, quieres implementar esta función cuanto antes. Utilizarás un agente personalizado para generar la funcionalidad.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa su contenido para entender cómo orientará a Copilot.

1. Vuelve a tu Codespace y abre `.github/agents/accessibility.md`.
2. Observa el frontmatter YAML con los campos `name` y `description`.

> [!CAUTION]
> El frontmatter con `name` y `description` es obligatorio para los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible;
   - las buenas prácticas de accesibilidad;
   - ejemplos de código HTML, CSS y JavaScript;
   - una lista de errores habituales.

## Crear y asignar una incidencia

El centro de operaciones es el lugar desde el que se trabaja con los agentes del entorno. Allí puedes asignar tareas al agente de Copilot en la nube, supervisarlas y dar instrucciones adicionales. Empecemos por asignarle una tarea para crear un modo de alto contraste.

1. Ve al repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo.
4. Selecciona **Blank issue**.
5. En **Title**, escribe `Add high contrast mode to website`.
6. En **Description**, escribe:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecciona **Create**.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

    ![Captura de la asignación del agente en la nube, con el agente personalizado y Accessibility resaltados](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en segundo plano.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot: asistentes de IA especializados en tareas y ámbitos concretos. Permiten codificar la experiencia y los estándares del equipo en agentes reutilizables que orientan el trabajo de Copilot.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados;
- cómo se asigna una tarea a uno.

Mientras Copilot implementa el modo de alto contraste, en el siguiente ejercicio [supervisarás y guiarás la sesión del agente][next-lesson] desde el centro de operaciones.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Prepararse para usar agentes personalizados en una organización][org-custom-agents]
- [Prepararse para usar agentes personalizados en una empresa][enterprise-custom-agents]

---

| [← Ejercicio anterior: Agente de GitHub Copilot en la nube][previous-lesson] | [Siguiente ejercicio: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
