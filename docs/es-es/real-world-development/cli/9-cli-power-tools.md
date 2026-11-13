---
title: "Lección 9 - Comandos de barra de GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar tareas opcionalmente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier CLI de calidad, GitHub Copilot CLI incluye numerosos comandos de barra para interactuar con la herramienta. Estos comandos ofrecen funciones avanzadas, información interna y opciones de configuración adicionales. Ya has utilizado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Vamos a explorar otros comandos útiles.

En esta lección, harás lo siguiente:

- usar `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de las conversaciones.
- usar `/model` para explorar los modelos disponibles.
- aprender cómo `/share` puede exportar o compartir una sesión.
- explorar comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas en cloud agent.

## Escenario

Has terminado el flujo de trabajo básico de la CLI. Ahora veremos algunas capacidades adicionales: gestionar el contexto, cambiar de modelo, compartir sesiones y delegar tareas opcionalmente en [Copilot cloud agent][about-cloud-agent].

## Explora el contexto de Copilot CLI

Al trabajar en tareas grandes o complejas, puede que alcances la ventana de contexto máxima del modelo. Copilot CLI compacta automáticamente la conversación cuando es necesario, y puedes inspeccionarla o compactarla tú mismo con comandos de barra.

1. Vuelve a tu codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y cómo se distribuye el contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio disponible.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a escribir `/context` y compara el resultado. Si la conversación ya es breve, puede que no haya cambiado mucho.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto cuando se llena la ventana. Usa `/compact` si quieres decidir cuándo hacerlo. Usa `/clear` o `/new` cuando cambies a una tarea que no esté relacionada y quieras empezar una conversación nueva.

## Elige el modelo

Los modelos tienen distintas fortalezas y cada desarrollador tiene sus propias preferencias. Copilot CLI permite mostrar y seleccionar el modelo que quieras utilizar.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información de uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Comparte una sesión

Colaborar en equipo y compartir lo aprendido ayuda a mejorar el uso de las herramientas de inteligencia artificial (IA). El comando `/share` puede exportar una sesión a un archivo Markdown o en lenguaje de marcado de hipertexto (HTML), crear un enlace para compartir o publicar un gist de GitHub.

1. Escribe `/help` y revisa las opciones de `/share` disponibles en la versión instalada.
2. Si quieres compartir esta sesión, elige el destino adecuado, como `/share file` para exportar un archivo Markdown local.
3. Revisa el contenido exportado antes de enviárselo a alguien o publicarlo. Las exportaciones de sesiones pueden incluir prompts, respuestas y detalles del proyecto.

Publicar un enlace o un gist es opcional. No publiques contenido del repositorio o de una conversación que el equipo no tenga previsto compartir.

## Opcional: amplía el trabajo o delégalo

El flujo de trabajo principal del taller ha terminado. Copilot CLI también incluye comandos para tareas más grandes:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo de Git aislado para una tarea distinta.
- `/delegate` puede enviar una tarea a Copilot cloud agent, que trabaja de forma asíncrona y puede abrir una solicitud de incorporación de cambios.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o trabajo remoto. Empieza una tarea nueva y bien acotada antes de probarlos, y revisa el resultado con el flujo de trabajo habitual. Si quieres profundizar en el trabajo asíncrono con agentes, continúa con el [taller de cloud agent][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI permiten configurarlo, compartir sesiones y ver qué ocurre en segundo plano. En esta lección, has:

- usado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de las conversaciones.
- usado `/model` para explorar los modelos disponibles.
- aprendido cómo `/share` puede exportar o compartir una sesión.
- explorado comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas en cloud agent.

Copilot CLI tiene más comandos de barra y muchas otras posibilidades. Para terminar el recorrido, [repasaremos lo aprendido][next-lesson] y veremos los siguientes pasos.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca de Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
