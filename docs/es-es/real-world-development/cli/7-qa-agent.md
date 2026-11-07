---
title: "Lección 7 - Crear y usar un agente de QA"
description: "Crea un agente de QA personalizado que combine los requisitos de la incidencia, la habilidad quality-checks y MCP de Playwright."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y MCP de Playwright para observar los filtros en un navegador. Ahora reunirás esas capacidades en un agente personalizado que siga un proceso de QA bien definido.

En esta lección:

- explorarás cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- crearás y revisarás un perfil reutilizable de control de calidad.
- seleccionarás el agente de QA y revisarás sus conclusiones respecto a la incidencia de filtrado.

## Escenario

Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador antes de abrir una solicitud de incorporación de cambios (PR). Un agente personalizado puede coordinar el proceso y crear un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil Markdown. El perfil describe su propósito, instrucciones y herramientas disponibles. En este taller, crearás el rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Cada personalización cumple una función distinta. Las instrucciones del repositorio describen los estándares del equipo. La habilidad `quality-checks` agrupa comprobaciones repetibles. MCP de Playwright proporciona herramientas para el navegador. El perfil de QA indica a Copilot cómo utilizar esas capacidades para evaluar los requisitos y comunicar los resultados. El perfil no reemplaza a las demás personalizaciones ni requiere otra conversación.

## Crea el perfil de QA

Antes de abrir la PR de la funcionalidad, pide a Copilot que cree un perfil de QA reutilizable. Especificará las comprobaciones y los límites del agente.

1. Vuelve al Codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Revisa el perfil

Antes de usar el agente, revisa su perfil para confirmar que incluye el proceso y los límites que has indicado. Así evitarás que modifique la funcionalidad cuando solo querías comprobarla.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, pero proporciona un nombre visible al agente.
3. Revisa las instrucciones. Confirma que el agente empieza por los requisitos, sigue las instrucciones del repositorio, ejecuta la habilidad `quality-checks` y usa MCP de Playwright.
4. Confirma que el informe incluye pruebas, que el agente pregunta antes de modificar el código y que no confirma cambios ni abre solicitudes de incorporación de cambios.
5. Si falta alguna responsabilidad o límite, pide al agente predeterminado que lo corrija antes de continuar.

## Ejecuta QA respecto a la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Reanuda la conversación de filtrado y selecciona QA para que pueda usar la incidencia y las decisiones tomadas en la planificación.

1. Activa el agente con este prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, quizá no aparezca en la lista de agentes disponibles. El comando anterior lo activará igualmente.

2. Pide al agente de QA que revise la funcionalidad:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. El agente de QA empezará a trabajar.
4. Cuando termine, lee el informe.

## Resumen y próximos pasos

Has añadido un rol especializado reutilizable al flujo de trabajo y has revisado sus resultados. En esta lección:

- has explorado cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- has creado y revisado un perfil reutilizable de QA.
- has seleccionado el agente de QA y revisado sus conclusiones respecto a la incidencia.

Ya tienes la implementación, la actualización de la habilidad, el perfil de QA, las pruebas y el informe de verificación listos para revisión. A continuación, [reunirás todo en una PR de la funcionalidad y usarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
