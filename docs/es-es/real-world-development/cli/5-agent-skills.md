---
title: "Lección 5 - Personalizar y usar la habilidad quality-checks"
description: "Explora la habilidad quality-checks, personaliza el formato de su informe y úsala para validar los filtros."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste solo en escribir código. Ya has comprobado manualmente que funciona y has utilizado archivos de instrucciones para que siga los estándares del equipo. ¿Y las pruebas, el lint y el resto de la integración continua (CI)?

Las **habilidades de agente** son adecuadas para estas tareas. Ayudan a Copilot a entender cómo realizar operaciones como estas.

En esta lección:

- explorarás la habilidad `quality-checks` existente.
- personalizarás el formato de los resultados.
- volverás a cargar la habilidad y la ejecutarás.

## Escenario

Tailspin Toys usa la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que sea más fácil de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente reúnen instrucciones reutilizables, scripts ejecutables y recursos complementarios que el agente carga cuando los necesita. En esencia, son una carpeta con el nombre de la habilidad y un archivo Markdown `SKILL.md`. El archivo incluye un frontmatter con el nombre y la descripción, una explicación de la función y pautas sobre cuándo usarla. También puede contener subcarpetas con scripts y otros recursos.

> [!NOTE]
> Una habilidad no necesita otras carpetas ni archivos. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md`, porque utiliza los comandos existentes del proyecto.

Las habilidades pueden estar en `.github/skills` para que el equipo las reutilice desde el repositorio o en la carpeta personal de habilidades `~/.copilot/skills`.

## Explora la habilidad

Explora `quality-checks`, la habilidad que creó el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve al Codespace. En el editor, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a decidir cuándo utilizar la habilidad.
3. Lee las instrucciones y observa cómo guían las pruebas y el lint.
4. Fíjate en que todavía no hay una sección **Results output formatting**.

## Ejecuta la habilidad antes de hacer cambios

Las habilidades se pueden invocar directamente desde Copilot CLI o mediante lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Revisa el informe.

## Personaliza el informe

Queremos que el informe indique qué se ejecutó, si tuvo éxito y qué resultados comunicaron las herramientas. Actualiza la habilidad para que genere ese informe.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecuta la habilidad actualizada

Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía el mismo prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Revisa el informe y compáralo con el primero.

## Resumen y próximos pasos

Has personalizado y utilizado una habilidad de agente existente. En esta lección:

- has explorado la habilidad `quality-checks` para pruebas unitarias, lint y comprobaciones de tipos.
- has personalizado el formato del informe.
- has vuelto a cargar y ejecutado la habilidad.

Ese cambio acompañará a los filtros en la PR. A continuación, permitirás que Copilot interactúe con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad sirven de referencia, no son tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación: `drawio`][drawio-example]
- [Pruebas del navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
