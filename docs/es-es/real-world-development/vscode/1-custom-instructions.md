---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es esencial al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información de fondo, asegúrate de que tenga acceso a ella. Los [archivos de instrucciones][instruction-files] proporcionan directrices no solo sobre *qué* código quieres, sino también sobre *cómo* debe estructurarse.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las pautas de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones aplicables a rutas concretas.
- generarás la primera parte de la capa de datos para los filtros (un helper de editoriales) con las instrucciones actuales.
- añadirás un estándar general para el repositorio en `.github/copilot-instructions.md`.
- volverás a ejecutar el mismo prompt y observarás cómo el código generado sigue el nuevo estándar.
- confirmarás los cambios de instrucciones y filtrado en `main` para que Copilot pueda usar las directrices actualizadas en el próximo ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares que hayas definido. Copilot no es determinista. El objetivo es observar la tendencia del cambio después de actualizar las instrucciones, no obtener exactamente el mismo resultado carácter por carácter.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo, Tailspin Toys tiene pautas y requisitos para sus prácticas de programación. Entre ellos:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe usar el modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que describa su función.

Usarás archivos de instrucciones para proporcionar a Copilot la información necesaria para que realice las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que entienda mejor el estilo de programación y los requisitos. Permiten orientar sus sugerencias con convenciones de programación, bibliotecas y tipos de comentarios preferidos. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información de contexto relevante para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción general del proyecto, las prácticas recomendadas y otras directrices globales.
- Los archivos `.github/instructions/*.instructions.md` contienen directrices para lenguajes, tipos de archivo o tareas concretos.

> [!NOTE]
> En el IDE, los archivos de instrucciones se utilizan para generar código en Copilot Chat, pero no para completar código ni para las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que incluyen frontmatter `applyTo`.
>
> Copilot también [admite otros formatos de instrucciones][custom-instructions-support], como los archivos AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

La conversación completa sobre cómo crear archivos de instrucciones queda fuera del alcance del taller. Los ejemplos del proyecto muestran un enfoque representativo. A grandes rasgos:

- Limita `copilot-instructions.md` a directrices generales del proyecto, como su descripción, estructura y estándares globales de programación.
- Usa `*.instructions.md` para tipos de archivos (pruebas unitarias, componentes Astro o la capa de datos) o tareas concretas.
- Escribe las instrucciones en lenguaje natural y con claridad. Incluye ejemplos de cómo debe y no debe ser el código.

