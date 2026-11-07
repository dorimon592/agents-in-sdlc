---
title: "Ejercicio 2 - Añadir funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen implicar varios archivos y tareas, como ejecutar pruebas. El flujo habitual de desarrollo incluye localizar los archivos necesarios, hacer los cambios, ejecutar pruebas, depurar, descubrir qué archivo falta y volver a modificarlo. Y así sucesivamente.

El modo de agente de Copilot está pensado para este tipo de trabajo.

En el IDE, el modo de agente trabaja con mayor autonomía. Explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los problemas que encuentra. Veamos cómo usarlo para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE y permite ciclos de respuesta rápidos e interacción directa. El agente en la nube trabaja de forma asíncrona, como un compañero del equipo, y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- usar el modo de agente de Copilot para explorar el proyecto e identificar archivos pertinentes.
- usar GitHub Copilot en modo de agente para implementar funcionalidades en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados por Copilot antes de incorporarlos al código.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editorial. En el ejercicio anterior ya añadiste un helper de editoriales. Ahora completarás la capa de datos, la interfaz y las pruebas con el modo de agente de Copilot.

## Ejecuta el sitio web Tailspin Toys

Antes de hacer cambios, explora el sitio web Tailspin Toys para conocer su funcionalidad actual.

El sitio es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar juegos y sus detalles. Se trata de una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz en el cliente. Los helpers reutilizables para acceder a los datos están en `src/lib/`, y la interactividad se añade mediante un pequeño elemento `<script>` de Astro que utiliza las API DOM estándar.

### Inicia el sitio web

La tarea `npm run dev` inicia el único servidor de desarrollo de Astro. Para ejecutarlo en GitHub Codespaces:

1. Vuelve al codespace. Deberías estar en `main` después del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea y cambia a una rama para trabajar en los filtros:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo Astro esté listo, verás un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir `http://localhost:4321` en el navegador, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, seleccionar el enlace a la URL localhost del terminal redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado al codespace, que ahora aloja el servidor web.

### Explora el sitio web

Con el sitio en marcha, explora sus funciones principales:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como su título, descripción, editorial y categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso indicará cuál debes elegir.

La primera versión del sitio funciona, pero queremos añadir capacidades. Empieza por revisar el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente un backlog de incidencias. Pide a GitHub Copilot que te lo muestre.

1. En el selector de agentes de la vista Chat, elige **Agent**. El agente planifica e implementa cambios en varios archivos, ejecuta comandos de terminal y usa herramientas.

   ![Captura del selector de agentes de la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no recomiendan un modelo concreto. Al preparar el taller usamos Claude Sonnet 4.5 y lo indicamos para que las sugerencias sean relativamente uniformes y la experiencia resulte más predecible. Sin embargo, los LLM son probabilísticos y es normal que las sugerencias de Copilot difieran de los ejemplos.

> [!NOTE]
> Por su naturaleza probabilística, Copilot puede utilizar un comando MCP distinto y aun así completar la tarea.

3. Pide a Copilot que muestre el backlog con este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Copilot también habrá priorizado los elementos según lo que considera más útil para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidad al sitio.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa sus directrices, que incluyen:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones específicos para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Esto permite reutilizar contenido, dividir conjuntos grandes de instrucciones y proporcionar ejemplos y plantillas.

## Implementa los filtros

Para completar los filtros, tendrás que realizar al menos tres cambios independientes en la aplicación:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento de filtrado.
- actualizar la página de listado de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse correctamente antes de incorporar los cambios. El modo de agente de Copilot puede realizar estas tareas.

1. Puedes continuar la conversación actual con Copilot o seleccionar **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Captura que muestra cómo seleccionar el archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el selector de agentes de la vista Chat.

   ![Captura del selector de agentes de la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot explorará el proyecto para localizar los archivos pertinentes, como los helpers de la capa de datos, la interfaz y las pruebas. Después, modificará los archivos y ejecutará las pruebas.

   ![Captura de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot explorará el proyecto, modificará archivos y ejecutará pruebas. El proceso puede tardar varios minutos, según la complejidad de la tarea y del código. Mientras trabaja, puede que aparezcan los botones **Keep** y **Undo** en el editor. No tienes que seleccionarlos mientras el agente sigue trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un cuadro de diálogo de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas y que algunas pruebas fallen durante el proceso. Copilot itera entre la generación de código y las pruebas hasta terminar y dejar de detectar errores.

   ![Captura de una sesión completa de Copilot Agent Mode](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Revisa el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot y otras herramientas de IA.

10. Vuelve al navegador con el sitio en ejecución y explora la nueva funcionalidad.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y próximos pasos

Has usado el modo de agente de GitHub Copilot para añadir funcionalidades al sitio web Tailspin Toys. Has aprendido a:

- implementar funcionalidades en la interfaz y la capa de datos con el modo de agente de Copilot.
- explorar el proyecto e identificar archivos pertinentes para coordinar los cambios.
- revisar el código y las pruebas generados antes de incorporarlos.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio adicional: implementar paginación

A medida que aumente el número de juegos, será necesario paginar la lista. Usa lo aprendido para pedir a Copilot que implemente la paginación. Al diseñar la solución:

- sigue las prácticas recomendadas y utiliza los archivos de instrucciones existentes.
- decide cómo funcionará la paginación: por ejemplo, si el usuario podrá elegir el tamaño de página o si será fijo.
- incluye en el prompt las directrices necesarias para obtener la implementación que quieres.
- itera con GitHub Copilot mediante solicitudes de cambios y contexto adicional. Es el flujo de trabajo habitual con Copilot.

## Recursos

- [Copilot: modos ask, edit y agent; para qué sirven y cuándo usarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
