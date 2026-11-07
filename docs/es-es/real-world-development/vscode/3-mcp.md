---
title: "Ejercicio 3 - Probar la funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear los filtros con el modo de agente. Antes de abrir una solicitud de incorporación de cambios, comprueba que funcionan en el navegador. En lugar de recorrer la aplicación manualmente, conectarás el **servidor MCP de Playwright** y pedirás a Copilot que pruebe la funcionalidad en un navegador real. Después, publicarás la rama y abrirás la solicitud.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo los servidores MCP amplían las herramientas de Copilot.
- añadirás el servidor MCP de Playwright al espacio de trabajo.
- pedirás a Copilot que lo use para probar los filtros en un navegador.
- publicarás la rama y abrirás una solicitud de incorporación de cambios.

## ¿Qué es Model Context Protocol (MCP)?

El modo de agente es mucho más potente cuando puede acceder a recursos externos al editor. Copilot utiliza Model Context Protocol (MCP) para comunicarse con herramientas y servicios externos mediante un estándar común.

![Diagrama del funcionamiento interno del modo de agente y su interacción con el contexto, el LLM y las herramientas, incluidas las que proporcionan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Los servidores MCP actúan como puente entre el agente de IA y las herramientas y los servicios externos. Gestionan la comunicación con estos servicios, como las API existentes o herramientas locales como los paquetes de NPM. Cada servidor MCP proporciona un conjunto distinto de herramientas y recursos.

Algunos servidores MCP populares:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: proporciona funciones de automatización del navegador mediante Playwright.

Hay muchos otros servidores MCP con acceso a distintas herramientas y recursos. GitHub aloja un [registro de MCP][mcp-registry] que facilita descubrir estos servidores y colaborar en el ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y ten en cuenta las implicaciones de seguridad. Usa únicamente servidores MCP de confianza y sé prudente al conceder acceso a recursos u operaciones sensibles.

## Revisa la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en el codespace.
2. Deberías ver que ya está configurado un servidor `github`:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla del proyecto incluye esta entrada `github`, por lo que Copilot pudo leer el backlog de incidencias en el ejercicio anterior. Usa el [servidor MCP remoto de GitHub][remote-github-mcp-server], así que no hace falta instalar nada localmente: VS Code se conecta por HTTP y te autenticas con GitHub la primera vez que Copilot usa una de sus herramientas.

## Añade el servidor MCP de Playwright

Ahora añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador controlable para probar la funcionalidad.

1. En `.vscode/mcp.json`, añade una entrada `playwright` junto a `github` para que el archivo quede así:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es **local**: VS Code lo inicia en tu equipo mediante `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin ventana visible. Es necesario en un codespace, que no tiene escritorio donde mostrarla.

> [!NOTE]
> El proyecto Tailspin Toys ya usa Playwright para sus pruebas de extremo a extremo, por lo que normalmente el navegador necesario ya está instalado. Si Copilot indica que falta el navegador, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Inicia el servidor y confía en él

VS Code inicia los servidores MCP **cuando se necesitan**: la primera vez que Copilot solicita las herramientas de un servidor, VS Code lo inicia y pide que confirmes si confías en él.

1. En `.vscode/mcp.json`, aparecerá la acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code te pregunta si confías en el servidor, revisa la configuración y confirma que confías en él para que se inicie.

Iniciarlo manualmente es opcional. Si no lo haces, VS Code lo iniciará y te pedirá que confirmes si confías en él la primera vez que Copilot necesite las herramientas, en el paso siguiente.

## Prueba los filtros

El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador real. En vez de comprobar la aplicación manualmente, puedes pedir al agente que abra una página, navegue por ella, aplique filtros, lea los resultados y te los resuma. Así puedes comprobar el comportamiento esperado sin salir de la conversación.

Internamente, el servidor MCP de Playwright usa el [árbol de accesibilidad][playwright-mcp-server] de la página en lugar de capturas de pantalla. El agente interpreta elementos estructurados y etiquetados, como botones, enlaces y elementos de lista, igual que las tecnologías de asistencia. Por eso, una comprobación funcional rápida también sirve como revisión básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe los filtros:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot abrirá un navegador mediante el servidor MCP de Playwright, seguirá los pasos y te comunicará el resultado. Compara el informe con los criterios de aceptación de la incidencia. Si algo no funciona como esperabas, formula preguntas de seguimiento o pide a Copilot que corrija el código antes de abrir la solicitud de incorporación de cambios.

> [!NOTE]
> La aplicación debe estar en ejecución en `http://localhost:4321`. Si has detenido el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot use el servidor MCP de Playwright puede que tenga que descargar un navegador. Si indica que falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/

## Publica la rama y crea una solicitud de incorporación de cambios

Ya has comprobado que la funcionalidad funciona. Ahora abrirás una solicitud de incorporación de cambios (PR) para que el equipo pueda revisarla. Primero, publica la rama `filtering-vscode`.

1. Abre el panel **Source Control** del codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de confirmación con el botón **Sparkle**.

   ![Captura del panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crea la solicitud de incorporación de cambios

Hay varias formas de crear una solicitud, como github.com o la interfaz de línea de comandos (CLI) de GitHub. Como ya estás usando GitHub Copilot, pídele que la cree. Puede buscar la incidencia correspondiente y vincularla a la solicitud.

1. Abre el panel Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Confirma que **Agent** está seleccionado en el selector de agentes para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree una solicitud:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Cuando sea necesario, selecciona **Continue** para permitir que Copilot recopile información y realice las operaciones. La primera vez que use una herramienta de GitHub, puede que tengas que autenticarte. Sigue las instrucciones.
5. Observa cómo Copilot busca las incidencias, encuentra la adecuada y crea la solicitud.
6. Selecciona el enlace que genera Copilot para revisar la solicitud, pero **no la combines todavía**.

## Resumen y próximos pasos

En este ejercicio:

- has aprendido qué es Model Context Protocol (MCP) y cómo amplía las herramientas de Copilot.
- has añadido el servidor MCP de Playwright al espacio de trabajo.
- lo has usado para probar los filtros en el navegador antes de publicar los cambios.
- has publicado la rama y abierto una solicitud de incorporación de cambios.

Has usado un servidor MCP para *probar* una funcionalidad, pero MCP solo es una de las formas de ampliar Copilot. A continuación, [crearás un agente personalizado][next-lesson] para agilizar tareas específicas, como las revisiones de accesibilidad.

## Recursos

- [Qué es MCP y por qué habla todo el mundo de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
