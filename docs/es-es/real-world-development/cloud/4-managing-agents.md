---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los ejercicios anteriores has pedido al agente de GitHub Copilot en la nube que trabaje en tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente está diseñado para trabajar de forma autónoma y asíncrona, también es importante supervisar esas tareas.

Hay varias herramientas para gestionar el trabajo asignado al agente, entre ellas la [página de agentes][agents-page] de GitHub.com. Desde este centro de operaciones puedes consultar todas las tareas de agente que tienen solicitudes de incorporación de cambios abiertas, explorar las operaciones realizadas e incluso orientar una sesión que siga en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube;
- guiarás una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo decidió añadir también un modo claro. Como ya se está actualizando el estilo del sitio y añadiendo controles de selección, parece lógico incluirlo en el trabajo. Guiarás al agente para que añada el modo claro además del modo de alto contraste.

## Revisar las tareas del agente de Copilot en la nube

Consulta el estado actual de las tareas asignadas al agente.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código base;
   - generar API para modificar juegos;
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa las operaciones realizadas por Copilot. Pueden incluir:
   - obtener el código del repositorio;
   - preparar el entorno de Copilot;
   - configurar servidores MCP;
   - realizar varios pasos para completar la tarea.

> [!NOTE]
> Los pasos concretos que aparezcan dependerán del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios que aparece a la derecha. Allí puedes consultar la PR y los archivos modificados.

## Guiar al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada un control de modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre elegido por Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en el centro de operaciones](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Verás mensajes parecidos a los siguientes.
3. En el cuadro **Steer active session while Copilot is working**, añade este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro de orientación de la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa que Copilot reconoce la solicitud y la incorpora a su trabajo.

## Dejar que Copilot trabaje

Copilot continuará con la tarea actualizada. Incorporará la nueva petición después de terminar el paso que estaba realizando cuando enviaste el mensaje.

Como antes, tardará unos minutos. Puedes aprovechar ese tiempo para repasar lo que has aprendido.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro desde el que se supervisan y guían las tareas del agente en la nube. Puedes hacer seguimiento de las tareas activas y terminadas, revisar el trabajo y redirigir una sesión en curso para cambiar el alcance o añadir indicaciones.

Has explorado estos conceptos:

- usar el centro de operaciones y la página de agentes para supervisar tareas;
- redirigir una sesión en curso para pedir funcionalidad adicional.

Mientras Copilot termina el trabajo de accesibilidad, a continuación [iterarás sobre las solicitudes de incorporación de cambios que haya creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Siguiente ejercicio: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
