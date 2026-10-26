---
title: "Lección 10 - Resumen y siguientes pasos"
description: "Repasa el flujo de trabajo con Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado GitHub Copilot CLI en un flujo de trabajo continuo con Tailspin Toys. En este taller:

- has preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado del backlog;
- has añadido valoraciones por estrellas, revisado el resultado en un navegador reenviado y combinado manualmente tu primera solicitud de incorporación de cambios (PR);
- has partido de la incidencia de filtrado, definido el enfoque en el modo Plan, desarrollado la funcionalidad en Autopilot y revisado el resultado en Interactive;
- has guiado al agente con instrucciones personalizadas, personalizado la habilidad existente `quality-checks` y la has usado para ejecutar pruebas unitarias, lint y comprobaciones de tipos;
- has añadido el servidor Model Context Protocol (MCP) de Playwright y lo has utilizado para explorar el filtrado en un navegador real;
- has creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas obtenidas en el navegador;
- has revisado el cambio completo del filtrado y autorizado Agent Merge para la PR;
- has explorado comandos de barra para el contexto, los modelos, el uso compartido y la delegación opcional a la nube.

## Qué has entregado

El taller incluye dos hitos de PR, cada uno en su propia rama creada desde `main` actualizado:

1. **Valoraciones por estrellas**: mostrar `starRating` y un estado explícito para los juegos sin valoración en sus tarjetas.
2. **Filtrado y flujo de calidad**: implementar el filtrado, actualizar las instrucciones y aplicarlas a la funcionalidad, personalizar el informe de `quality-checks`, crear un perfil de QA e incluir las pruebas correspondientes.

Desde la planificación del filtrado hasta la apertura de su PR, has usado la misma conversación y rama. Hemos agrupado el trabajo en una sola PR para simplificar el taller.

## Distintos tipos de verificación

Has comprobado el código de varias formas: mediante pruebas automatizadas, tu propia inspección en el navegador y la exploración del navegador de Copilot con MCP. La habilidad `quality-checks` ha ejecutado pruebas unitarias, lint y comprobaciones de tipos y ha informado de los resultados con el formato nuevo. QA ha reunido estos resultados con una revisión de los requisitos y la cobertura de pruebas antes de abrir la PR.

Las pruebas nuevas deben cubrir carencias reales; una ejecución de QA que no necesite ninguna prueba adicional también puede ser correcta. Revisa el código y las pruebas antes de autorizar la combinación, y actualiza las pruebas afectadas después de realizar cambios.

## Buenas prácticas

El contexto y las herramientas que proporcionas a Copilot condicionan su trabajo. En este taller has actualizado instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones entre conversaciones y adáptalas a las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados reales de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** según la tarea. Usa **Plan** para pensar el enfoque antes de desarrollar, **Interactive** para participar en cambios acotados y **Autopilot** para tareas bien delimitadas. Puedes elegir un modelo más rápido para cambios rutinarios y otro más potente para trabajos complejos.

El contexto sigue siendo tan importante como la infraestructura. Explicar con claridad *qué* quieres crear, *por qué* y *cómo* cambia significativamente el resultado.

## Más cosas que puedes explorar

Ya has cubierto el flujo de trabajo principal. También puedes probar estas funciones de la CLI:

- `/review` para pedir al agente de revisión de código que analice los cambios;
- `/rubber-duck` para hablar sobre un problema y obtener otra perspectiva;
- `/fleet` para coordinar tareas independientes en paralelo;
- `/worktree` para aislar una tarea en otro árbol de trabajo;
- `/delegate` para enviar una tarea a Copilot cloud agent.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola. Úsala con código de producción, proyectos personales o esa pequeña aplicación que llevas años queriendo crear. Comparte lo que aprendas con el equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Backer Concierge con Foundry][foundry]. Para comparar otros entornos, explora los talleres de [VS Code][vscode], la [aplicación GitHub Copilot][app] o [Copilot cloud agent][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
