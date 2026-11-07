---
title: "Lección 1 - Instalar la aplicación GitHub Copilot"
description: "Instala la aplicación GitHub Copilot, conecta el repositorio que has creado a partir de la plantilla, conoce el espacio de trabajo y prueba un chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La **[aplicación GitHub Copilot][about-copilot-app]** es una aplicación de escritorio para el desarrollo dirigido por agentes. Se basa en GitHub Copilot CLI y se integra de forma nativa con GitHub, por lo que los repositorios, las ramas y las canalizaciones de CI funcionan sin configuración adicional. Está diseñada para flujos de trabajo en los que diriges varios agentes en paralelo, cada uno en su propio espacio de trabajo aislado, en lugar de realizar todo el trabajo por tu cuenta, y para automatizar tareas repetitivas. Como ya tienes Node.js instalado y una copia del proyecto preparada, el siguiente paso es instalar la aplicación y conectar el repositorio.

En esta lección:

- instalarás la aplicación GitHub Copilot e iniciarás sesión.
- añadirás el proyecto a la aplicación desde su repositorio de GitHub.
- conocerás el espacio de trabajo, incluido el backlog precargado por la plantilla.
- probarás un chat rápido para conocer la propia aplicación.

## Escenario

Tu equipo está empezando a usar agentes de IA para abordar un backlog cada vez mayor. La aplicación Copilot permite dirigir ese trabajo desde un mismo lugar: seleccionar incidencias, ejecutar agentes, revisar cambios y combinar solicitudes de incorporación de cambios. En esta lección instalarás y conectarás la aplicación y aprenderás a iniciar una conversación sobre el proyecto.

> [!NOTE]
> Necesitas un plan de Copilot válido: Copilot Free, Copilot Student o cualquier plan de pago (Pro, Pro+, Business o Enterprise). Si utilizas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva **GitHub Copilot app**.

## Instala y configura la aplicación GitHub Copilot

El primer paso para utilizar la aplicación GitHub Copilot es instalarla. Hay versiones para Windows, macOS y Linux. Instala la aplicación, autentícate y añade el repositorio Tailspin Toys.

1. En un navegador, abre la [página de inicio de la aplicación GitHub Copilot][download-app].
2. Descarga la aplicación para tu plataforma e instálala siguiendo las instrucciones de la página.
3. Abre la aplicación.
4. Selecciona **Sign in to GitHub** y sigue las instrucciones para autenticarte. Si utilizas GitHub Enterprise Cloud con residencia de datos, selecciona **Use GitHub Enterprise** e introduce el nombre de host `*.ghe.com` cuando se te solicite. GitHub Enterprise Server no es compatible.
5. Si durante la configuración se te ofrece elegir un repositorio, selecciona el repositorio Tailspin Toys que has creado a partir de la plantilla. También puedes omitir este paso y añadir el proyecto al terminar la configuración.
6. Cuando se te solicite, elige un tema y termina el proceso de configuración.
7. Busca el repositorio en **Sessions**, en la barra lateral. Si ya aparece, no tienes que volver a añadirlo.
8. Si todavía no está conectado, selecciona **+** junto a **Sessions** para crear una sesión y, después, selecciona **Add GitHub repository**. Usa el propietario y el nombre del repositorio que anotaste en la lección de requisitos previos.
9. Comprueba que el repositorio aparece en la barra lateral. En el diseño de este taller, las carpetas de repositorio, como `tailspin-toys`, aparecen agrupadas en **Sessions**, con sus sesiones debajo.

## Conoce el espacio de trabajo

Con el proyecto conectado, familiarízate con las áreas de la barra lateral:

