---
title: "Lección 1 - Instalar la aplicación GitHub Copilot"
description: "Instala la aplicación GitHub Copilot, conecta el repositorio creado desde la plantilla, familiarízate con el espacio de trabajo y prueba un chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La **[aplicación GitHub Copilot][about-copilot-app]** es una aplicación de escritorio para el desarrollo dirigido por agentes. Se basa en GitHub Copilot CLI y se integra de forma nativa con GitHub, por lo que los repositorios, las ramas y las canalizaciones de CI funcionan sin configuración adicional. Está diseñada para flujos de trabajo en los que diriges varios agentes en paralelo —cada uno en su propio espacio de trabajo aislado— y automatizas tareas repetitivas, en lugar de encargarte de todo el trabajo. Con Node.js instalado y la copia del proyecto preparada, el siguiente paso es instalar la aplicación y conectar el repositorio.

En esta lección:

- instalarás la aplicación GitHub Copilot e iniciarás sesión;
- añadirás el proyecto a la aplicación desde su repositorio de GitHub;
- conocerás el espacio de trabajo, incluido el backlog creado por la plantilla;
- probarás un chat rápido para conocer la propia aplicación.

## Escenario

Tu equipo está adoptando agentes de IA para abordar un backlog cada vez mayor. La aplicación Copilot te permite dirigir ese trabajo desde un solo lugar: seleccionar incidencias, ejecutar agentes, revisar cambios y combinar solicitudes de incorporación de cambios. En esta lección instalarás y conectarás la aplicación y aprenderás a iniciar una conversación sobre el proyecto.

> [!NOTE]
> Se necesita un plan de Copilot válido: Copilot Free, Copilot Student o cualquier plan de pago (Pro, Pro+, Business o Enterprise). Si usas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva **GitHub Copilot app**.

## Instalar y configurar la aplicación GitHub Copilot

El primer paso para utilizar la aplicación GitHub Copilot es instalarla. Hay versiones para Windows, macOS y Linux. Instala la aplicación, autentícate y añade el repositorio de Tailspin Toys.

1. En un navegador, abre la [página de inicio de la aplicación GitHub Copilot][download-app].
2. Descarga la aplicación para tu plataforma e instálala siguiendo las instrucciones de la página.
3. Abre la aplicación una vez instalada.
4. Selecciona **Sign in to GitHub** y sigue las indicaciones para autenticarte. Si utilizas GitHub Enterprise Cloud con residencia de datos, elige **Use GitHub Enterprise** e introduce el nombre de host `*.ghe.com` cuando se te solicite. GitHub Enterprise Server no es compatible.
5. Si durante la incorporación se te pide seleccionar un repositorio, elige el repositorio Tailspin Toys que has creado desde la plantilla. También puedes omitir la selección y añadir el proyecto después.
6. Cuando se te solicite, elige un tema y termina el proceso de incorporación.
7. Busca el repositorio en **Sessions**, en la barra lateral. Si ya aparece en la lista, no hace falta que lo añadas de nuevo.
8. Si aún no está conectado, selecciona **+** junto a **Sessions** para crear una sesión nueva y, después, **Add GitHub repository**. Usa el propietario y el nombre del repositorio que anotaste en la lección de requisitos previos.
9. Confirma que el repositorio aparece en la barra lateral. En el diseño que se muestra aquí, las carpetas de repositorio, como `tailspin-toys`, se agrupan bajo **Sessions**, y cada repositorio contiene sesiones individuales.

## Familiarizarte con el espacio de trabajo

Con el proyecto conectado, dedica un momento a conocer el espacio de trabajo. La aplicación organiza la información en varias áreas de la barra lateral:

