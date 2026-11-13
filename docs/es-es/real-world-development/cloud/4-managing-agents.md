---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores, pediste a Copilot cloud agent que se ocupara de tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque cloud agent trabaja de forma autónoma y asíncrona, sigue siendo importante poder supervisar estas tareas.

Hay varias herramientas para gestionar las tareas asignadas a cloud agent, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes que tienen solicitudes de incorporación de cambios abiertas. También puedes explorar las operaciones realizadas e incluso dirigir una sesión en curso.

En esta lección, harás lo siguiente:

- explorar la página de agentes para supervisar las tareas de cloud agent.
- dirigir una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo decidió que también sería un buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo la funcionalidad de alternancia, parecía lógico incluirlo. Dirigirás el trabajo del agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas de Copilot cloud agent

Vamos a consultar el estado actual de todas las tareas asignadas a Copilot cloud agent.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación de la base de código.
   - generar interfaces de programación de aplicaciones (API) para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las tareas que Copilot ha realizado. Pueden incluir:
   - obtener el código del repositorio.
   - crear el entorno de trabajo de Copilot.
   - configurar servidores del Protocolo de contexto de modelo (Model Context Protocol, MCP).
   - realizar distintos pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos de la lista variarán según el estado del trabajo de Copilot y el enfoque utilizado.

4. Fíjate también en el panel de solicitudes de incorporación de cambios que aparece a la derecha. Desde allí puedes ver la solicitud y los archivos modificados para supervisar mejor el trabajo.

## Dirige cloud agent

Ya has visto las tareas activas. Ahora pediremos a Copilot que incluya el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos hasta que indique que ha terminado de configurarse y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes parecidos a los siguientes.
3. En el diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarea de cloud agent en la página de agentes con el diálogo para dirigir una sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que lo ha recibido y lo incorpora a su flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud al flujo de trabajo después de completar el paso en el que estaba trabajando cuando enviaste el mensaje.

Este proceso también tardará varios minutos, así que puedes aprovechar para repasar lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro desde el que se supervisan y dirigen las tareas de GitHub Copilot cloud agent. Desde este centro de control puedes seguir todas las tareas activas y completadas, revisar el trabajo que se está realizando y redirigir tareas en curso para ajustar su alcance o añadir indicaciones.

Has explorado estos conceptos:

- explorar mission control y la página de agentes para supervisar las tareas de cloud agent.
- redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

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
