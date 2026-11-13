---
title: "Ejercicio 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan problemas con la deuda técnica. Puede tratarse de problemas de seguridad sin resolver, código heredado que hay que actualizar o solicitudes de funcionalidad que llevan mucho tiempo en el backlog porque no ha habido tiempo para implementarlas. Copilot cloud agent puede realizar tareas como actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, genera una solicitud de incorporación de cambios (PR) en borrador para que la revise un desarrollador. Así es posible delegar tareas tediosas, acelerar el desarrollo y permitir que los desarrolladores se centren en cuestiones más amplias.

Con Copilot cloud agent explorarás lo siguiente:

- personalizar el entorno de generación de código.
- asegurar que las operaciones se realizan de forma segura.
- comprender la importancia de delimitar claramente las incidencias.
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica que quiere abordar. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado poco satisfactorio: comprobarás que falta por completo. Como primer paso, quieren añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la exploración de juegos. Quiere que la página de detalles de cada juego muestre juegos relacionados, es decir, otros títulos de la misma categoría, para que la gente pueda seguir explorando. Por ahora no necesita una implementación refinada; solo quiere algo que sirva para probar la experiencia de usuario. Aunque esta tarea bloquea el trabajo actual, hay otras incidencias de mayor prioridad.

Estas son tareas que suelen quedar relegadas y que se pueden asignar a Copilot cloud agent. Cloud agent puede trabajar en ellas de forma asíncrona mientras el desarrollador se centra en otras tareas y, después, puede volver para revisar el trabajo de Copilot y comprobar que todo está como se espera.

## Presentación de GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a un desarrollador. Al igual que cuando trabajas con otra persona, puedes asignarle trabajo de distintas formas, por ejemplo, [asignándole una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Una vez asignada, Copilot crea una solicitud de incorporación de cambios en borrador para registrar el progreso, configura un entorno y empieza a trabajar. Puedes consultar la sesión de Copilot mientras sigue en curso o una vez terminada. Cuando la solución esté lista para revisarla, Copilot te mencionará en la solicitud de incorporación de cambios.

## La importancia de delimitar bien las instrucciones

GitHub Copilot no hace magia: no puedes saltarte el trabajo de análisis. Incluso las tareas aparentemente sencillas tienen complejidades que descubrirás al estudiarlas con detalle. Por eso, [delimita con cuidado las tareas de Copilot cloud agent][cloud-agent-best-practices]. Trátalo como a un compañero de programación con inteligencia artificial (IA): trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian al incorporar la IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores hemos presentado las instrucciones personalizadas y cómo guían a Copilot. Si ya has completado el ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo a Copilot cloud agent, inspecciona los archivos de instrucciones que ya incluye este repositorio sin modificarlos para poder observar después su efecto.

Abre los siguientes archivos en la interfaz web de GitHub o en un codespace si ya tienes uno abierto:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las expectativas para las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes a cloud agent la incidencia `Code lacks documentation` en la sección siguiente, observa si la solicitud de incorporación de cambios resultante incluye comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Estas indicaciones proceden de los archivos de instrucciones. Volveremos a señalarlas al revisar las solicitudes en un ejercicio posterior.

## Configura el entorno de desarrollo de Copilot cloud agent

Para crear código, independientemente de quién lo haga, suele ser necesario contar con un entorno específico y ejecutar algunos scripts de configuración para que todo esté listo. Lo mismo ocurre cuando se asignan tareas a Copilot, que trabaja de forma parecida a un ingeniero de software (SWE).

