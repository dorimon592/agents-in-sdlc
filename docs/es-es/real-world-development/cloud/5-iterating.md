---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante el taller has pedido a GitHub Copilot varias tareas para mejorar la experiencia y añadir funcionalidad: documentar el código, crear una funcionalidad de juegos relacionados y añadir opciones de accesibilidad con modos de alto contraste y claro. Revisa los cambios y, si hace falta, proporciona comentarios para que Copilot los mejore.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Antes de activar la revisión para el resto del equipo, revisa las propuestas de Copilot para la documentación, los juegos relacionados y las funciones de accesibilidad.

## Seguridad y agente de GitHub Copilot en la nube

El agente trabaja de forma asíncrona y sin supervisión, por lo que se aplican algunas restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para el código.
- El agente se ejecuta en GitHub Actions, dentro de un entorno independiente y efímero.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que se ejecuten.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empieza por la primera solicitud de incorporación de cambios (PR) que generó el agente: la documentación del código. La revisarás desde la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Es posible que veas una advertencia de que Copilot está bloqueado por un firewall. Es lo esperado: el acceso del agente a recursos externos, incluidas las llamadas a servidores MCP externos, está limitado de forma predeterminada. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la solicitud cuyo título sea parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la solicitud tendrá la etiqueta **[WIP]**. Espera a que termine. Puede tardar unos minutos; puedes descansar o repasar lo aprendido.

4. Cuando la solicitud esté lista, selecciona **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye nuevos comentarios TSDoc y otra documentación. Los cambios concretos pueden variar. Comprueba los comentarios TSDoc, las convenciones de TypeScript y los encabezados de comentario. Proceden de los archivos de instrucciones que revisaste al principio del ejercicio 2.
6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. Puede tardar varios minutos.

## Solicita cambios a GitHub Copilot

Trabajar con Copilot en una solicitud no es un proceso unidireccional. Puedes mencionarlo en comentarios de la solicitud o en comentarios en línea sobre el código. Copilot los verá e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no hay un prompt único que debas usar. Por ejemplo, puedes pedirle:

- que añada encabezados de comentario con una breve descripción al principio de cada archivo de código.
- que añada comentarios TSDoc a archivos TypeScript y Astro.
- que cree un archivo README con una descripción de la estructura de la aplicación Astro.

1. Añade un comentario y menciona a **@copilot** como harías con otra persona. Usa una de las ideas anteriores o pide otro cambio de documentación.
2. Selecciona **View Session** para observar cómo Copilot inicia una sesión nueva y aplica los cambios.
3. Selecciona **Back to pull request** para volver a la solicitud.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, debería aparecer una confirmación nueva en la solicitud.
5. Selecciona **Files changed** y revisa los cambios.

Itera hasta que el resultado te satisfaga. Cuando esté listo, convierte la solicitud en borrador en una solicitud lista para revisión y combínala.

![Convertir la solicitud en lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Vuelve a la solicitud que Copilot generó para mostrar juegos relacionados en la página de detalles.

1. Ve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la solicitud cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona **Files changed** y revisa el código generado.
5. Cuando hayas terminado, vuelve a **Conversation** y desplázate hacia abajo.
6. Comprueba si hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Confirma que las comprobaciones del proyecto para la aplicación Astro se superen; puede tardar varios minutos.
9. **Opcional:** cambia a la rama de Copilot desde tu codespace para probar manualmente la funcionalidad. En el terminal, ejecuta estos comandos y sustituye `<branch-name>` por la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la funcionalidad de juegos relacionados. Puedes iterar y pedir ajustes, por ejemplo, cambiar cuántos juegos se muestran o añadir encabezados de comentario y comentarios TSDoc (recuerda que la incidencia se asignó **antes** de añadir las instrucciones nuevas). Añade un comentario en la pestaña **Conversation** para iniciar otra sesión.

## Revisa las funciones de accesibilidad

Por último, revisa las funciones implementadas con el agente personalizado de accesibilidad. La solicitud debe incluir el modo de alto contraste del ejercicio 3 y el modo claro que pediste en Mission Control en el ejercicio 4.

1. Ve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Abre la solicitud cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, la solicitud tendrá la etiqueta **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** y revisa el código generado.
5. Presta atención a:
   - los componentes de interfaz para cambiar de modo.
   - el uso del almacenamiento local para conservar las preferencias.
   - los cambios CSS de los modos de alto contraste y claro.
   - los atributos de accesibilidad, como etiquetas ARIA y navegación con teclado.
   - el código JavaScript o TypeScript que gestiona los modos.

6. Cuando hayas terminado, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Comprueba si hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Confirma que las comprobaciones del proyecto para la aplicación Astro se superen.
10. **Opcional:** cambia a la rama de Copilot en el codespace para probar las funciones de accesibilidad. Abre el terminal y ejecuta estos comandos; sustituye `<branch-name>` por la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    A continuación, inicia la aplicación y prueba los selectores de alto contraste y tema claro en el navegador. Comprueba que funcionan y que las preferencias se conservan al recargar la página.

El agente personalizado de accesibilidad ha ayudado a Copilot a implementar estas funciones siguiendo prácticas de accesibilidad. Si detectas problemas o mejoras posibles, menciona a **@copilot** en un comentario para pedir cambios.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil para delegar tareas reales del backlog y ocuparte de otra cosa mientras trabaja. Para practicar, crea más incidencias y asígnaselas a Copilot. Algunas ideas:

- crear un formulario de interés para las personas patrocinadoras en la página de detalles del juego.
- implementar la paginación en la página de la lista de juegos.
- añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el itinerario del agente en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizados del repositorio** para reconocer su efecto en el trabajo del agente.
- **Has asignado incidencias al agente de Copilot en la nube** y has visto cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** para implementar modos de alto contraste y claro con atención a la accesibilidad.
- **Has usado la página de agentes de Copilot como centro de operaciones** para supervisar y guiar la sesión de accesibilidad.
- **Has revisado e iterado sobre las solicitudes de Copilot**, mencionándolo con `@copilot` para pedir cambios y aprobando flujos de trabajo.

## Repaso y próximos pasos

Has terminado el itinerario del agente en la nube. Para seguir explorando, puedes probar otros entornos:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración de MCP desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y el puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente una incidencia de refactorización o cobertura de pruebas.
- crea un agente personalizado para otro ámbito.
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente de Copilot en la nube][agent-firewall]

---

| [← Lección anterior: Supervisar y gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
