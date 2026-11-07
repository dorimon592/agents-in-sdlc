---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa los filtros, las instrucciones, la habilidad, el perfil de QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación de los filtros, las instrucciones actualizadas, la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Ahora revisarás todos los cambios juntos y abrirás una solicitud de incorporación de cambios. Combinaste manualmente la PR de valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separarías la funcionalidad, las instrucciones, la habilidad y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el trabajo de filtrado y calidad en una conversación y una rama.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- revisarás el cambio completo de la funcionalidad y las pruebas de verificación.
- crearás la PR de filtrado.
- activarás Agent Merge solo después de revisar los cambios y confirmarás que se ha combinado la PR.

## Escenario

Has planificado, implementado y comprobado una funcionalidad con Copilot. Tailspin Toys quiere automatizar las tareas restantes de la PR, pero mantener bajo tu control la autorización para combinarla.

## Introducción a Agent Merge

**Agent Merge** automatiza las tareas restantes para incorporar una solicitud de incorporación de cambios. Al activarlo, Copilot resuelve los bloqueos: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y reorganiza la rama cuando es necesario. Después, habilita la combinación automática de GitHub si la configuración del repositorio lo permite.

Hasta ahora has seleccionado manualmente **Merge pull request**. Agent Merge puede encargarse de ese proceso. Revisa el trabajo y confirma que está listo antes de activarlo.

## Usa Agent Merge para gestionar la PR

Con el código listo, revísalo, crea la PR y permite que Agent Merge gestione las tareas restantes.

1. Vuelve al Codespace.
2. Escribe `/agent` para abrir el cuadro de diálogo de agentes.
3. Selecciona **Default** y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará la integración continua de la PR y la combinará cuando todo se haya completado correctamente.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, revisa el motivo y, cuando el repositorio lo permita, completa manualmente la combinación después de revisar el trabajo.

## Resumen y próximos pasos

Has automatizado varias partes del desarrollo: la generación y validación del código, las pruebas y ahora el proceso de la solicitud. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- has revisado el cambio completo de la funcionalidad y las pruebas de verificación.
- has creado la PR de filtrado.
- has activado Agent Merge después de revisar los cambios y has confirmado la combinación.

A continuación, [explorarás comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, compartir sesiones y delegar tareas opcionalmente al agente en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
