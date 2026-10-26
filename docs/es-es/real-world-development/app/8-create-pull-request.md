---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de las instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Es hora de revisarlos conjuntamente y abrir una solicitud de incorporación de cambios. Tú mismo combinaste la solicitud de incorporación de cambios (PR) de las valoraciones por estrellas; esta vez permitirás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de las instrucciones, la actualización de la habilidad y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de trabajo de filtrado y calidad en una misma sesión y rama, y todo ese trabajo formará parte de esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una PR;
- revisarás la PR completa de la funcionalidad y las evidencias de verificación;
- autorizarás Agent Merge solo después de revisar el trabajo y confirmarás que la PR se ha combinado.

## Escenario

Durante el flujo de trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR, sin dejar de reservar al desarrollador la autorización para combinarla.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo pendiente para integrar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Al habilitarlo, la sesión de la aplicación lee la PR y resuelve los problemas que impiden combinarla: corrige las comprobaciones fallidas de integración continua (CI), responde a los comentarios de revisión y actualiza la rama cuando sea necesario. Después, la combina en cuanto GitHub lo permite. Se ejecuta en segundo plano, sigue activo aunque se reinicie la aplicación y se desactiva al combinarse la PR.

Hasta ahora, tú mismo seleccionabas **Merge pull request**. Agent Merge puede hacerse cargo de esta responsabilidad, pero para editar código y combinar la PR sigue necesitando tu autorización explícita. Revisa las acciones permitidas y el trabajo antes de concederle permiso para combinarla.

## Utilizar Agent Merge para gestionar la PR

Ya has creado y revisado el código. Ahora vamos a permitir que Agent Merge gestione el proceso de la PR.

1. Usa el selector de agente para elegir **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no basta para iniciarlo.

Se iniciará el proceso de Agent Merge. El agente:

- creará la solicitud de incorporación de cambios con un título y una descripción;
- si has iniciado la sesión desde una incidencia, la incluirá en el cuerpo de la descripción como referencia;
- actualizará la rama o resolverá posibles conflictos de combinación con la rama de destino;
- supervisará el proceso de CI para comprobar que se superan todas las verificaciones;
- supervisará la PR para detectar comentarios de otros desarrolladores o de la revisión de código de Copilot y aplicará los cambios necesarios para responder a ellos;
- podrá combinar la PR automáticamente cuando se hayan superado todas las comprobaciones.

Dejaremos que Agent Merge combine también la PR cuando se hayan superado todas las comprobaciones.

5. Antes de conceder permiso para combinarla, revisa las diferencias de la PR y las evidencias de verificación.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que la opción **Merge pull request** está marcada.
8. Pide explícitamente al agente que combine la PR:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Sigue el proceso y resuelve los bloqueos que comunique el agente. Antes de continuar, confirma que el estado de la PR es **Merged**; tener habilitado el permiso para combinarla o un agente en ejecución no confirma que ya se haya combinado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos necesarios. Resuelve esos bloqueos antes de continuar.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación, y ahora también el proceso de la solicitud de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una PR;
- has revisado la PR completa de la funcionalidad y las evidencias de verificación;
- has autorizado Agent Merge solo después de revisar el trabajo y has confirmado que la PR se ha combinado.

A continuación, [utilizarás un lienzo existente y crearás un lienzo de triaje][next-lesson] para explorar una forma más completa de inspeccionar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
