---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Ejercicio anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido que añada documentación al código, implemente la funcionalidad de juegos relacionados para que el equipo de diseño pueda evaluarla y añada funciones de accesibilidad, incluidos controles de alto contraste y modo claro. Vamos a revisar los cambios de código y, si hace falta, enviar comentarios a Copilot para que los mejore.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian al incorporar IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Revisaremos las propuestas de GitHub Copilot para la documentación, los juegos relacionados y las funciones de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente trabaja de forma asíncrona y sin supervisión directa, se aplican restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utilizará para sus cambios.
- El agente trabaja en GitHub Actions dentro de un entorno efímero independiente.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que se ejecuten.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisar la documentación generada

Empecemos por la primera PR que creó el agente en la nube: la que añade documentación al código. Revisarás los cambios mediante la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Puede que la PR muestre un aviso de que el cortafuegos bloquea a GitHub Copilot. Es normal, ya que Copilot tiene acceso limitado a recursos externos, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el cortafuegos del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para ver la lista.
3. Abre la PR titulada **Add missing documentation** o con un título parecido.

> [!NOTE]
> Si Copilot sigue trabajando, la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, incluidos los comentarios de documentación TSDoc y otros comentarios. Los cambios exactos pueden variar. Comprueba los comentarios TSDoc, las convenciones de TypeScript y los encabezados de archivo incluidos en los archivos de instrucciones del ejercicio 2.
6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Debería aparecer un indicador que señala que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían superarse en la aplicación Astro. El proceso puede tardar unos minutos.

## Solicitar cambios a GitHub Copilot

La colaboración con Copilot en una solicitud de incorporación de cambios no es unidireccional. Puedes mencionarlo en comentarios generales o comentarios en línea sobre el código; Copilot los leerá e iniciará otra sesión para atenderlos. Como los resultados no son deterministas, no hay un texto único que debas usar. Por ejemplo, puedes pedirle:

- añadir encabezados de comentario al principio de cada archivo con una breve descripción de su función;
- añadir comentarios TSDoc a los archivos TypeScript y Astro;
- crear un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot** como harías con cualquier otra persona. Usa una de las sugerencias anteriores o pide otra mejora de documentación.
2. Selecciona **View Session** para observar cómo trabaja Copilot. Verás que inicia una sesión nueva para realizar los cambios.
3. Puedes seleccionar **Back to pull request** para volver a la PR.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, aparecerá una confirmación nueva en la PR.
5. Selecciona **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y combinarla.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la funcionalidad de juegos relacionados

Vuelve a la PR que Copilot generó para resolver la incidencia de juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la PR titulada **Show related games on the game details page** o con un título parecido.
4. Selecciona **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Debería aparecer un indicador de flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo aparecerán en las comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían superarse en la aplicación Astro. El proceso puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en Codespace para probar manualmente la funcionalidad de juegos relacionados. Abre un terminal y ejecuta estos comandos; sustituye `<branch-name>` por la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ya ha creado la funcionalidad de juegos relacionados. Como antes, puedes iterar y pedir cambios. Por ejemplo, puedes solicitar un número distinto de juegos relacionados o pedir que se añadan encabezados de comentario y comentarios TSDoc. Recuerda que esta incidencia se asignó **antes** de actualizar las instrucciones personalizadas. Para pedir cambios, añade un comentario en la pestaña **Conversation**; Copilot iniciará una sesión nueva.

## Revisar las funciones de accesibilidad

Por último, revisaremos las funciones de accesibilidad implementadas por el agente personalizado. La PR debería incluir tanto el modo de alto contraste que asignaste en el ejercicio 3 como el modo claro solicitado desde el centro de operaciones en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la PR titulada **Add high contrast mode to website** o con un título parecido.

> [!NOTE]
> Si Copilot sigue trabajando, la PR incluirá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** para revisar el código generado.
5. Revisa la implementación, especialmente:
   - los componentes de interfaz para alternar entre modos;
   - el uso del almacenamiento local para guardar las preferencias;
   - los cambios de CSS o estilos para los modos de alto contraste y claro;
   - los atributos de accesibilidad (etiquetas ARIA, navegación con teclado, etc.);
   - el código JavaScript/TypeScript que gestiona los modos.

6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Debería aparecer un indicador de flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo aparecerán en las comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían superarse en la aplicación Astro.
10. **Opcional:** puedes cambiar a la rama en Codespace para probar manualmente las funciones de accesibilidad. Abre un terminal y ejecuta estos comandos; sustituye `<branch-name>` por el nombre de la rama de Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los controles de alto contraste y modo claro en el navegador. Confirma que funcionan y que las preferencias persisten al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha guiado a Copilot para implementar estas funciones siguiendo buenas prácticas de accesibilidad. Si detectas algún problema o mejora posible, puedes mencionar a **@copilot** en un comentario y pedirle que actualice la PR.

## Ejercicio opcional: seguir delegando

El agente en la nube resulta útil para asignarle tareas reales del backlog y ocuparte mientras tanto de otros asuntos. Para practicar, crea algunas incidencias en tu repositorio y asígnaselas a Copilot. Algunas ideas:

- crear un formulario para que los usuarios indiquen su interés en un juego;
- añadir paginación a la lista de juegos;
- añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizados del repositorio** y has visto más adelante su efecto en los resultados del agente.
- **Has asignado incidencias al agente de Copilot en la nube** y has observado cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** para mejorar la accesibilidad con modos de alto contraste y claro.
- **Has utilizado la página de agentes de Copilot como centro de operaciones** para supervisar y orientar la sesión de accesibilidad mientras estaba en curso.
- **Has revisado las solicitudes de incorporación de cambios del agente y has iterado sobre ellas**, mencionando a `@copilot` para pedir cambios y aprobando flujos de trabajo.

## Revisión y siguientes pasos

Has completado el recorrido obligatorio del agente en la nube. Si quieres, aquí puedes terminar el taller.

Los otros recorridos amplían los escenarios desde distintas interfaces:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración de MCP directamente desde el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega desde el terminal dos cambios revisados con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y un puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas;
- crea un agente personalizado para otro ámbito;
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración del agente][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente en la nube][agent-firewall]

---

| [← Ejercicio anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
