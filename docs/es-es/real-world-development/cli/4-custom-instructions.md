---
title: "Lección 4 - Guiar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es clave al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta, conviene que Copilot disponga de esas directrices. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has implementado el filtrado, explorarás las instrucciones utilizadas por Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y los archivos de instrucciones con ámbito de ruta;
- actualizarás las instrucciones para que se sigan los estándares de desarrollo;
- comprobarás el efecto de los archivos de instrucciones en el código.

## Escenario

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo. Entre ellos:

- Los comentarios deben explicar la intención y las decisiones que no sean obvias, no limitarse a repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar con TSDoc/JSDoc su propósito, parámetros y valores devueltos, incluido el argumento `db` inyectable cuando exista.
- Los componentes Astro reutilizables deben documentar su contrato `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Deben conservarse las directrices de formato y lint existentes.

Mediante archivos de instrucciones, te asegurarás de que Copilot disponga de la información adecuada para realizar el trabajo conforme a estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan contexto y preferencias a Copilot para ayudarle a comprender tu estilo y requisitos de desarrollo. Puedes especificar convenciones, bibliotecas e incluso los comentarios que quieres incluir en el código. Puedes crear instrucciones para todo el repositorio o para tipos de archivo y tareas concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`, un único archivo que se envía a Copilot para **todas** las solicitudes del repositorio. Debe contener información del proyecto que sea pertinente para la mayoría de las solicitudes.
- Los archivos `.github/instructions/*.instructions.md`, que proporcionan directrices para lenguajes, tipos de archivo o tareas concretos.

> [!NOTE]
> Los formatos de instrucciones adicionales y su compatibilidad dependen del entorno. Antes de confiar en un formato concreto, consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support].

## Explorar los archivos de instrucciones personalizadas del proyecto

El proyecto inicial ya incluye archivos de instrucciones. Explora su contenido antes de cambiar nada para poder observar el efecto de la actualización.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces, no en el terminal, abre `.github/copilot-instructions.md`.
3. Explora la descripción breve del proyecto y las directrices de desarrollo. Se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre la carpeta `.github/instructions` y explora los archivos. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y otros temas.
5. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` de la parte superior establece un glob que determina a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y observa los enlaces a otros archivos de instrucciones y a los archivos del proyecto. Así puedes dividir instrucciones extensas en archivos reutilizables y mostrar ejemplos a Copilot.

## Actualizar las instrucciones para que reflejen las directrices del equipo

Los archivos existentes son un buen comienzo, pero todavía falta algo. Modifica el archivo principal `copilot-instructions.md` para indicar que el código TypeScript nuevo debe incluir comentarios TSDoc.

1. En `.github/copilot-instructions.md`, localiza la sección **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade esta viñeta al final:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Usar las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar las instrucciones, sal y reanuda la conversación de filtrado para que Copilot las cargue sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit`. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio y en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.
3. Pide a Copilot que aplique las instrucciones actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y lee los archivos TypeScript modificados. Observa los comentarios TSDoc nuevos y confirma que describen el código correctamente.

## Resumen y siguientes pasos

Has explorado cómo Copilot CLI utiliza los archivos de instrucciones del repositorio y has aplicado un estándar nuevo a tu funcionalidad. En concreto:

- has explorado cómo llegan al agente las instrucciones del repositorio y los archivos con ámbito de ruta;
- has actualizado las instrucciones para garantizar que se sigan los estándares de desarrollo;
- has comprobado el efecto de los archivos de instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable `quality-checks`][next-lesson] para comprobar de forma coherente el lint y las pruebas.

## Recursos

- [Añadir instrucciones personalizadas a Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
