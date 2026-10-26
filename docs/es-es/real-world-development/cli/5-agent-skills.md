---
title: "Lección 5 - Personalizar y usar una habilidad de comprobaciones de calidad"
description: "Explora la habilidad existente quality-checks, personaliza el formato del informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escribir código no es lo único que hace falta. Ya has comprobado manualmente que el código funciona y has usado archivos de instrucciones para garantizar que sigue los estándares del equipo. Pero ¿qué ocurre con las pruebas, el lint y otros elementos de la integración continua (CI)?

Las **habilidades de agente** son adecuadas para este tipo de tareas. Ayudan a Copilot a ejecutar correctamente operaciones como estas.

En esta lección:

- explorarás la habilidad existente `quality-checks`;
- personalizarás el formato de sus resultados;
- volverás a cargar la habilidad y la ejecutarás.

## Escenario

Tailspin Toys usa la habilidad `quality-checks` para las pruebas unitarias, el lint y la comprobación de tipos. El equipo quiere mejorar el informe para que resulte más fácil de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos de apoyo que el agente carga cuando los necesita. Una habilidad es, en esencia, una carpeta que contiene un archivo Markdown `SKILL.md`. El archivo incluye frontmatter con un nombre y una descripción, información general y directrices sobre cuándo se debe utilizar. La carpeta también puede contener scripts y otros recursos.

> [!NOTE]
> Una habilidad no necesita carpetas ni archivos adicionales. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque utiliza los comandos existentes del proyecto.

Las habilidades pueden estar en `.github/skills` para compartirlas y reutilizarlas en el equipo, o en la carpeta personal `~/.copilot/skills`.

## Explorar la habilidad

Explora la habilidad `quality-checks` que creó el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu Codespace. En el editor, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a saber cuándo debe utilizar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot por el proceso de pruebas y lint.
4. Fíjate en que la habilidad aún no incluye una sección **Results output formatting**.

## Ejecutar la habilidad antes de cambiarla

Puedes invocar habilidades directamente desde Copilot CLI o pedirlas con lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Usa este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Revisa el informe final.

## Personalizar el informe

Mejoremos el informe para que indique qué se ejecutó, si tuvo éxito y qué resultados devolvieron las herramientas.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecutar la habilidad actualizada

Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Usa exactamente el mismo prompt que antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Revisa el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y utilizado una habilidad de agente existente. En esta lección:

- has explorado `quality-checks` para las pruebas unitarias, el lint y las comprobaciones de tipos;
- has personalizado el formato de los resultados;
- has vuelto a cargar y ejecutar la habilidad.

Ese cambio acompañará al filtrado en la PR de la funcionalidad. A continuación, permitirás a Copilot interactuar directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos comunitarios son referencias, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación incluido: `drawio`][drawio-example]
- [Pruebas de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
