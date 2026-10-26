---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Requisitos previos][previous-lesson] · [Siguiente ejercicio: Modo de agente →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe hacerse de una forma concreta o Copilot necesita información de fondo, conviene asegurarse de que ese contexto esté disponible. Hay varias formas de proporcionárselo, entre ellas los [archivos de instrucciones][instruction-files], que guían la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las directrices de desarrollo y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos con ámbito de ruta;
- generarás el primer fragmento de datos del filtrado (un helper de editoriales) con las instrucciones *actuales*;
- añadirás un estándar general del repositorio a `.github/copilot-instructions.md`;
- repetirás el mismo prompt y observarás cómo el código generado adopta el nuevo estándar;
- confirmarás la actualización de las instrucciones y el fragmento de filtrado en `main` para que Copilot pueda utilizar las directrices actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede desviarse de algunos estándares. Copilot no es determinista. El objetivo de este ejercicio es observar la *tendencia* del cambio de comportamiento tras actualizar las instrucciones, no conseguir una salida idéntica carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe estar en modo oscuro y tener un aspecto moderno.
- El código debe incluir comentarios de documentación TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que explique su función.

Mediante archivos de instrucciones, te asegurarás de que Copilot disponga de la información adecuada para realizar el trabajo conforme a estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para ayudarle a comprender mejor tu estilo de desarrollo y tus requisitos. Puedes indicar convenciones de desarrollo, bibliotecas y los tipos de comentarios que quieres incluir en el código. Puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot para **todas** las solicitudes del repositorio. Debe contener información del proyecto pertinente para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción de lo que se está creando, buenas prácticas y otras directrices generales.
- Los archivos `.github/instructions/*.instructions.md`, que se pueden crear para tareas o tipos de archivo concretos. Sirven para proporcionar directrices para lenguajes como TypeScript o Astro, o para tareas como crear un componente de interfaz o un nuevo conjunto de pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para las finalizaciones ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y Copilot cloud agent utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` (con frontmatter `applyTo`) para generar código.
>
> Copilot también [admite archivos de instrucciones con otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar archivos de instrucciones

Una conversación completa sobre cómo crear estos archivos queda fuera del alcance del taller. Los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en directrices generales del proyecto, como su descripción, estructura y estándares globales.
- Utiliza `*.instructions.md` para tipos de archivo (pruebas unitarias, componentes Astro y capa de datos) o tareas concretas.
- Usa lenguaje natural y directrices claras, con ejemplos de cómo debe ser el código y de cómo no.

No existe una única forma de crear archivos de instrucciones, igual que no existe una única forma de usar la IA. La experimentación te permitirá descubrir qué funciona mejor.

> [!TIP]
> Los proyectos que usan GitHub Copilot deberían contar con una colección sólida de archivos de instrucciones. En este proyecto encontrarás, entre otros, archivos para [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede generar archivos de instrucciones. Cada interfaz lo ofrece de forma distinta, por ejemplo **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI; la lección de cada interfaz lo indicará cuando corresponda.
>
> Para buscar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas del proyecto

Lee los archivos de instrucciones incluidos en el proyecto: encontrarás un `copilot-instructions.md` principal y varios archivos `*.instructions.md` para tareas distintas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la descripción breve del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**. Se aplican a todas las interacciones con Copilot.
3. Explora la carpeta `.github/instructions`, donde hay directrices para archivos Astro, la capa de datos de Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` de la parte superior establece un glob, relativo a la raíz del repositorio, que determina a qué archivos se aplican las instrucciones. En este caso coincide con archivos de prueba TypeScript, como `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir instrucciones extensas en archivos reutilizables y mostrar ejemplos a Copilot. (Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio).

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares del proyecto, pero todavía no exige documentación dentro del código. A continuación añadirás reglas para los comentarios de documentación TSDoc y los encabezados de archivo.

## Crear una rama para los cambios

Sigue las buenas prácticas y crea una rama para estos cambios.

1. Vuelve a tu Codespace y abre un terminal.
2. Pulsa <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) para abrir un terminal nuevo.
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abrir Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea pertinente.

Para observar el efecto de las instrucciones, empieza enviando un prompt con el archivo de instrucciones actual. Más adelante lo actualizarás y volverás a enviar el mismo prompt.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot solo utilice el contexto que necesitas.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe situar el helper.
3. Selecciona **Agent** en el selector de agentes de la vista Chat para que Copilot pueda aplicar los cambios.

   ![Captura del selector de agente en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará actualizaciones, que a menudo abarcan el archivo helper y sus pruebas.
6. Observa que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Esto procede de las convenciones de la capa de datos en `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Fíjate en que al código propuesto **le faltan** los comentarios TSDoc y el encabezado de comentario a nivel de archivo.

> [!CAUTION]
> Como Copilot es probabilístico, puede que añada comentarios TSDoc aunque no se lo indiques. No pasa nada: el objetivo sigue siendo observar la mejora de *consistencia* después de actualizar las instrucciones.

## Añadir un nuevo estándar del repositorio

Como se indicó antes, `.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Añadiremos los estándares de desarrollo del repositorio para mejorar las sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Documenta los estándares de desarrollo, pero todavía no incluye reglas para la documentación dentro del código.
3. Añade estas líneas Markdown justo debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste en la lección anterior, los archivos de instrucciones pueden definirse a nivel de repositorio (`.github/copilot-instructions.md`) para las directrices globales, o como archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas específicas. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla de comentarios de documentación que acabas de añadir.

## Volver a enviar el prompt y observar el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para limpiar el contexto.
2. Selecciona `src/lib/publishers.ts` para centrar el contexto de Copilot en el archivo correcto.
3. Envía el **mismo prompt** de antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Helpers de acceso a datos de editoriales para la plataforma de financiación
    * colectiva de Tailspin Toys.
    * Proporciona funciones para recuperar información de editoriales de la base de datos.
    */
   ```

5. Observa que la función propuesta incluye un comentario TSDoc similar a este:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su id y su nombre.
    *
    * @param db - El cliente de base de datos de Drizzle.
    * @returns Una promesa que se resuelve en una matriz de editoriales.
    */
   ```

Has guiado a Copilot para que siga un nuevo estándar y lo aplique al código en el que se basará el siguiente ejercicio.

## Confirmar las instrucciones y enviar el cambio

1. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que las instrucciones y el helper están actualizados:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de las instrucciones y todos los archivos del helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados (por ejemplo, pruebas), añade también esos archivos.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y combinar una solicitud de incorporación de cambios

Ya has enviado la rama. Ahora abrirás una PR vinculada a la incidencia sobre las instrucciones del backlog. Copilot puede buscar la incidencia, crear la PR para cerrarla y combinarla.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot realiza acciones en GitHub. La plantilla del proyecto ya configura las herramientas de GitHub, así que no tienes que hacer nada. La primera vez que Copilot use una herramienta, VS Code puede pedirte que inicies sesión en GitHub; sigue las instrucciones para autorizarlo. En un ejercicio posterior aprenderás cómo funciona esta conexión (Model Context Protocol).

1. Abre Copilot Chat en tu Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado.
3. Pide a Copilot que busque la incidencia relacionada con la actualización de los archivos de instrucciones y cree una PR desde la rama actual. Indícale que describa tanto la actualización de las instrucciones como la base del helper de editoriales:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la PR.
5. Cuando Copilot te pida permiso para realizar acciones de GitHub, revisa el comando y selecciona **Allow** si corresponde.
6. Cuando se cree la PR, pídele que la combine y que vuelva a `main`:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Cuando Copilot te pida permiso para realizar acciones de GitHub o ejecutar comandos del shell, revisa el comando y selecciona **Allow** si corresponde.

¡Ya has creado y combinado una solicitud de incorporación de cambios con ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones y después has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *existentes*;
- añadir un estándar general del repositorio a `.github/copilot-instructions.md`;
- volver a enviar el mismo prompt y observar cómo el código propuesto adopta el estándar nuevo;
- confirmar las instrucciones y la base del helper en `main` para que el siguiente ejercicio pueda basarse en ellas.

A continuación, utilizarás esas instrucciones en el [modo de agente][next-lesson] para que Copilot añada una funcionalidad en varios puntos del código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [5 consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales personalizadas para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
