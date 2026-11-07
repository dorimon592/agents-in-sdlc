---
title: "Lección 0 - Requisitos previos"
description: "Prepara el entorno para las lecciones de la aplicación GitHub Copilot: instala Node.js para Tailspin Toys y crea una copia propia del repositorio a partir de la plantilla."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La aplicación GitHub Copilot es una aplicación de escritorio que sirve como centro de operaciones para Copilot y GitHub. Proporciona acceso rápido a incidencias y solicitudes de incorporación de cambios y permite desarrollar con GitHub Copilot. Durante este taller trabajarás en local con la aplicación Tailspin Toys, creada con Astro, mediante la aplicación GitHub Copilot. Antes de empezar, comprueba que Node.js esté instalado localmente y, después, instala la aplicación Copilot.

En esta lección:

- instalarás Node.js para poder ejecutar las pruebas del proyecto en tu equipo.
- crearás una copia propia del proyecto Tailspin Toys a partir de la plantilla.

## Instala Node.js

En varias lecciones pedirás a un agente que implemente funcionalidades y ejecute en local las pruebas de Tailspin Toys. Para ello se necesita **[Node.js][nodejs]**, el único entorno de ejecución que requiere el proyecto. Instala la versión **LTS** actual.

La opción más sencilla en todas las plataformas es usar el instalador oficial:

1. Abre un terminal en el sistema operativo. Puedes usar Windows Terminal, Terminal en macOS o el terminal que suelas utilizar.
2. Ejecuta estos comandos para comprobar las versiones instaladas de Node.js y npm:

    ```shell
    node --version
    npm --version
    ```

3. Si ambos comandos funcionan y Node.js cumple los requisitos indicados en el README del proyecto y en `package.json`, pasa a la sección siguiente.

> [!TIP]
> Solo tienes que completar estos pasos si no tienes Node.js instalado o necesitas actualizarlo.

4. Abre la [página de descarga de Node.js][node-download].
5. Descarga la versión **LTS** para tu sistema operativo.
6. Ejecuta el instalador y acepta las opciones predeterminadas. En Windows, deja seleccionada la opción **Add to PATH**.
7. Cuando termine la instalación, abre un terminal nuevo.
8. Comprueba la instalación desde ese terminal:

    ```bash
    node --version
    npm --version
    ```

9. Deberían aparecer las versiones instaladas de Node.js y npm.

> [!IMPORTANT]
> El agente de la aplicación Copilot también debe poder ejecutar `node --version` y `npm --version`. Que funcionen en el terminal habitual no garantiza que estén disponibles en la aplicación. Cierra completamente y vuelve a abrir la aplicación después de instalar Node.js o cambiar `PATH`. Si alguno de los comandos sigue fallando en una sesión, sigue los [pasos para solucionar problemas del entorno de sesión][session-environment] antes de continuar.

> [!IMPORTANT]
> Cada árbol de trabajo también necesita las dependencias del proyecto y Chromium de Playwright para las comprobaciones E2E. Sigue el README del repositorio de Tailspin Toys para preparar un árbol de trabajo y revisa cualquier solicitud de instalación antes de aprobarla.

## Configura el repositorio del taller

Trabajarás con una copia propia del proyecto Tailspin Toys. Créala ahora a partir del [repositorio de plantilla][template-repository]. Incluirá todos los archivos necesarios para el taller y la conectarás al instalar la aplicación.

1. En una nueva ventana del navegador, ve al repositorio de GitHub de este taller: `https://github.com/github-samples/tailspin-toys`.
2. Crea una copia propia del repositorio seleccionando **Use this template** en la página del repositorio del taller. Después, selecciona **Create a new repository**.

    ![Botón Use this template con Create a new repository seleccionado en el menú desplegable](../../../_images/app-0-use-template.png)

3. Si participas en un evento organizado por GitHub o Microsoft, sigue las instrucciones de los mentores. De lo contrario, crea el repositorio en una organización donde tengas acceso a GitHub Copilot.

    ![Formulario Create a new repository con github-samples/tailspin-toys como plantilla y el nombre del repositorio completado](../../../_images/app-0-create-repository.png)

4. Anota la ruta del repositorio que has creado (**organization-or-user-name/repository-name**), ya que la necesitarás más adelante en el taller.

> [!NOTE]
> Al crear el repositorio a partir de la plantilla, se genera automáticamente un backlog de incidencias de GitHub. Trabajarás con esas incidencias durante el taller; no tienes que crear ninguna.

Utiliza una copia nueva de la plantilla del taller. Incluye instrucciones del repositorio, código de la aplicación, pruebas, una habilidad `quality-checks` y una extensión de Canvas existente. Personalizarás la habilidad y crearás un agente de QA durante el taller. Si utilizas una copia anterior, comprueba con quien imparte el taller que contiene los archivos necesarios.

## Resumen y próximos pasos

Ya tienes todo listo. En esta lección:

- has instalado Node.js para compilar y probar el proyecto en tu equipo.
- has creado una copia propia del repositorio Tailspin Toys a partir de la plantilla.

A continuación, [instalarás la aplicación GitHub Copilot][next-lesson], conectarás el repositorio y conocerás el espacio de trabajo.

## Recursos

- [Descargar Node.js][node-download]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#iniciar-una-sesion-desde-la-incidencia
