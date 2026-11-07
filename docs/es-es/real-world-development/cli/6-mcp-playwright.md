---
title: "Lección 6 - Validar la funcionalidad con MCP de Playwright"
description: "Configura o revisa MCP de Playwright en Copilot CLI y úsalo para explorar en el navegador la funcionalidad de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos visto, programar no consiste solo en escribir código. También hay que trabajar con datos y servicios externos y proporcionar a Copilot automatizaciones adicionales. Para ello sirven los servidores MCP: permiten ampliar las herramientas integradas en la CLI.

En esta lección:

- aprenderás qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI.
- añadirás el servidor MCP de Playwright si todavía no está disponible.
- pedirás al agente que controle un navegador y explore los filtros.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar cambios en la interfaz hay que interactuar con ella. Quieres que Copilot use el sitio como lo haría una persona y automatice más comprobaciones.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada y realizar acciones en tu nombre.

Los servidores MCP actúan como puente entre el agente y las herramientas y los servicios. Cada servidor proporciona un conjunto distinto de herramientas y recursos.

Algunos servidores MCP populares:

- **[GitHub MCP Server][github-mcp]**: da acceso a las API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios.
- **[Playwright MCP Server][playwright-mcp-server]**: proporciona automatización del navegador mediante Playwright.

Hay muchos otros servidores MCP. GitHub mantiene un [registro MCP][mcp-registry] para facilitar el descubrimiento y la colaboración.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y ten en cuenta las implicaciones de seguridad.

## Añade el servidor MCP de Playwright

Si el servidor MCP de Playwright todavía no está disponible, añádelo para que Copilot pueda interactuar con el sitio.

> [!NOTE]
> Si ya está configurado en Copilot CLI, omite estos pasos y continúa en la sección siguiente.

1. Vuelve al Codespace.
2. En Copilot CLI, ejecuta este comando para abrir el cuadro de diálogo de servidor MCP:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo de servidor **STDIO** con <kbd>Enter</kbd> y pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo MCP.

## Pide a Copilot que explore la funcionalidad con Playwright

Ya has comprobado manualmente que los filtros funcionan. Ahora pídele a Copilot que haga lo mismo mediante el servidor MCP de Playwright.

1. Envía este prompt:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Normalmente, no hace falta indicar a Copilot que use el servidor MCP; suele elegirlo automáticamente. Sin embargo, especificarlo ayuda a que el agente lo use de forma coherente y puede ahorrar tokens.

2. Observa los pasos que realiza en el navegador para comprobar la funcionalidad.
3. Lee el informe y confirma que todo funciona según lo esperado.

Copilot iniciará el servidor, interactuará con el sitio mediante Playwright, detendrá el servidor y te mostrará un informe.

## Resumen y próximos pasos

¡Has usado el servidor MCP de Playwright para explorar la funcionalidad en un navegador real desde Copilot CLI! En esta lección:

- has aprendido qué es MCP y cómo lo usa Copilot CLI.
- has añadido el servidor MCP de Playwright si no estaba disponible.
- has pedido al agente que controle un navegador y pruebe los filtros.

A continuación, crearás un [agente personalizado de QA][next-lesson] que combinará la habilidad y las herramientas del navegador.

## Recursos

- [Qué es MCP y por qué habla todo el mundo de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
