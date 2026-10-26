---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: utiliza Plan para acordar un enfoque, Autopilot para desarrollar el filtrado a partir de una incidencia e Interactive para revisar y verificar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Empezamos por añadir una funcionalidad pequeña al proyecto. Sin embargo, los cambios de mayor envergadura requieren un proceso más sólido. Por suerte, la aplicación GitHub Copilot está diseñada para adaptarse al flujo de trabajo de cada organización y ayudarnos a crear lo que necesitamos de la forma adecuada. Esta es la primera de varias lecciones en las que seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como esperas y, por último, la combinarás correctamente con el proyecto.

> [!NOTE]
> Utilizarás la misma sesión durante todo el flujo de trabajo de la funcionalidad. Normalmente usarías sesiones o PR distintas para los diferentes tipos de archivos, pero en este taller haremos una excepción para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una sesión de agente nueva a partir de una incidencia de GitHub;
- definirás los requisitos con el modo **Plan**;
- implementarás la nueva funcionalidad con el modo **Autopilot**;
- revisarás el código;
- validarás manualmente la funcionalidad en un lienzo de navegador.

Durante el resto del trabajo en esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo, y las personas que lo visitan necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero hay que acordar ciertos detalles antes de escribir código, por ejemplo, cómo combinar varias categorías. Utilizarás el modo Plan para resolver esas decisiones y, a continuación, autorizarás una implementación acotada con Autopilot.

## Contexto

Incorporar agentes de programación con IA al flujo de desarrollo no cambia sus principios fundamentales. De hecho, ¡los hace aún más importantes! La mayoría de los equipos de desarrollo siguen un proceso parecido a este:

1. Abrir una incidencia con detalles del trabajo que hay que realizar.
2. Crear un plan de implementación.
3. Desarrollar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Abrir una solicitud de incorporación de cambios (PR).
7. Cuando el código se haya revisado y se hayan superado las comprobaciones de integración continua, combinar el código.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los flujos de trabajo siguen un proceso parecido al anterior.

Seguir este proceso estándar ayuda a garantizar que el código generado por IA cumpla los requisitos y pase por el mismo proceso de revisión que el código escrito manualmente.

## Modos de sesión

El **modo de sesión** controla el grado de autonomía del agente. Puedes seleccionarlo en el menú desplegable situado debajo del campo del prompt y cambiarlo en cualquier momento:

- **Interactive**: tú y el agente trabajáis en equipo. El agente propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: primero, el agente crea un plan. Lo revisas y apruebas antes de que empiece a ejecutarlo.
- **Autopilot**: el agente trabaja de forma totalmente autónoma: escribe código, ejecuta pruebas e itera sin esperar tus indicaciones.

Empieza en el modo Plan, revisa el plan y, después, utiliza Autopilot para implementarlo.

## Iniciar una sesión desde la incidencia

Antes de continuar, confirma que la PR de las valoraciones por estrellas se ha combinado y que tu rama local `main` está actualizada.

