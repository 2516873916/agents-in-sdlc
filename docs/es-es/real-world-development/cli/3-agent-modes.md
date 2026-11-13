---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para implementar el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una pequeña funcionalidad al proyecto. Sin embargo, los cambios más amplios requieren un proceso más sólido. Por suerte, GitHub Copilot CLI está diseñado para trabajar con el flujo de trabajo existente de una organización y ayudarnos a crear lo que necesitamos de la forma adecuada. Esta es la primera de varias lecciones en las que seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para generar una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como se espera y, finalmente, la combinarás correctamente en el proyecto.

> [!NOTE]
> Utilizarás la misma conversación y rama durante todo el flujo de trabajo de la funcionalidad. Lo habitual sería usar distintas ramas o solicitudes de incorporación de cambios para los diferentes tipos de archivos, pero simplificaremos el proceso para centrarnos en los conceptos principales.

Para empezar, en esta lección harás lo siguiente:

- iniciar una nueva conversación de Copilot a partir de una incidencia de GitHub.
- definir los requisitos en el modo Plan.
- implementar la nueva funcionalidad con el modo Autopilot.
- revisar el código.
- validar manualmente la funcionalidad en un navegador reenviado.

Durante el desarrollo de esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás validación con el Protocolo de contexto de modelo (Model Context Protocol, MCP), crearás un agente de control de calidad (QA) y abrirás la solicitud de incorporación de cambios.

## Escenario

El catálogo de Tailspin Toys está creciendo y los visitantes necesitan filtrar los juegos por categoría y editor. El backlog incluye una incidencia que describe la funcionalidad, pero hay que acordar algunos detalles, como la combinación de categorías, antes de programarla. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

La introducción de agentes de programación con inteligencia artificial (IA) en tu flujo de desarrollo no cambia los fundamentos. De hecho, los hace aún más importantes. La mayoría de los desarrolladores siguen un flujo parecido a este:

1. Partir de una incidencia registrada que describa el trabajo necesario.
2. Crear un plan con lo que hay que desarrollar.
3. Crear y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Cuando se haya revisado el código y se haya completado correctamente el proceso de integración continua, combinar el código.

> [!NOTE]
> Los detalles concretos dependen del equipo y de la organización, pero la mayoría de los procesos siguen alguna variante de este modelo.

Al seguir este enfoque habitual, te aseguras de que el código generado por IA cumple los requisitos establecidos y pasa por el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** controla el grado de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: trabajas con el agente. Este sugiere cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas y repite el proceso hasta que considera que la tarea está terminada.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Empieza por la incidencia

Antes de iniciar el trabajo de filtrado, vuelve a tu codespace y comprueba que el repositorio y el terminal están listos.

1. Vuelve a tu codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que la solicitud de incorporación de cambios de valoración con estrellas se ha combinado.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de flecha para resaltar la incidencia titulada **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que el prompt ahora empieza por `#7` (o un número similar). El carácter `#` permite añadir al contexto una incidencia o solicitud de incorporación de cambios (PR) de GitHub.

## Planifica la funcionalidad de filtrado

La planificación permite definir cómo se implementará una funcionalidad o se realizará una tarea antes de encargársela a Copilot. Para cualquier trabajo complejo, siempre conviene dedicar un poco de tiempo a planificar. Cambia al modo Plan y pide a Copilot que cree el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador debajo del prompt muestra **Plan**.
2. Después de la referencia a la incidencia que has añadido en el paso anterior, escribe el siguiente prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot se pone a preparar el plan. Primero explorará el proyecto y, después, determinará el mejor enfoque.

3. Durante el proceso, Copilot puede hacerte preguntas sobre cómo debe funcionar el filtrado. Respóndelas según tus preferencias. No hay respuestas incorrectas.
4. Cuando el plan esté listo, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que modifique las partes del plan que no se ajusten a tus decisiones.

## Autoriza Autopilot

Una vez redactado y revisado el plan, es el momento de implementarlo. Deja que Copilot trabaje con el modo Autopilot.

Autopilot permite a Copilot iterar sobre el problema hasta que considera que está resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción equivalente que aparezca en la versión instalada.
2. Confirma que el indicador debajo del prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan acordado, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa primero el plan. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar por terminadas las comprobaciones.

## Revisa y verifica la implementación

Una vez generado el código, hay que revisarlo antes de combinarlo, como cualquier otro código. Revisaremos el código y ejecutaremos el sitio para comprobar que todo funciona correctamente.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones que tomaste durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que resuelva los errores.

## Explora la nueva funcionalidad

De acuerdo, el código parece correcto, pero ¿se ejecuta? Iniciemos la aplicación como antes y abramos el sitio a través del puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba los filtros por categoría y editor, así como las combinaciones que acordaste en el plan.
4. Confirma que las acciones para restablecer los filtros y el comportamiento sin resultados coinciden con la incidencia y con tus decisiones.
5. Vuelve a tu codespace y pide a Copilot que detenga el servidor de desarrollo que ha iniciado.

## Resumen y siguientes pasos

Has usado distintos modos de conversación para crear y revisar una funcionalidad. En esta lección, has:

- iniciado una nueva conversación de Copilot a partir de una incidencia de GitHub.
- definido los requisitos en el modo Plan.
- implementado la nueva funcionalidad con el modo Autopilot.
- revisado el código.
- validado manualmente la funcionalidad en un navegador reenviado.

A continuación, profundizarás en cómo se genera el código y te asegurarás de que sigue las prácticas documentadas [mediante instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
