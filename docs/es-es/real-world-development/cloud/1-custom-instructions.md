---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Ejercicio anterior: Requisitos previos][previous-lesson] · [Siguiente ejercicio: Agente de GitHub Copilot en la nube →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea tiene que realizarse de una forma concreta o Copilot necesita información de fondo, conviene que ese contexto esté disponible. Los [archivos de instrucciones][instruction-files] proporcionan esas directrices para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo*.

En este ejercicio:

- explorarás cómo llegan al agente en la nube el contexto del proyecto, las directrices de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y los archivos con ámbito de ruta;
- añadirás un estándar general del repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los recorridos de VS Code y CLI, aquí no ejecutarás un prompt *antes y después*. El agente de GitHub Copilot en la nube trabaja de forma asíncrona en incidencias de GitHub, por lo que es más difícil mostrar el efecto en tiempo real. Verás la influencia del archivo de instrucciones más adelante, cuando revises las solicitudes de incorporación de cambios que genere.

## Archivos de instrucciones

### Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices para sus prácticas:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe estar en modo oscuro y tener un aspecto moderno.
- El código debe incluir comentarios de documentación TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que explique su función.

Los archivos de instrucciones permiten proporcionar a Copilot el contexto adecuado para que realice el trabajo conforme a estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias para que Copilot comprenda tu estilo de desarrollo y tus requisitos. Puedes especificar convenciones, bibliotecas y los tipos de comentarios que quieres incluir en el código. También puedes crear instrucciones para todo el repositorio o para tipos de archivo y tareas concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot para **todas** las solicitudes del repositorio. Debe contener información general del proyecto, como la pila tecnológica, una descripción de lo que se está construyendo, buenas prácticas y otras directrices.
- `.github/instructions/*.instructions.md`, archivos que puedes crear para tareas o tipos de archivo concretos. Sirven para proporcionar directrices para lenguajes como TypeScript o Astro y para tareas como crear componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En un IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat; no se usan para las finalizaciones ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` (con frontmatter `applyTo`) para generar código.
>
> Copilot también [admite archivos de instrucciones con otros estándares][custom-instructions-support], incluidos AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar archivos de instrucciones

El proyecto de ejemplo muestra un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en directrices del proyecto, como la descripción, la estructura y los estándares globales.
- Utiliza archivos `*.instructions.md` para tipos de archivo, como pruebas unitarias, componentes Astro o la capa de datos, y para tareas concretas.
- Usa lenguaje natural, directrices claras y ejemplos de cómo debe ser el código y de cómo no.

No existe una única forma de crear archivos de instrucciones, igual que no existe una única forma de usar IA. La experimentación te ayudará a descubrir qué funciona mejor.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían tener una colección sólida de archivos de instrucciones. En este proyecto verás, entre otros, archivos para [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede generar archivos de instrucciones. Cada interfaz lo ofrece de forma distinta, por ejemplo **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI; la lección correspondiente lo indicará cuando sea necesario.
>
> Para buscar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio lleno de archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas de este proyecto

Dedica unos minutos a leer los archivos que incluye el repositorio: un `copilot-instructions.md` principal y varios `*.instructions.md` para tareas distintas. Puedes abrirlos en tu editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**, aplicables a las interacciones con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Encontrarás instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` establece un glob (relativo a la raíz del repositorio) que determina qué archivos coinciden; por ejemplo, los archivos TypeScript que sigan el patrón `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Observa los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir instrucciones extensas en archivos reutilizables y proporcionar ejemplos a Copilot. (Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio).

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero todavía no requiere documentación dentro del código. A continuación añadirás reglas para los comentarios de documentación TSDoc y los encabezados de archivo.

## Añadir un nuevo estándar del repositorio

El único cambio que harás en este ejercicio es añadir un estándar general a `.github/copilot-instructions.md`. Así mejorarás las sugerencias de código de Copilot.

Antes de editar, crea una rama. El agente en la nube leerá las instrucciones de la rama a la que se dirige la incidencia, pero también confirmarás los cambios con claridad:

1. Desde el terminal de Codespace, crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Vuelve a abrir `.github/copilot-instructions.md`.
3. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Documenta los estándares de desarrollo, pero todavía no incluye directrices para la documentación dentro del código.
4. Añade estas líneas Markdown justo debajo de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. Guarda `copilot-instructions.md`.

> [!TIP]
> Los archivos de instrucciones pueden definirse a nivel del repositorio (`.github/copilot-instructions.md`) para establecer directrices globales, o como `*.instructions.md` para lenguajes, tipos de archivo o tareas concretas. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla de comentarios que acabas de añadir.

## Confirmar, enviar y combinar la actualización de las instrucciones

El agente en la nube lee las instrucciones de la rama a la que se dirige la incidencia. En el siguiente ejercicio asignarás una incidencia a Copilot desde `main`; por tanto, los cambios de instrucciones tienen que llegar a `main` para que el agente pueda utilizarlos.

1. Prepara y confirma el cambio:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios de `custom-instructions-cloud` a `main` y combínala. El agente en la nube leerá las instrucciones cuando trabaje en incidencias asignadas en los siguientes ejercicios.

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones y has añadido un estándar general del repositorio para que el agente en la nube lo siga. Verás sus efectos en las solicitudes de incorporación de cambios que genere durante el resto del taller.

A continuación, [asignarás tu primera incidencia al agente de GitHub Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [5 consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
