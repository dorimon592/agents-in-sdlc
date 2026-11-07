---
title: "Lección 10 - Repaso y próximos pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y más recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI durante un flujo de trabajo completo de Tailspin Toys. Has:

- preparado un Codespace, instalado Copilot CLI, explorado el proyecto y localizado la incidencia de filtrado precargada.
- añadido valoraciones por estrellas, revisado el resultado en el navegador reenviado y combinado manualmente tu primera solicitud de incorporación de cambios (PR).
- partido de la incidencia de filtrado, definido el enfoque en modo Plan, implementado en modo Autopilot y revisado en modo Interactive.
- guiado al agente con instrucciones personalizadas, adaptado la habilidad existente `quality-checks` y utilizado pruebas unitarias, lint y comprobaciones de tipos.
- añadido el servidor Model Context Protocol (MCP) de Playwright y explorado los filtros en un navegador real.
- creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas del navegador.
- revisado el cambio de filtrado completo y autorizado Agent Merge para su PR.
- explorado comandos de barra para consultar el contexto y los modelos, compartir sesiones y delegar tareas opcionalmente en la nube.

## Qué has publicado

El taller incluye dos hitos de PR, cada uno en una rama independiente basada en una versión actualizada de `main`:

1. **Valoraciones por estrellas:** mostrar el valor existente de `starRating` y un estado explícito para los juegos sin valorar.
2. **Filtros y flujo de calidad:** implementar los filtros, actualizar y aplicar las instrucciones, personalizar el informe de `quality-checks`, crear el perfil de QA e incluir las pruebas asociadas.

Desde la planificación de los filtros hasta la creación de la PR, has usado la misma conversación y rama. Para simplificar el taller, todo ese trabajo se reunió en una sola PR.

## Diferentes formas de verificación

Has comprobado el código de varias maneras: mediante pruebas automatizadas, revisando el sitio en el navegador y dejando que Copilot lo explorara a través de MCP. La habilidad `quality-checks` ejecutó las pruebas unitarias, el lint y las comprobaciones de tipos, y comunicó los resultados con el nuevo formato. El agente de QA reunió esos resultados con la revisión de los requisitos y la cobertura antes de abrir la PR.

Las pruebas nuevas deben cubrir carencias reales; si una ejecución de QA concluye que no hacen falta pruebas, puede ser correcto. Revisa el código y las pruebas antes de autorizar la combinación y vuelve a generar las pruebas de verificación afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller has actualizado instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza y adapta estas personalizaciones a las necesidades del equipo. Las instrucciones establecen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas; no te limites al resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para definir el enfoque antes de implementarlo, **Interactive** para mantener el control en cambios específicos y **Autopilot** para tareas bien acotadas. Elige un modelo más rápido para ediciones rutinarias y uno más avanzado para trabajos complejos.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* influye de forma importante en el resultado.

## Más cosas por explorar

Has completado el flujo principal. Puedes probar otras funciones de CLI:

- `/review` para pedir al agente de revisión que analice los cambios.
- `/rubber-duck` para explicar un problema y recibir otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar otra tarea.
- `/delegate` para enviar una tarea al agente de Copilot en la nube.

## Próximos pasos

La mejor forma de mejorar con una herramienta es seguir usándola: en código de producción, proyectos personales o esa aplicación que llevas años pensando en crear. Comparte lo aprendido con el equipo y aprovecha sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
