---
title: "Lección 8 - Crea y combina la solicitud de incorporación de cambios"
description: "Revisa juntos el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, crea una solicitud de incorporación de cambios y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de las instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Es el momento de revisarlos en conjunto y abrir una solicitud de incorporación de cambios. Combinaste manualmente la solicitud de incorporación de cambios (PR) de la valoración con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de las instrucciones, la actualización de la habilidad y el agente de QA en varias solicitudes de incorporación de cambios. Para simplificar el taller, has mantenido todo el flujo de filtrado y calidad en una conversación y una rama, y lo incluirás en esta solicitud.

En esta lección, harás lo siguiente:

- aprender qué es Agent Merge y cómo automatiza el ciclo de vida de una solicitud de incorporación de cambios.
- inspeccionar todos los cambios de la funcionalidad y las pruebas de verificación.
- crear la solicitud de incorporación de cambios de filtrado.
- habilitar Agent Merge solo después de la revisión y confirmar que se ha combinado la solicitud.

## Escenario

Durante el flujo de trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la solicitud de incorporación de cambios, manteniendo el control de autorización de la combinación en manos del desarrollador.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios. Cuando lo habilitas, Copilot resuelve los bloqueos de la solicitud: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y reaplica los cambios sobre la rama base cuando hace falta. Después, habilita la combinación automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado tú **Merge pull request**. Agent Merge puede encargarse de esa responsabilidad. Revisa el trabajo y asegúrate de que está listo antes de habilitar Agent Merge.

## Usa Agent Merge para gestionar la solicitud de incorporación de cambios

Ya has creado todo el código. Vamos a revisarlo, crear la solicitud de incorporación de cambios y dejar que Agent Merge gestione el resto del proceso.

1. Vuelve a tu codespace.
2. Abre el diálogo del agente escribiendo `/agent`.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Crea una solicitud de incorporación de cambios con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la solicitud. Cuando todo se complete correctamente, la combinará.
7. Comprueba que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo indicado y completa manualmente la combinación revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: generar código, probarlo y validarlo, y gestionar la solicitud de incorporación de cambios. En esta lección, has:

- aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una solicitud de incorporación de cambios.
- inspeccionado todos los cambios de la funcionalidad y las pruebas de verificación.
- creado la solicitud de incorporación de cambios de filtrado.
- habilitado Agent Merge solo después de revisar los cambios y has confirmado que se han combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
