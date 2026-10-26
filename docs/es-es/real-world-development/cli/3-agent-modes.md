---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Acuerda un enfoque con Plan, desarrolla el filtrado desde una incidencia con Autopilot y revisa el resultado en el modo Interactive."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una pequeña funcionalidad al proyecto, pero los cambios más amplios requieren un proceso más sólido. GitHub Copilot CLI está diseñado para trabajar con el flujo existente de una organización y ayudar a desarrollar lo que hace falta de la forma adecuada. En esta primera de varias lecciones seguirás un proceso de desarrollo habitual con agentes: partirás de una incidencia, generarás una funcionalidad, comprobarás el código, validarás su comportamiento y, finalmente, la combinarás en el proyecto.

> [!NOTE]
> Seguirás utilizando la misma conversación y rama durante todo el flujo de trabajo de esta funcionalidad. Normalmente usarías ramas o PR distintas para los diferentes tipos de archivos, pero aquí simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot a partir de una incidencia de GitHub;
- definirás los requisitos en el modo Plan;
- implementarás la funcionalidad en el modo Autopilot;
- revisarás el código;
- validarás manualmente la funcionalidad en un navegador reenviado.

Mientras avanzas con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás la PR.

## Escenario

El catálogo de Tailspin Toys crece y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero hay que acordar detalles como la combinación de categorías antes de escribir código. Usarás el modo Plan para tomar esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

La incorporación de agentes de programación de IA al flujo de desarrollo no cambia los principios básicos; de hecho, los hace aún más importantes. La mayoría de los desarrolladores sigue un proceso parecido a este:

1. Partir de una incidencia que describa el trabajo necesario.
2. Crear un plan de implementación.
3. Desarrollar y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código cuando se haya revisado y se hayan superado las comprobaciones de integración continua.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría de los procesos son variantes de este.

Al seguir un proceso estándar, te aseguras de que el código generado por IA cumpla los requisitos y pase por la misma revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla el nivel de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar de modo:

- **Interactive**: colaboras con el agente. Propone cambios y espera tu indicación antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta que considera que la tarea está completa.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Partir de la incidencia

Antes de empezar el filtrado, vuelve a tu Codespace y confirma que el repositorio y el terminal están listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo.
2. Confirma que se ha combinado la PR de las valoraciones por estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de dirección para seleccionar la incidencia **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt empieza ahora con `#7` o un número similar. El carácter `#` permite añadir al contexto incidencias y solicitudes de incorporación de cambios (PR) de GitHub.

## Planificar el filtrado

La planificación permite decidir cómo implementar una funcionalidad o realizar una tarea antes de pedir a Copilot que se encargue de ella. Conviene dedicar tiempo a planificar cualquier trabajo complejo. Vamos a cambiar al modo Plan y pedir a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador de modo situado debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia que has añadido antes, escribe este prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot empezará a explorar el proyecto y a determinar el mejor enfoque.

3. Puede que Copilot te pregunte cómo debería funcionar el filtrado. Responde según tus preferencias; no hay respuestas incorrectas.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que revise cualquier parte que no coincida con tus decisiones.

## Aprobar Autopilot

Una vez escrito y revisado el plan, toca implementarlo. Deja que Copilot trabaje con Autopilot, que le permite iterar hasta que considere que el problema está resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente de tu versión instalada.
2. Confirma que el indicador de modo situado debajo del prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación inmediatamente, así que revisa primero el plan. Si Copilot indica que faltan dependencias o que hay un conflicto de puertos, resuelve el problema de configuración antes de dar por terminadas las comprobaciones.

## Revisar y verificar la implementación

El código generado debe revisarse antes de combinarlo, igual que cualquier otro código. Revisa los cambios y ejecuta el sitio para confirmar que todo funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para acceder al modo Interactive. Confirma que el indicador de modo ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y con las decisiones que tomaste al planificar.
4. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diff.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija cualquier error.

## Explorar la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros por categoría y editorial, así como las combinaciones acordadas en el plan.
4. Confirma que el reinicio de filtros y los resultados vacíos se comportan según la incidencia y tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo que inició.

## Resumen y siguientes pasos

Has usado distintos modos de conversación para desarrollar y revisar una funcionalidad. En esta lección:

- has iniciado una conversación de Copilot a partir de una incidencia de GitHub;
- has definido los requisitos en el modo Plan;
- has implementado la funcionalidad en el modo Autopilot;
- has revisado el código;
- has validado manualmente la funcionalidad en un navegador reenviado.

A continuación, profundizarás en la generación de código y en cómo garantizar que siga las prácticas documentadas al [usar instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