- **New**: inicia una sesión de chat con Copilot.
- **Pull requests**: muestra las solicitudes de incorporación de cambios mediante la integración nativa con GitHub. Desde aquí puedes explorarlas y filtrarlas, comprobar el estado de CI y revisarlas sin salir de la aplicación.
- **Issues**: muestra las incidencias del mismo modo. Puedes explorarlas y filtrarlas e iniciar una sesión a partir de una incidencia.
- **Automations**: tareas de agente guardadas que se ejecutan según una programación o bajo demanda. Sirven, por ejemplo, para gestionar listas de tareas, realizar el mantenimiento periódico del proyecto o delegar otras tareas tediosas. El repaso final las menciona como próximo paso, no como otro ejercicio del taller.
- **Customize**: añade funciones a la aplicación Copilot mediante servidores MCP, plugins, habilidades y otros componentes. La usarás para configurar MCP de Playwright.
- **Chats**: conversaciones breves para preguntas y lluvias de ideas que no necesitan una rama ni un espacio de trabajo propios. Esta sección aparece después de iniciar el primer chat. Si no está visible, selecciona **+** junto a **Sessions** para empezar un chat.
- **Sessions**: agrupa las carpetas del repositorio y las sesiones de agente. Selecciona un repositorio, como `tailspin-toys`, para abrir su vista, o una sesión para volver a la conversación. Cada sesión se ejecuta en su propio espacio de trabajo aislado, por lo que puedes ejecutar varias sin que sus cambios entren en conflicto. Iniciarás la primera sesión al añadir las valoraciones por estrellas.

Explorarás el espacio de trabajo durante el taller.

> [!TIP]
> Si tienes dudas, pregunta a Copilot. Si no sabes cómo hacer algo o si es posible, puede ayudarte a orientarte.

### Localiza el backlog precargado

Prácticamente todos los proyectos tienen trabajo pendiente, y Tailspin Toys no es una excepción. Explora el backlog que se creó al crear el repositorio a partir de la plantilla.

1. En la barra lateral, dentro de **Sessions**, selecciona la carpeta del repositorio Tailspin Toys, no una sesión individual. Después, selecciona **Issues** en la navegación superior del repositorio.
2. Busca estas incidencias por su título, no des por hecho sus números:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecciona una incidencia para leer los detalles. También puedes iniciar una sesión de agente desde cualquier incidencia. Después de un primer cambio breve, empezarás desde la incidencia de filtrado.

> [!NOTE]
> Las listas de **Issues** y **Pull requests** se filtran automáticamente para mostrar solo elementos de los repositorios añadidos a la aplicación Copilot. Para ver elementos de otros repositorios, añádelos a la aplicación.

## Prueba un chat rápido

Una buena forma de familiarizarte con la aplicación es utilizarla para conocer la *propia aplicación*. Para ello, un **chat rápido** es la herramienta adecuada. Puedes hacer una pregunta o plantear ideas sin crear una rama ni un árbol de trabajo; es ideal para consultas breves y desechables, sin necesidad de iniciar una sesión.

1. En la barra lateral, selecciona **+** junto a **Chats** para abrir un chat. Si **Chats** aún no está visible, selecciona **+** junto a **Sessions** para iniciar el primer chat; después aparecerá **Chats**.
2. Pregunta cómo funcionan las sesiones de la aplicación:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Lee la respuesta en la vista de conversación. Verás que cada sesión se ejecuta en su propio árbol de trabajo de Git aislado, lo que permite ejecutar varios agentes en paralelo sin que sus cambios entren en conflicto. Puedes continuar la conversación o iniciar otra en cualquier momento.

## Resumen y próximos pasos

Has instalado la aplicación GitHub Copilot, conectado el proyecto y explorado el espacio de trabajo. Has aprendido a:

- instalar la aplicación e iniciar sesión en GitHub.
- añadir un proyecto desde su repositorio de GitHub.
- conocer el espacio de trabajo y localizar el backlog precargado en **Issues**.
- usar un chat rápido para hacer una pregunta breve y desechable.

A continuación, [iniciarás tu primera sesión de agente][next-lesson] y mostrarás una valoración por estrellas en las tarjetas de juegos.

## Recursos

- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Introducción a la aplicación GitHub Copilot][getting-started]
- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
