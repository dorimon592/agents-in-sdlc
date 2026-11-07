---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Muchas organizaciones tienen deuda técnica: problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidad que llevan tiempo en el backlog. El agente de GitHub Copilot en la nube puede actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, abre una solicitud de incorporación de cambios (PR) en borrador para que una persona revise el resultado. Así puedes delegar tareas repetitivas y dedicar tiempo a cuestiones más importantes.

En este ejercicio explorarás cómo:

- personalizar el entorno de generación de código.
- asegurar que las operaciones se realicen de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys quiere resolver varios problemas. En primer lugar, el código carece de documentación: el equipo quiere añadir comentarios TSDoc a todas las funciones exportadas. Además, el equipo de diseño quiere que cada página de detalles de un juego muestre otros títulos de la misma categoría, para que las personas puedan seguir explorando. No hace falta una implementación pulida; solo una versión que permita probar la experiencia. Este trabajo se ha bloqueado temporalmente porque hay otras incidencias prioritarias.

Son tareas que suelen quedar relegadas y que puedes asignar al agente de Copilot en la nube. El agente las resolverá de forma asíncrona mientras tú te ocupas de otras tareas. Después, volverás para revisar el resultado.

## Introducción al agente de GitHub Copilot en la nube

[El agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a una persona desarrolladora. Por ejemplo, puedes [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Cuando se la asignas, Copilot crea una solicitud de incorporación de cambios en borrador para hacer un seguimiento, configura un entorno y empieza a trabajar. Puedes consultar la sesión mientras sigue en curso o cuando haya terminado. Cuando la solución esté lista para que la revises, Copilot te mencionará en la solicitud.

## La importancia de definir bien el alcance

GitHub Copilot no hace magia: no puedes saltarte el análisis. Incluso las tareas aparentemente sencillas pueden ser complejas. Por eso, [define cuidadosamente el alcance de las tareas para el agente en la nube][cloud-agent-best-practices]. Trátalo como un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al añadir IA generativa.

## Instrucciones personalizadas del repositorio

En ejercicios anteriores has visto cómo las instrucciones personalizadas guían a Copilot. Si ya completaste el ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción. Si no, revísalo ahora. Antes de asignar trabajo al agente, consulta los archivos de instrucciones del repositorio para poder reconocer su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un codespace:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente la incidencia `Code lacks documentation`, observa en la solicitud resultante cómo se aplican los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentario. Estas pautas provienen de los archivos de instrucciones. Volverás a examinarlas al revisar las solicitudes en un ejercicio posterior.

## Configura el entorno de desarrollo del agente de Copilot en la nube

La creación de código suele requerir un entorno concreto y scripts de configuración. Lo mismo ocurre cuando asignas tareas a Copilot, que trabaja de forma parecida a una persona ingeniera de software.

El agente de Copilot en la nube usa [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo con un [flujo de trabajo de configuración][setup-workflow] especial, definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece. Así tendrá acceso a las herramientas y dependencias necesarias. El taller ya configura este flujo de trabajo: instala Node.js y las dependencias, los navegadores de Playwright para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada de la aplicación Astro.

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de la configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en estos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (ahora toda la aplicación usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y precarga la base de datos SQLite local para que las compilaciones y pruebas dispongan de datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud.
- El activador `workflow_dispatch` permite ejecutar el flujo manualmente desde la pestaña **Actions** del repositorio. Resulta útil para comprobar que se ejecuta correctamente sin esperar a que Copilot lo inicie.

## Añade documentación

Aunque la importancia de la documentación es evidente, muchos proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica suele quedar desatendida, reduce la productividad y dificulta mantener el código o incorporar a nuevas personas al equipo. Copilot puede ayudar a crear documentación; así que puedes asignarle esta incidencia y revisar el resultado en un ejercicio posterior.

1. En una nueva pestaña del navegador, ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en **Title**.
6. En **Description**, escribe:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo de asignación.

  ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests** y abre la nueva solicitud, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si todavía no aparece, espera un momento y actualiza la página.
11. Después de unos minutos, Copilot debería haber creado una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer. Copilot está preparando el entorno mediante el flujo de trabajo anterior, analizando el proyecto y determinando cómo abordar la tarea.

12. Revisa las tareas que va a completar.
13. Desplázate por la cronología de la solicitud para ver cuándo Copilot empieza a trabajar.
14. Selecciona **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

15. Recorre la sesión en directo y observa cómo Copilot explora el código, analiza el estado, hace una pausa para decidir el plan y crea código.

El proceso puede tardar varios minutos. Una ventaja del agente en la nube es que puede trabajar de forma asíncrona mientras tú te ocupas de otras tareas. Aprovecha esa posibilidad y asígnale otra incidencia.

## Añade una sección de juegos relacionados a la página de detalles

Puedes dividir el trabajo: mientras el agente ayuda al equipo de diseño con los juegos relacionados, tú puedes ocuparte de otra tarea. Asígnale esa incidencia.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. Escribe `Show related games on the game details page` en **Title**.
6. En **Description**, escribe:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de diálogo.

  ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, verás varios 👀 en el primer comentario de la incidencia. Indican que Copilot está trabajando en ella.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. Como trabaja de forma parecida a una persona desarrolladora, no tienes que supervisarlo continuamente: revisa el resultado cuando termine. Mientras tanto, aprenderás a crear y usar agentes personalizados.

## Resumen y próximos pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias para que las resuelva de forma asíncrona. Úsalo para abordar deuda técnica, crear funcionalidades o ayudar a migrar código entre frameworks.

Has aprendido a:

- personalizar el entorno de generación de código.
- asegurar que las operaciones se realicen de forma segura.
- definir incidencias con un alcance claro.
- asignar incidencias a Copilot.

Mientras el agente trabaja, puedes crear y usar agentes personalizados. El [agente de Copilot en la nube también admite servidores MCP][cloud-agent-mcp] y puede utilizar las instrucciones personalizadas que exploraste en ejercicios anteriores.

## Recursos

- [Acerca del agente de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
