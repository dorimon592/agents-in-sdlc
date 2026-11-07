---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando usas GitHub Copilot en modo de agente, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tiempo real en tu equipo, Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición. Puedes revisar las diferencias en el editor, aceptar o rechazar cambios concretos y orientar la conversación con prompts para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás las ediciones propuestas.
- guiarás al agente durante la sesión para que añada un selector de tema claro junto al modo de alto contraste.

## Escenario

El agente personalizado de accesibilidad que usaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un selector de tema claro para quienes consideran que el tema predeterminado es demasiado oscuro. En lugar de iniciar otra conversación, guiarás la sesión existente para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a herramientas. Verás lecturas y ediciones de archivos y los comandos de terminal que solicita ejecutar.
3. Cuando el agente proponga una edición, aparecerán las diferencias:
    - Selecciona **Keep** para aceptar el cambio.
    - Selecciona **Undo** para revertirlo y pedir al agente que lo intente de nuevo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a herramientas solicita permiso, por ejemplo, para ejecutar un comando de terminal, el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que consideres seguros.

5. Espera a que el agente termine de implementar el modo de alto contraste. Al finalizar, mostrará un resumen de los cambios.

## Guía al agente para añadir el tema claro

Ahora que está implementado el modo de alto contraste, ampliarás la misma conversación para añadir un selector de tema claro. Como continuarás en la misma sesión, el agente conservará el contexto de su trabajo: los componentes que modificó, el enfoque de estilos y el patrón de almacenamiento local.

1. En el campo de Copilot Chat, envía este prompt:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará desde su trabajo anterior, revisará los cambios y propondrá cómo añadir el tema claro.
3. Revisa cada nueva edición a medida que aparezca, igual que hiciste con el modo de alto contraste. Usa **Keep** o **Undo** en cada diferencia.
4. Si el agente sigue una dirección que no quieres —por ejemplo, intenta reorganizar los selectores de una forma que no te convence—, detenlo con el botón **Stop** junto al campo de chat y envía un prompt para aclararlo.

> [!TIP]
> Guiar una sesión en curso es más rápido que empezar de nuevo. Envía prompts de seguimiento para pedir ajustes («simplifica el CSS: usa variables CSS en lugar de duplicar selectores»), corregir el rumbo («no añadas un tercer selector; integra el tema claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos selectores»).

## Revisa los cambios

Antes de confirmar el trabajo, revisa brevemente todo lo que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en componentes Astro, estilos y las pruebas relacionadas.
3. Abre algunos archivos modificados y revisa las diferencias. Confirma que se aplican las prácticas de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la próxima lección][next-lesson].

## Resumen y próximos pasos

Has usado el modo de agente de Copilot Chat para supervisar y guiar una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado prompts para ampliar el trabajo sin perder contexto.

En esta lección has:

- supervisado el trabajo de un agente en tiempo real.
- aceptado y rechazado ediciones de archivos individuales.
- guiado una sesión en curso con prompts de seguimiento.

En la [próxima lección][next-lesson], publicarás el trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios que ha generado el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
