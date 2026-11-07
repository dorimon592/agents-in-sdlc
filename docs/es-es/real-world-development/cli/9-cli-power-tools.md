---
title: "Lección 9 - Comandos de barra de GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, elegir modelos, compartir sesiones y delegar tareas opcionalmente al agente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como muchas herramientas de CLI, GitHub Copilot CLI incluye comandos de barra para interactuar con la aplicación. Permiten acceder a funciones avanzadas, consultar información interna y cambiar la configuración. Ya has utilizado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Ahora explorarás otros útiles.

En esta lección:

- usarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación.
- usarás `/model` para ver los modelos disponibles.
- aprenderás a exportar o compartir una sesión con `/share`.
- explorarás comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

## Escenario

Has terminado el flujo principal del taller. Ahora revisarás otras posibilidades: gestionar el contexto, cambiar de modelo, compartir sesiones y, si quieres, delegar trabajo al [agente de Copilot en la nube][about-cloud-agent].

## Explora el contexto de Copilot CLI

En tareas grandes o complejas, puede que alcances el límite de contexto del modelo. Copilot CLI lo compacta automáticamente cuando hace falta, pero también puedes consultarlo o compactarlo con comandos de barra.

1. Vuelve al Codespace e inicia Copilot CLI desde la raíz del repositorio si no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y la distribución del contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio disponible.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a ejecutar `/context` y compara los resultados. Si la conversación ya es pequeña, puede que apenas haya cambiado.

> [!NOTE]
> Copilot CLI compacta el contexto automáticamente a medida que se llena la ventana. Usa `/compact` para decidir cuándo hacerlo. Usa `/clear` o `/new` al cambiar a una tarea independiente y querer iniciar una conversación nueva.

## Elige el modelo

Los modelos tienen distintas fortalezas y las personas desarrolladoras pueden tener preferencias diferentes. Copilot CLI permite consultar y elegir el modelo.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información de uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Compartir el trabajo y los aprendizajes permite que el equipo mejore el uso de las herramientas de IA. El comando `/share` puede exportar una sesión a Markdown o HTML, crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` disponibles en la versión instalada.
2. Si quieres compartir la sesión, elige el destino adecuado; por ejemplo, `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo o publicarlo. Las exportaciones pueden incluir prompts, respuestas y detalles del proyecto.

Compartir un enlace o un gist es opcional. No publiques contenido del repositorio o de una conversación que el equipo no tenga previsto compartir.

## Opcional: ampliar o delegar el trabajo

El flujo principal del taller ha terminado. Copilot CLI también tiene comandos para tareas más amplias:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo aislado para otra tarea.
- `/delegate` puede enviar una tarea al agente de Copilot en la nube, que trabaja de forma asíncrona y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales, ya que pueden crear árboles de trabajo adicionales o iniciar tareas remotas. Antes de probarlos, inicia una tarea nueva con un alcance claro y revisa los resultados con el flujo habitual. Para aprender más sobre los agentes asíncronos, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y próximos pasos

Los comandos de barra de Copilot CLI permiten configurarlo, compartir sesiones y consultar lo que ocurre entre bastidores. En esta lección:

- has usado `/context` y `/compact` para explorar la gestión del contexto.
- has usado `/model` para consultar los modelos disponibles.
- has aprendido cómo `/share` puede exportar o compartir una sesión.
- has explorado comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas al agente en la nube.

Copilot CLI incluye muchos otros comandos de barra. Para terminar el recorrido, [repasarás lo aprendido][next-lesson] y verás los próximos pasos.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente de Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