- **New**: inicia una sesión de chat nueva con Copilot.
- **Pull requests**: muestra las solicitudes de incorporación de cambios mediante la integración nativa con GitHub. Puedes examinarlas y filtrarlas, comprobar el estado de CI y revisarlas sin salir de la aplicación.
- **Issues**: muestra las incidencias de la misma manera. Puedes examinarlas y filtrarlas, y empezar una sesión desde una incidencia.
- **Automations**: tareas de agente guardadas que se ejecutan según una programación o bajo demanda. Sirven para gestionar listas de tareas, realizar el mantenimiento periódico del proyecto o delegar tareas tediosas. El repaso final las presenta como un paso siguiente, no como otro ejercicio del taller.
- **Customize**: añade funciones a la aplicación Copilot mediante servidores MCP, plugins, habilidades y otros componentes. La utilizarás para configurar Playwright MCP.
- **Chats**: conversaciones ligeras para preguntas e ideas que no necesitan una rama ni un espacio de trabajo propios. Esta sección aparece cuando inicias el primer chat. Si no está visible, selecciona **+** junto a **Sessions** para empezar un chat.
- **Sessions**: agrupa carpetas de repositorio y sesiones de agente. Selecciona el nombre de un repositorio, como `tailspin-toys`, para abrir su vista, o una sesión debajo para volver a esa conversación. Cada sesión se ejecuta en su propio espacio de trabajo aislado, así que puedes ejecutar varias a la vez sin que sus cambios entren en conflicto. Iniciarás la primera sesión cuando añadas valoraciones por estrellas.

A medida que avances por el taller, explorarás el espacio de trabajo.

> [!TIP]
> Si tienes dudas, pregunta a Copilot. Si no sabes cómo hacer algo o si es posible, puede ayudarte a orientarte.

### Encontrar el backlog inicial

Es difícil encontrar un proyecto sin trabajo pendiente, y Tailspin Toys no es una excepción. Vamos a explorar el backlog que se creó al preparar la plantilla.

1. En **Sessions**, selecciona la carpeta del repositorio Tailspin Toys, no una sesión individual. Después, selecciona **Issues** en la navegación superior del repositorio.
2. Busca estas incidencias por su título, sin dar por hecho su número:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecciona una incidencia para leer los detalles. Cada incidencia también permite iniciar una sesión de agente. Partirás de la incidencia de filtrado después de completar un primer cambio rápido.

> [!NOTE]
> Las listas de **Issues** y **Pull requests** solo muestran automáticamente los elementos de los repositorios que has añadido a la aplicación Copilot. Para consultar elementos de otros repositorios, añádelos a la aplicación.

## Probar un chat rápido

Una buena forma de familiarizarte con la aplicación es utilizarla para conocer la propia aplicación. Un **chat rápido** es justo lo que necesitas. Permite hacer preguntas o intercambiar ideas sin crear una rama ni un árbol de trabajo; es ideal para consultas puntuales que no requieren una sesión.

1. En la barra lateral, selecciona **+** junto a **Chats** para abrir un chat nuevo. Si **Chats** aún no está visible, selecciona **+** junto a **Sessions** para iniciar el primer chat; después aparecerá **Chats**.
2. Pregunta cómo funcionan las sesiones de la aplicación:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Lee la respuesta en la vista de conversación. Verás que cada sesión se ejecuta en su propio árbol de trabajo de Git aislado, lo que permite ejecutar varios agentes en paralelo sin que sus cambios entren en conflicto. Puedes continuar la conversación o iniciar un chat nuevo en cualquier momento.

## Resumen y siguientes pasos

¡Enhorabuena! Has instalado la aplicación GitHub Copilot, conectado el proyecto y explorado el espacio de trabajo. Has aprendido a:

- instalar la aplicación e iniciar sesión en GitHub;
- añadir un proyecto desde su repositorio de GitHub;
- familiarizarte con el espacio de trabajo y encontrar el backlog inicial en **Issues**;
- utilizar un chat rápido para hacer una consulta puntual.

A continuación, [iniciarás tu primera sesión de agente][next-lesson] y la utilizarás para mostrar las valoraciones por estrellas en las tarjetas de juegos.

## Recursos

- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Introducción a la aplicación GitHub Copilot][getting-started]
- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
