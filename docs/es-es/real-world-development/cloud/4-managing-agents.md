---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente de Copilot en la nube que realice tres tareas distintas para mejorar la experiencia y añadir funcionalidad. Aunque trabaja de forma asíncrona y autónoma, sigue siendo importante supervisar estas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente, entre ellas la [página de agentes][agents-page] de GitHub.com. Desde este centro de operaciones puedes consultar todas las tareas con solicitudes de incorporación de cambios abiertas, revisar las acciones realizadas y guiar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- guiarás una sesión en curso para pedir funcionalidad adicional.

## Escenario

Después de asignar al agente la implementación de un modo de alto contraste, el equipo decidió que sería un buen momento para añadir también un modo claro. Como ya estaba trabajando en los estilos y en los selectores, puedes guiarlo para que añada el modo claro además del alto contraste.

## Revisa las tareas del agente en la nube de Copilot

Consulta el estado de las tareas asignadas al agente de Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Revisa la lista de tareas del panel principal y del panel izquierdo. Deberías encontrar las que has asignado a Copilot, entre ellas:
    - actualizar la documentación del código.
    - generar API para modificar juegos.
    - añadir un modo de alto contraste al sitio web.
3. Selecciona una tarea en curso y revisa las acciones que ha realizado Copilot. Pueden incluir:
    - obtener el código del repositorio.
    - crear el entorno de trabajo.
    - configurar servidores MCP.
    - realizar distintos pasos para completar la tarea.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Observa también el panel de solicitudes de incorporación de cambios, situado a la derecha. Desde allí puedes consultar la solicitud y los archivos modificados.

## Guía al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos hasta que la sesión indique que ha terminado de configurarse y ha empezado a trabajar. Verás mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la página de agentes, con el cuadro de diálogo para guiar la sesión resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma la solicitud y la incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot continuará con la tarea actualizada. Incorporará la solicitud nueva cuando termine el paso que estaba realizando al recibir el mensaje.

El proceso puede tardar varios minutos. Puedes aprovechar ese tiempo para repasar lo aprendido hasta ahora.

## Resumen y próximos pasos

Has explorado la página de agentes de Copilot, el centro de operaciones para supervisar y guiar tareas del agente en la nube. Puedes consultar tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para ajustar su alcance o añadir indicaciones.

Has aprendido a:

- usar Mission Control y la página de agentes para supervisar las tareas del agente.
- redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot completa las funciones de accesibilidad, continúa con la [revisión iterativa de sus solicitudes de incorporación de cambios][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