No hay una única forma de crear archivos de instrucciones, como tampoco hay una sola forma de usar la IA. La experimentación te ayudará a encontrar la que mejor se adapte al proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería tener una colección sólida de archivos de instrucciones. Al explorar los del proyecto, puede que veas instrucciones para distintos tipos de tareas, como [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece una opción distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección correspondiente lo indicará cuando sea pertinente.
>
> Si buscas plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

El proyecto inicial ya incluye un archivo `copilot-instructions.md` principal y varios archivos `*.instructions.md`. Examínalos antes de hacer cambios para observar el efecto de sus directrices.

1. Abre `.github/copilot-instructions.md`.
2. Revisa la descripción breve del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en las pautas anidadas de **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Explora la carpeta `.github/instructions`. Incluye instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` del principio define un patrón global que determina a qué archivos se aplica. En este caso, coincide con cualquier archivo de prueba TypeScript, por ejemplo, los que sigan el patrón `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y consulta al final los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar a Copilot ejemplos que puede seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` recoge los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás las reglas para los comentarios TSDoc y los encabezados de comentario de los archivos.

## Crea una rama para los cambios

Sigue las prácticas recomendadas y crea una rama para los cambios.

1. Vuelve al codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat antes de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de continuar, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo dependen del ejercicio; cada paso indicará cuál elegir.

Para observar el efecto de las instrucciones personalizadas, empieza enviando un prompt con el archivo actual. Más adelante, lo actualizarás y volverás a enviar el mismo prompt para comparar los resultados.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot solo tenga el contexto necesario.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse el helper.
3. En el selector de agentes de la vista Chat, elige **Agent** para que Copilot pueda modificar archivos.

   ![Captura del selector de agentes de la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios, normalmente en el archivo helper y en sus pruebas.
6. Observa que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editoriales. Esto se debe a las convenciones de la capa de datos en `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** los comentarios TSDoc y el encabezado de comentario del archivo.

> [!CAUTION]
> Como Copilot no es determinista, puede que añada comentarios aunque no se lo hayas pedido. No pasa nada; el objetivo es observar la mejora de coherencia tras actualizar las instrucciones.

## Añade un estándar general para el repositorio

`.github/copilot-instructions.md` proporciona contexto general a Copilot. Actualiza los estándares de programación del repositorio para orientar mejor sus sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares de programación, pero todavía no incluye reglas para la documentación en el código.
3. Añade estas líneas Markdown justo después de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, las instrucciones del repositorio (`.github/copilot-instructions.md`) son adecuadas para estándares generales, mientras que los archivos `*.instructions.md` sirven para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar indicado para reglas generales, como la de los comentarios de documentación.

## Vuelve a enviar el prompt y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para borrar el contexto anterior.
2. Selecciona de nuevo `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía **el mismo prompt**:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que el archivo propuesto ahora comienza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Helpers de acceso a datos de editoriales para la plataforma de financiación colectiva de Tailspin Toys.
    * Proporciona funciones para recuperar información de editoriales de la base de datos.
    */
   ```

5. Observa que ahora la función incluye un comentario TSDoc similar al siguiente:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su id y nombre.
    *
    * @param db - Cliente de base de datos Drizzle.
    * @returns Promesa que se resuelve en una matriz de objetos de editorial.
    */
   ```

Has orientado a Copilot para que siga un estándar nuevo y lo aplique a código real que servirá de base para el próximo ejercicio.

## Confirma las instrucciones y publica la rama

Los archivos de instrucciones son recursos del repositorio y se gestionan con el mismo flujo de control de versiones que los demás archivos.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Comprueba que están presentes las instrucciones y los cambios del helper:

   ```bash
   git status
   ```

3. Prepara y confirma los cambios del archivo de instrucciones y todos los archivos modificados por Copilot para crear la base del helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados, como pruebas, añádelos también.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama en el repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y combina una solicitud de incorporación de cambios

Con la rama publicada, crea una solicitud de incorporación de cambios y vincúlala a la incidencia del backlog sobre la actualización de las instrucciones. Copilot puede buscar la incidencia, crear la solicitud y asociarla con ella mediante las herramientas de GitHub que ya están conectadas al proyecto.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actúa en GitHub en tu nombre. La plantilla del proyecto ya conecta las herramientas de GitHub con el espacio de trabajo, así que no hace falta configurarlas. Puede que VS Code te pida iniciar sesión la primera vez que Copilot use una herramienta. Sigue las instrucciones para permitirlo. En un ejercicio posterior verás cómo funciona esta conexión mediante Model Context Protocol.

1. Abre Copilot Chat en el codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que **Agent** está seleccionado.
3. Pide a Copilot que encuentre la incidencia sobre la actualización de los archivos de instrucciones y cree una solicitud desde la rama actual. Indícale que incluya tanto los cambios de instrucciones como la base del helper de editoriales:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot buscará la incidencia y creará la solicitud.
5. Si Copilot te pide **Allow** para realizar acciones de GitHub, revisa el comando y selecciona **Allow** cuando corresponda.
6. Cuando se cree la solicitud, pide a Copilot que la combine y vuelva a la rama `main` local:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Si Copilot solicita **Allow** para realizar acciones de GitHub y ejecutar comandos de shell, revisa el comando y selecciona **Allow** cuando corresponda.

¡Ya has creado y combinado una solicitud de incorporación de cambios con ayuda de GitHub Copilot!

## Resumen y próximos pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones existentes.
- añadir un estándar general al archivo `.github/copilot-instructions.md`.
- volver a enviar el prompt y observar cómo el código propuesto sigue el nuevo estándar.
- confirmar en `main` los cambios de instrucciones y la base del helper para el próximo ejercicio.

A continuación, aplicarás esas instrucciones en el [modo de agente][next-lesson] mientras Copilot añade una funcionalidad al código.

## Recursos

- [Añadir instrucciones personalizadas para GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
