---
title: "Lección 2 - Añadir valoraciones por estrellas: una mejora rápida"
description: "Usa Copilot CLI para hacer un cambio pequeño en las tarjetas de juegos, revísalo en un navegador reenviado y combínalo como primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has instalado Copilot CLI y probado una conversación. Ahora harás el primer cambio al proyecto. Será pequeño: los juegos ya tienen una valoración por estrellas en sus datos, pero las tarjetas de la página de inicio no la muestran. Pedirás al agente que la muestre, revisarás el cambio y lo combinarás como tu primera solicitud de incorporación de cambios.

En esta lección:

- iniciarás una conversación de Copilot centrada en una rama de funcionalidad.
- pedirás al agente que haga un cambio pequeño.
- revisarás el cambio con `/diff`.
- ejecutarás la aplicación y comprobarás el cambio en un navegador reenviado.
- abrirás y combinarás tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración por estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas de la página de inicio solo muestran el título, la categoría, la editorial y la descripción. Como calentamiento, pedirás al agente que muestre la valoración existente en cada tarjeta. Es un cambio pequeño y autónomo, ideal para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI usa el repositorio y la rama seleccionada en el terminal, no crea un árbol de trabajo específico. Las conversaciones guardadas permiten volver al mismo intercambio, mientras que los archivos y la rama siguen siendo el estado habitual de Git en el disco.

En una conversación verás los prompts y las respuestas del agente, su actividad con herramientas al explorar y editar archivos, y los cambios que puedes examinar con `/diff`.

## Inicia una conversación y solicita el cambio

Inicia una conversación nueva para implementar la funcionalidad.

1. Vuelve al Codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Si Copilot no está abierto, inicia la aplicación con este comando:

   ```bash
   copilot --yolo
   ```

4. Comprueba que se inicia una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Envía este prompt:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot explorará el proyecto, localizará los archivos que muestran los detalles de los juegos y creará el código necesario. ¡Ya has añadido una funcionalidad con Copilot CLI!

## Revisa las diferencias

Antes de combinar cualquier cambio generado por IA, aunque sea pequeño, revísalo en Copilot CLI.

1. Escribe `/diff` y revisa todos los archivos modificados.
2. Confirma que las tarjetas muestran la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones que ejecutó Copilot y pídele que corrija los errores.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.

> [!NOTE]
> Copilot, como todas las herramientas de IA generativa, es probabilístico, no determinista. El código exacto puede variar. Revisa el comportamiento en lugar de esperar una implementación idéntica.

## Comprueba los cambios

No basta con leer el código y dar por sentado que funciona. Pide a Copilot que inicie el sitio para examinar la interfaz actualizada en el navegador reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas muestran las valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que inició:

   ```plaintext
   Stop the development server you started.
   ```

## Abre y combina tu primera solicitud de incorporación de cambios

Ya has creado la funcionalidad. Ahora abrirás una solicitud de incorporación de cambios (PR) para integrarla en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Escribe `/pr create`. Copilot CLI enviará la confirmación existente al crear la PR y mostrará su URL.
3. Abre la solicitud manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté lista, selecciona **Merge pull request** y confirma la combinación.
6. Vuelve al Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza la rama local `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y próximos pasos

¡Enhorabuena! Has publicado tu primer cambio con GitHub Copilot CLI. En concreto:

- has iniciado una conversación centrada en una rama de funcionalidad.
- has pedido al agente que modifique las tarjetas.
- has revisado el cambio con `/diff`.
- has comprobado la valoración por estrellas en el navegador reenviado.
- has abierto y combinado tu primera solicitud de incorporación de cambios.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para crear una funcionalidad más amplia.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
