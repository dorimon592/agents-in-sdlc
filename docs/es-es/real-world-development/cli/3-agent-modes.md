---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear los filtros desde una incidencia y el modo Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezaste añadiendo una funcionalidad pequeña. Los cambios más amplios necesitan un proceso más sólido. GitHub Copilot CLI está diseñado para adaptarse al flujo de trabajo de una organización y ayudar a crear lo adecuado de la forma correcta. En esta lección iniciarás un proceso típico de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás el código y el comportamiento y, finalmente, lo integrarás en el proyecto.

> [!NOTE]
> Continuarás el flujo de trabajo de la funcionalidad en la misma conversación y rama. Normalmente usarías ramas o solicitudes distintas para tipos de cambios diferentes, pero aquí simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot desde una incidencia de GitHub.
- definirás los requisitos en el modo Plan.
- implementarás la funcionalidad con el modo Autopilot.
- revisarás el código.
- comprobarás manualmente la funcionalidad en un navegador reenviado.

Más adelante actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás validación MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys crece y las personas necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero es necesario acordar algunos detalles, como la combinación de categorías, antes de programar. Usarás Plan para tomar esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

La IA no cambia los fundamentos del desarrollo de software; de hecho, los hace aún más importantes. Un flujo habitual incluye:

1. Partir de una incidencia que describa el trabajo.
2. Planificar qué se debe crear.
3. Crear y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código después de revisarlo y superar la integración continua.

> [!NOTE]
> Los detalles varían entre equipos y organizaciones, pero la mayoría sigue alguna versión de este proceso.

Al mantener este enfoque, te aseguras de que el código generado por IA cumpla los requisitos y pase por el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** determina cuánta autonomía tiene el agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar:

- **Interactive**: trabajas con el agente; propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma, escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Parte de la incidencia

Antes de empezar a trabajar en los filtros, vuelve al Codespace y comprueba que el repositorio y el terminal estén listos.

1. Vuelve al Codespace. Si está detenido, reinícialo.
2. Confirma que se ha combinado la PR de valoraciones por estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para los filtros:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de dirección para resaltar la incidencia **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

El prompt empezará con `#7` (o un número parecido). El carácter `#` permite añadir al contexto una incidencia o una solicitud de GitHub.

## Planifica los filtros

La planificación permite acordar el enfoque antes de implementar una funcionalidad o realizar una tarea. Es especialmente útil para trabajos complejos. Cambia al modo Plan y pide a Copilot que cree un plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar a Plan. Confirma que el indicador debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia que añadiste en el paso anterior, escribe:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot explorará el proyecto y determinará el enfoque adecuado.

3. Puede que Copilot haga preguntas sobre el comportamiento de los filtros. Respóndelas según tus preferencias; no hay una única respuesta correcta.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan y revísalo.
6. Pide a Copilot que cambie cualquier parte que no coincida con tus decisiones.

## Aprueba Autopilot

Después de redactar y revisar el plan, puedes pedir a Copilot que lo implemente. Autopilot le permite iterar hasta considerar que ha terminado.

1. Selecciona **Accept plan and build on autopilot (recommended)** o una opción con nombre similar en la versión instalada.
2. Confirma que el indicador debajo del prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa el plan antes. Si Copilot indica que faltan dependencias o que el puerto está ocupado, resuelve el problema de configuración antes de dar por terminadas las comprobaciones.

## Revisa y comprueba la implementación

Como cualquier otro código, los cambios generados deben revisarse antes de combinarlos. Revisa el código e inicia el sitio para confirmar que funciona.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para volver al modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` y revisa la implementación de los filtros y sus pruebas.
3. Compara el resultado con la incidencia y las decisiones de planificación.
4. Al terminar la revisión, pulsa <kbd>Esc</kbd> para cerrar la vista de diferencias.
5. Revisa los resultados de las comprobaciones y pide a Copilot que corrija cualquier error.

## Explora la nueva funcionalidad

¿Funciona el código? Inicia la aplicación como antes y ábrela desde el puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros de categoría, los de editorial y las combinaciones acordadas durante la planificación.
4. Confirma que el restablecimiento y los resultados vacíos se comportan según la incidencia y tus decisiones.
5. Vuelve al Codespace y pide a Copilot que detenga el servidor de desarrollo.

## Resumen y próximos pasos

Has utilizado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección:

- has iniciado una conversación de Copilot desde una incidencia de GitHub.
- has definido los requisitos en el modo Plan.
- has implementado la funcionalidad con Autopilot.
- has revisado el código.
- has probado la funcionalidad en un navegador reenviado.

A continuación, usarás [instrucciones personalizadas][next-lesson] para que el código siga las prácticas documentadas.

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
