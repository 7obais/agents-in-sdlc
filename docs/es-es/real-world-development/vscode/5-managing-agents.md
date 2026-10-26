---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Agentes personalizados][previous-lesson]

Al usar el modo de agente de GitHub Copilot, este trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como ese trabajo ocurre en tu equipo y en tiempo real, Copilot Chat en VS Code muestra cada llamada a herramientas y edición de archivos. Puedes revisar los diff en línea, aceptar o rechazar cambios concretos y orientar la conversación con prompts de seguimiento sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás las ediciones propuestas;
- guiarás al agente durante la sesión para añadir un selector de modo claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un modo claro para quienes consideran demasiado oscuro el tema predeterminado. En lugar de iniciar otra conversación, ampliarás el trabajo de la sesión existente.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y los cambios que propone. Las ediciones aparecen en el chat con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver las llamadas a herramientas, las lecturas y ediciones de archivos y los comandos del terminal que solicita ejecutar.
3. Cuando proponga una edición, aparecerá un diff en línea:
   - selecciona **Keep** para aceptar el cambio;
   - selecciona **Undo** para revertirlo y dejar que el agente lo intente de nuevo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a herramientas solicita permiso (por ejemplo, para ejecutar un comando del terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que te resulten aceptables.

5. Espera a que el agente termine de implementar el modo de alto contraste. Debería aparecer un mensaje final que resuma los cambios.

## Guiar al agente para añadir el modo claro

Ahora que existe el modo de alto contraste, ampliarás la misma conversación para añadir un selector de modo claro. Al continuar en la misma sesión, el agente conserva el contexto de su trabajo anterior, incluidos los componentes que modificó, el enfoque de estilos y el patrón de almacenamiento local.

1. En el cuadro de Copilot Chat, envía este prompt de seguimiento:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. El agente retomará el trabajo, revisará los cambios que ya hizo y propondrá los cambios para el modo claro.
3. Revisa cada edición nueva, igual que hiciste con el modo de alto contraste. Usa **Keep** o **Undo** en cada diff.
4. Si el agente sigue un enfoque que no quieres, por ejemplo, si intenta reorganizar la interfaz de los controles, puedes detenerlo con el botón **Stop** junto al cuadro de chat y enviarle una aclaración.

> [!TIP]
> Guiar una sesión en curso es más rápido que volver a empezar. Usa prompts de seguimiento para pedir ajustes («simplifica el CSS; usa variables CSS en lugar de duplicar selectores»), corregir el enfoque («no añadas un tercer selector; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también atajos de teclado para ambos modos»).

## Revisar los cambios

Antes de confirmar el trabajo, revisa rápidamente todo lo que haya modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y examina los diff. Confirma que se aplican los patrones de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia con almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la lección siguiente][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo de agente de Copilot Chat para supervisar y guiar una sesión en curso: has revisado las ediciones a medida que aparecían, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder contexto.

En esta lección:

- has supervisado el trabajo de un agente en tiempo real;
- has aceptado y rechazado ediciones individuales;
- has guiado una sesión en curso mediante prompts de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios generados por el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Ejercicio anterior: Agentes personalizados][previous-lesson] | [Siguiente ejercicio: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
