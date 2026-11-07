---
title: "Lección 4 - Guiar a Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es esencial al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta, asegúrate de que Copilot tenga acceso a esas directrices. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado los filtros, explorarás las instrucciones utilizadas por Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas.
- actualizarás el archivo de instrucciones para aplicar los estándares de programación.
- observarás el efecto de los archivos de instrucciones en el código.

## Escenario

Tailspin Toys tiene pautas y requisitos para sus prácticas de programación:

- Los comentarios deben explicar la intención y las decisiones que no sean evidentes, no repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, parámetros y valores devueltos mediante TSDoc/JSDoc, incluido el argumento `db` inyectable cuando exista.
- Los componentes Astro reutilizables deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados al cambiar el código correspondiente.
- Se deben conservar las directrices de formato y lint existentes.

Usarás archivos de instrucciones para proporcionar a Copilot el contexto necesario para respetar estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que entienda mejor el estilo de programación y los requisitos. Permiten orientar sus sugerencias con convenciones, bibliotecas y tipos de comentarios preferidos.

Hay dos tipos de archivos:

- `.github/copilot-instructions.md` se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general relevante para la mayoría de las tareas.
- `.github/instructions/*.instructions.md` contiene pautas para lenguajes, tipos de archivos o tareas concretos.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones varía según el entorno. Consulta la [referencia de compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de depender de un formato concreto.

## Explora los archivos de instrucciones personalizadas del proyecto

El proyecto inicial ya incluye varios archivos de instrucciones. Revisa lo que hay antes de modificarlo.

1. Vuelve al Codespace.
2. En el editor de Codespaces, no en el terminal, abre `.github/copilot-instructions.md`.
3. Revisa la descripción del proyecto y las pautas de programación. Se aplican a todas las interacciones con Copilot en este repositorio.
4. Explora `.github/instructions`, donde hay instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y más.
5. Abre `.github/instructions/unit-tests.instructions.md` y observa su campo `applyTo`, que define a qué archivos se aplica el patrón.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa sus enlaces a otros archivos de instrucciones y a archivos existentes del proyecto. Así puedes dividir conjuntos grandes en archivos reutilizables y proporcionar ejemplos.

## Actualiza los archivos de instrucciones según las pautas del equipo

Los archivos actuales son un buen comienzo, pero falta una directriz. Modifica `copilot-instructions.md` para pedir comentarios TSDoc al generar TypeScript nuevo.

1. En `.github/copilot-instructions.md`, busca la sección **Code formatting guidance**, que debería estar cerca de la línea 35.
2. Añade este elemento al final:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

El archivo se guardará automáticamente.

## Aplica las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardarlas, sal y reanuda la conversación de filtrado para aplicar las nuevas directrices sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit`. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio en la rama `game-filters-cli`, reanuda la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está seleccionada la rama `game-filters-cli`.
3. Pide a Copilot que aplique las directrices actualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Observa los comentarios TSDoc nuevos y confirma que describen el código correctamente.

## Resumen y próximos pasos

Has explorado cómo Copilot CLI carga las instrucciones del repositorio y has aplicado un estándar nuevo a la funcionalidad. En concreto:

- has explorado cómo llegan al agente las instrucciones del repositorio y las instrucciones específicas de rutas.
- has actualizado el archivo de instrucciones para aplicar los estándares del equipo.
- has observado el efecto de las instrucciones en el código.

A continuación, [personalizarás y ejecutarás la habilidad reutilizable `quality-checks`][next-lesson] para ejecutar las pruebas y el lint de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
