---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de Copilot en la nube →][next-lesson]

El contexto es esencial al trabajar con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita información de fondo, asegúrate de que tenga acceso a ella. Los [archivos de instrucciones][instruction-files] proporcionan directrices no solo sobre *qué* quieres, sino también sobre *cómo* debe hacerse.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las pautas de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos de instrucciones aplicables a rutas concretas.
- añadirás un estándar general al archivo `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los itinerarios de VS Code y CLI, aquí no se compara el comportamiento con y sin instrucciones. El agente de Copilot en la nube trabaja de forma asíncrona con incidencias de GitHub, por lo que es más difícil mostrar el efecto en tiempo real. Verás la influencia del archivo de instrucciones cuando revises las solicitudes de incorporación de cambios que genere más adelante.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene pautas y requisitos para sus prácticas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe usar el modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que describa su función.

Usarás archivos de instrucciones para proporcionar a Copilot el contexto necesario para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que entienda mejor el estilo de programación y los requisitos. Permiten orientar sus sugerencias con convenciones de programación, bibliotecas y tipos de comentarios preferidos. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información relevante para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, una descripción general del proyecto, las prácticas recomendadas y otras directrices globales.
- Los archivos `.github/instructions/*.instructions.md` contienen directrices para lenguajes, tipos de archivo o tareas concretos.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se utilizan para generar código en Copilot Chat, no para completar código ni para las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube utilizan tanto las instrucciones del repositorio como los archivos `*.instructions.md` que incluyen frontmatter `applyTo`.
>
> Copilot también [admite otros formatos de instrucciones][custom-instructions-support], como los archivos AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Los ejemplos del proyecto muestran un enfoque representativo. A grandes rasgos:

- Limita `copilot-instructions.md` a directrices generales del proyecto, como su descripción, estructura y estándares globales de programación.
- Usa `*.instructions.md` para tipos de archivos (pruebas unitarias, componentes Astro o la capa de datos) o tareas concretas.
- Escribe instrucciones claras en lenguaje natural e incluye ejemplos de cómo debe y no debe ser el código.

No hay una única forma de crear archivos de instrucciones ni de usar IA. La experimentación te ayudará a encontrar la que mejor se adapte al proyecto.

> [!TIP]
> Todo proyecto que utilice GitHub Copilot debería tener una colección sólida de archivos de instrucciones. Puede que veas instrucciones para distintos tipos de tareas, como [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece una opción distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI.
>
> Para ver plantillas y ejemplos, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

El repositorio incluye un archivo `copilot-instructions.md` principal y varios archivos `*.instructions.md`. Examínalos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Revisa la descripción breve del proyecto y secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las directrices anidadas de **GitHub Actions Workflows**. Se aplican a cualquier interacción con Copilot.
3. Explora la carpeta `.github/instructions`. Incluye instrucciones para archivos Astro, la capa de datos Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. Su campo `applyTo` define un patrón global que determina a qué archivos se aplica. En este caso, coincide con cualquier archivo de prueba TypeScript, por ejemplo, los que sigan `**/*.test.ts`.
5. Revisa las instrucciones específicas para crear pruebas unitarias.
6. Abre `.github/instructions/drizzle.instructions.md` y consulta al final los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos grandes de instrucciones en archivos más pequeños y reutilizables, y señalar ejemplos que Copilot pueda seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` recoge los estándares del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los encabezados de comentario.

## Añade un estándar general para el repositorio

El cambio que harás aquí consiste en indicar que la documentación debe incluirse en el código mediante comentarios TSDoc y un encabezado de comentario para cada archivo. El agente lo aplicará al trabajar en las incidencias que le asignes más adelante.

Antes de editar, crea una rama. El agente leerá las instrucciones de la rama a la que se dirija la incidencia, pero también confirmarás los cambios:

1. En el terminal del codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como se explicó antes, `.github/copilot-instructions.md` proporciona información general del proyecto a Copilot.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares del proyecto, pero todavía no incluye reglas para la documentación en el código.
3. Añade estas líneas Markdown justo después de los estándares existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Las instrucciones del repositorio (`.github/copilot-instructions.md`) son adecuadas para estándares generales; los archivos `*.instructions.md` sirven para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar indicado para reglas generales, como la de los comentarios de documentación.

## Confirma, publica y combina la actualización de instrucciones

El agente de Copilot en la nube lee las instrucciones de la rama a la que apunta la incidencia. En el próximo ejercicio, Copilot partirá de `main`, así que tus cambios deben llegar a `main` para que el agente pueda utilizarlos.

1. Prepara y confirma el cambio:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios desde `custom-instructions-cloud` a `main` y combínala. Así, el agente podrá leer estas instrucciones cuando trabaje en las incidencias del próximo ejercicio.

> [!TIP]
> Si prefieres usar `main` directamente durante el taller, puedes omitir los pasos de la rama y confirmar los cambios directamente en `main`. La rama sirve para practicar un flujo de trabajo habitual en un proyecto real.

## Resumen y próximos pasos

Has explorado cómo Copilot utiliza los archivos de instrucciones del proyecto y has añadido un estándar general a `.github/copilot-instructions.md`. Verás cómo se aplica en las solicitudes de incorporación de cambios que genere el agente durante el resto del itinerario.

A continuación, [asignarás tu primera incidencia al agente de Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
