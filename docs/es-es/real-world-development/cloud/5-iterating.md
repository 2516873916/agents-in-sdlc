---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido a Copilot que añadiera documentación al código, creara una funcionalidad de juegos relacionados para que el equipo de diseño pudiera iterar y añadiera funciones de accesibilidad, incluidos los modos de alto contraste y claro. Revisemos los cambios de código y, si hace falta, demos indicaciones a Copilot para mejorar el resultado.

### Escenario

Como ya hemos señalado en varias ocasiones, los fundamentos del diseño de software y DevOps no cambian al añadir inteligencia artificial (IA) generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Con esto en mente, revisaremos las sugerencias de GitHub Copilot para crear la documentación, la funcionalidad de juegos relacionados y las funciones de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y GitHub Copilot cloud agent

Como Copilot cloud agent realiza tareas de forma asíncrona y sin supervisión, se han establecido ciertas restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utilizará para el código.
- Cloud agent se ejecuta en GitHub Actions, donde crea un entorno efímero independiente para trabajar.
- Una persona debe aprobar cualquier flujo de trabajo de GitHub Actions antes de que pueda ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empezaremos por explorar la primera solicitud de incorporación de cambios (PR) generada por GitHub Copilot cloud agent: la que añade documentación al código. Para ello, usaremos la interfaz habitual de solicitudes de github.com.

> [!NOTE]
> Al explorar la solicitud, puede que veas una advertencia que indica que el cortafuegos ha bloqueado GitHub Copilot. Es **lo esperado**, ya que Copilot tiene acceso limitado de forma predeterminada a los recursos externos, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el cortafuegos de Copilot cloud agent][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de solicitudes.
3. Abre la solicitud titulada, por ejemplo, **Add missing documentation** o con un título más detallado.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud de incorporación de cambios incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes descansar o repasar lo que has aprendido.

4. Cuando la solicitud esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los nuevos comentarios TSDoc y otra documentación. Los cambios concretos pueden variar.

    Mientras revisas los cambios, busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentarios. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que hay flujos de trabajo a la espera de aprobación.
8. Si hay flujos de trabajo esperando aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se completarán correctamente. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

Trabajar con Copilot en una solicitud no es un proceso unidireccional. Puedes mencionar a Copilot en comentarios, como harías con otros miembros del equipo, tanto en la solicitud como en comentarios insertados en el código. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos recomendar un texto específico. Puedes pedirle, por ejemplo:

- que añada encabezados de comentarios al principio de cada archivo de código con una breve descripción de su contenido.
- que añada comentarios TSDoc a los archivos TypeScript y Astro.
- que cree un archivo README con una descripción de la estructura de la aplicación Astro.

1. Añade un comentario para solicitar un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier usuario. Puedes utilizar alguna de las ideas anteriores o sugerir otro cambio de documentación.
2. Selecciona **View Session** para observar cómo trabaja Copilot. Verás que inicia una nueva sesión para realizar los cambios.
3. Puedes seleccionar **Back to pull request** para volver a la solicitud.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine los cambios, debería aparecer un nuevo commit en la solicitud.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Cuando lo estés, puedes marcar la solicitud como lista para revisión (convertirla de borrador a lista para revisión) y combinarla en la rama principal.

![Marcar la solicitud como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la funcionalidad de juegos relacionados

Volvamos a la solicitud que Copilot generó para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud cuyo título sea parecido a **Show related games on the game details page** o tenga una descripción más detallada.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que señala que hay flujos de trabajo a la espera de aprobación.
7. Si hay flujos de trabajo esperando aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se completarán correctamente. El proceso puede tardar unos minutos.
9. **Opcional:** también puedes cambiar a esta rama en tu codespace para probar manualmente la funcionalidad de juegos relacionados. Ve al codespace, abre el terminal y ejecuta los siguientes comandos. Sustituye `<branch-name>` por el nombre de la rama que creó Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la funcionalidad de juegos relacionados. Como antes, puedes trabajar de forma iterativa con Copilot cloud agent para pedir actualizaciones. Por ejemplo, podrías pedirle que cambiara cuántos juegos relacionados se muestran o que añadiera encabezados de comentarios y comentarios TSDoc (recuerda que la incidencia se asignó **antes** de actualizar las instrucciones personalizadas). Como antes, puedes hacer estas solicitudes con un nuevo comentario en la pestaña **Conversation**; Copilot lo verá e iniciará una nueva sesión.

## Revisa las funciones de accesibilidad

Por último, revisaremos las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta solicitud debe incluir tanto el modo de alto contraste que asignaste en el ejercicio 3 como el modo claro que solicitaste desde mission control en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la solicitud cuyo título sea parecido a **Add high contrast mode to website** o tenga una descripción más detallada.

> [!NOTE]
> Si Copilot sigue trabajando en la tarea, la solicitud de incorporación de cambios incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - los componentes de interfaz de los controles para cambiar de modo.
   - el uso del almacenamiento local para conservar las preferencias del usuario.
   - los cambios en las hojas de estilo en cascada (CSS) para los modos de alto contraste y claro.
   - los atributos de accesibilidad (etiquetas de Aplicaciones de Internet enriquecidas accesibles (ARIA), navegación con el teclado, etc.).
   - el código JavaScript/TypeScript que gestiona los cambios de modo.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que hay flujos de trabajo a la espera de aprobación.
8. Si hay flujos de trabajo esperando aprobación, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la solicitud. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se completarán correctamente. El proceso puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en tu codespace para probar manualmente las funciones de accesibilidad. Ve al codespace, abre el terminal y ejecuta los siguientes comandos. Sustituye `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    A continuación, inicia la aplicación y prueba los controles de los modos de alto contraste y claro en el navegador. Comprueba que funcionan como se espera y que la configuración se conserva al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ha ayudado a guiar a Copilot para implementar estas funciones siguiendo las prácticas recomendadas de accesibilidad. Si detectas problemas o posibles mejoras de accesibilidad, puedes mencionar a **@copilot** en un comentario y solicitar cambios, como hiciste con las solicitudes anteriores.

## Ejercicio opcional: sigue delegando

Cloud agent resulta especialmente útil cuando puedes delegarle elementos reales del backlog y centrarte en otras tareas. Para crear ese hábito, registra algunas incidencias más en tu repositorio y asígnaselas a Copilot. Algunas ideas:

- crear un formulario para mostrar interés por un juego en la página de detalles.
- implementar la paginación en la página de la lista de juegos.
- añadir validación de entradas y gestión de errores a los métodos de acceso a datos.

## Resumen

Has completado el itinerario de cloud agent. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizadas incluidos en este repositorio** para observar más adelante su efecto en el resultado de cloud agent.
- **Has asignado incidencias a Copilot cloud agent** y has visto cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** para accesibilidad y has añadido controles para los modos de alto contraste y claro.
- **Has utilizado la página de agentes de Copilot como centro de control** para supervisar y dirigir la sesión de accesibilidad mientras estaba en curso.
- **Has revisado las solicitudes de incorporación de cambios de cloud agent y has iterado sobre ellas**, mencionando a `@copilot` para solicitar cambios y aprobando flujos de trabajo.

## Repaso y siguientes pasos

Has completado el itinerario de cloud agent. Si quieres seguir explorando, los demás itinerarios complementan lo que has practicado:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y un puente opcional `/delegate` a cloud agent.

En tu propio repositorio, prueba estas ideas:

- asigna a cloud agent una incidencia de refactorización o de cobertura de pruebas.
- crea un agente personalizado para otro ámbito.
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración de Copilot cloud agent][cloud-agent-best-practices]
- [Configurar el cortafuegos de Copilot cloud agent][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