1. En la barra lateral, debajo de **Sessions**, selecciona la carpeta del repositorio Tailspin Toys y, después, **Issues** en la navegación superior del repositorio. Abre la incidencia **Allow users to filter games by category and publisher**. Es una incidencia incluida en la plantilla; no tienes que crear otra.
2. Selecciona **New session** y elige un **new working tree** basado en la rama `main` actualizada.

    ![Vista de la incidencia en la aplicación GitHub Copilot con una flecha que señala el botón New session](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia está asociada a la sesión y selecciona **Plan** en el selector de modo.

> [!NOTE]
> Si no se encuentra Node.js o npm, pide al agente que ejecute `node --version` y `npm --version`. Si los comandos funcionan en tu terminal, pero no en la sesión, comprueba la configuración de `PATH` o del gestor de versiones de la aplicación y ciérrala por completo antes de volver a abrirla. Reintenta la operación y las comprobaciones que hayan fallado; no reinstales npm solo porque el agente no pueda encontrarlo.

## Planificar la funcionalidad de filtrado

La planificación te permite revisar el enfoque antes de que Copilot escriba código. Como has iniciado la sesión desde la incidencia, Copilot ya tiene la solicitud de la funcionalidad como contexto. Envía este prompt:

```plaintext
Build this feature.
```

Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que contempla el filtrado por categoría y editorial, controles accesibles, cambios en el acceso a los datos y pruebas. Habla con Copilot sobre cualquier comportamiento que no esté claro, como la combinación de varias categorías o qué ocurre cuando no hay juegos que coincidan.

El plan debe incluir lint, pruebas unitarias, pruebas E2E y comprobación de tipos con las herramientas que ya utiliza el proyecto. Céntralo en implementar y probar el filtrado; crearás la PR después de completar el flujo de calidad. Pide los cambios necesarios al plan antes de aprobarlo y guarda a mano la URL de la incidencia y cualquier aclaración acordada para la validación posterior.

## Aprobar Autopilot explícitamente

Cuando estés conforme con el plan, selecciona **Approve and implement this plan**, o la opción equivalente de tu versión. Comprueba el modo activo en lugar de dar por sentado que la aprobación lo ha cambiado. Confirma que el indicador de modo muestra **Autopilot**.

Copilot empezará a implementar la funcionalidad. Verás cómo avanza por el proceso y sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación inmediatamente, así que revisa el plan antes de aprobarlo. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar por completadas las comprobaciones. Detén únicamente los servidores que hayas iniciado tú.

## Revisar y verificar la implementación

Una vez generado el código, hay que revisarlo antes de combinarlo, igual que cualquier otro cambio. Revisaremos el código y ejecutaremos el sitio para comprobar que todo funciona correctamente.

1. Abre **Changes** y revisa la implementación del filtrado y sus pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de categorías y editoriales. Comprueba que los cambios siguen las instrucciones existentes del repositorio.
3. Revisa los resultados de lint, las pruebas unitarias, las pruebas E2E y la comprobación de tipos. Una comprobación omitida no cuenta como superada.
4. Resuelve los errores y vuelve a ejecutar las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright compila y sirve una vista previa, y puede reutilizar un servidor local; comprueba que el servidor utilizado para las pruebas pertenece a este árbol de trabajo y no a una lección anterior.

## Explorar la nueva funcionalidad

El código parece correcto, pero ¿funciona? Iniciaremos la aplicación como antes y abriremos el sitio en un lienzo de navegador.

1. Utiliza este prompt para pedir a Copilot que inicie la aplicación y abra la página en el lienzo de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. En unos instantes, se iniciará la aplicación y se abrirá una ventana del navegador dentro de la aplicación Copilot.
3. Selecciona una categoría y comprueba que los resultados coinciden. Borra el filtro, selecciona una editorial y comprueba que los resultados coinciden con ella.
4. Combina los filtros de categoría y editorial, y compara los resultados con la incidencia y el plan aprobado. Si se pueden seleccionar varias categorías, comprueba que su comportamiento coincide con lo acordado.
5. Prueba una combinación sin juegos coincidentes y comprueba el estado vacío. Borra o restablece los filtros y confirma que vuelve a aparecer la lista completa.
6. Cuando termines, utiliza este prompt para pedir a Copilot que detenga el servidor de desarrollo que inició para esta sesión y cierre el lienzo de navegador:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Conservar esta sesión para las próximas lecciones

Todavía no crees ni combines la PR de filtrado. Conserva esta sesión, el árbol de trabajo y la rama para las lecciones 4 a 8. A continuación, aplicarás instrucciones personalizadas al código de filtrado y después añadirás mejoras de habilidades y de QA antes de crear y combinar la PR conjunta en la lección 8. Puedes dejar la sesión inactiva después de detener su servidor de desarrollo y volver a ella en la siguiente lección.

## Resumen y siguientes pasos

Has utilizado distintos modos de agente para desarrollar y revisar una funcionalidad. En esta lección:

- iniciaste una sesión de agente nueva a partir de una incidencia de GitHub;
- definiste los requisitos con el modo **Plan**;
- implementaste la nueva funcionalidad con el modo **Autopilot**;
- revisaste el código;
- validaste manualmente la funcionalidad en un lienzo de navegador.

A continuación, profundizarás en cómo se genera el código y cómo conseguir que siga las prácticas documentadas mediante el uso de [instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