Cloud agent utiliza [GitHub Actions][github-actions] como entorno de trabajo. Puedes personalizarlo mediante un [flujo de trabajo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece a trabajar. Así puede acceder a las herramientas de desarrollo y dependencias necesarias. El flujo ya está configurado para facilitar el taller y permitirte explorar esta posibilidad. Da a Copilot acceso a Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y precargada de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para *tus pasos*. Copilot tendrá su propio token para sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de los pasos de configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en esos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y de la aplicación: Node.js (ahora toda la aplicación usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y precarga la base de datos SQLite local para que las compilaciones y pruebas dispongan de datos.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos importantes:

- Contiene un único trabajo llamado `copilot-setup-steps`. Se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- Observa el desencadenador `workflow_dispatch`, que permite ejecutar manualmente el flujo de trabajo desde la pestaña **Actions** del repositorio. Esto sirve para comprobar que se ejecuta correctamente sin esperar a que Copilot lo inicie.

## Añade documentación

Aunque todo el mundo reconoce la importancia de la documentación, la información de la mayoría de los proyectos está obsoleta o falta por completo. Esta deuda técnica suele quedar desatendida, lo que reduce la productividad y dificulta el mantenimiento del código o la incorporación de nuevos desarrolladores al equipo. Copilot destaca en la creación de documentación, por lo que esta es una incidencia ideal para asignarle a Copilot cloud agent. Trabajará en segundo plano para generar la documentación necesaria. En un ejercicio posterior volverás para revisar el resultado.

1. En una nueva pestaña del navegador, ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el diálogo de creación de incidencias.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el diálogo de asignación.

  ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la solicitud de incorporación de cambios (PR) que se acaba de generar; tendrá un título parecido a `[WIP]: Code lacks documentation`. Si no aparece ninguna solicitud nueva en la lista, espera un momento y actualiza la página.
12. Después de unos minutos, Copilot debería haber creado una lista de tareas pendientes.

> [!NOTE]
> La lista de tareas de Copilot puede tardar varios minutos en aparecer en la solicitud. Copilot está preparando su entorno (ejecutando el flujo de trabajo que acabamos de ver), analizando el proyecto y determinando el mejor enfoque.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la solicitud de incorporación de cambios. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Observa que puedes desplazarte por la sesión en directo y ver cómo Copilot resuelve el problema. Explora el código y comprende el estado del proyecto, observa cómo Copilot se detiene a pensar y decidir el plan adecuado y cómo crea el código.

El proceso puede tardar varios minutos. Uno de los objetivos principales de Copilot cloud agent es realizar tareas asíncronas para que puedas centrarte en otras cosas. Aprovecharemos esa característica asignándole otra tarea y, después, nos centraremos en escribir código para añadir funcionalidades a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Como ya hemos indicado, una gran ventaja de GitHub Copilot cloud agent es poder dividir el trabajo: tú puedes centrarte en un conjunto de tareas mientras el agente se ocupa de otro. Añadir una sección de juegos relacionados para el equipo de diseño quizá no lleve mucho tiempo, pero ese tiempo se puede dedicar a otras tareas. ¡Vamos a asignársela a Copilot cloud agent!

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el diálogo de creación de incidencias.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. Escribe `Show related games on the game details page` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el diálogo de asignación.

  ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, deberías ver una serie de 👀 en el primer comentario de la incidencia. Indica que Copilot ya está trabajando en ella.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. Copilot cloud agent trabaja de forma parecida a un ingeniero de software (SWE), así que no hace falta que lo supervises continuamente; puedes revisar el resultado cuando termine. Ahora nos centraremos en crear y utilizar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado [GitHub Copilot cloud agent][copilot-agents]. Puedes asignar incidencias a cloud agent para que las resuelva de forma asíncrona. Copilot puede abordar deuda técnica, crear funcionalidades o ayudar a migrar código de un framework a otro.

Has explorado estos conceptos:

- personalizar el entorno de generación de código.
- asegurar que las operaciones se realizan de forma segura.
- comprender la importancia de delimitar claramente las incidencias.
- asignar incidencias a Copilot.

Mientras cloud agent trabaja en segundo plano, puedes centrarte en crear y utilizar agentes personalizados. [Copilot cloud agent también puede utilizar servidores del Protocolo de contexto de modelo (Model Context Protocol, MCP)][cloud-agent-mcp] y puede acceder a las instrucciones personalizadas que hemos explorado en módulos anteriores.

## Recursos

- [Acerca de Copilot cloud agent][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración de Copilot cloud agent][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
