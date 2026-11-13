---
title: "Lección 7 - Crea y utiliza un agente de QA"
description: "Crea un agente personalizado de QA que combine los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has utilizado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y el servidor del Protocolo de contexto de modelo (Model Context Protocol, MCP) de Playwright para observar la experiencia de filtrado en un navegador. Ahora combinarás esas capacidades en un agente personalizado con un proceso de control de calidad (QA) bien definido.

En esta lección, harás lo siguiente:

- explorar cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- crear e inspeccionar un perfil de control de calidad reutilizable.
- seleccionar el agente de QA y revisar sus conclusiones respecto a la incidencia de filtrado.

## Escenario

Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador antes de abrir una solicitud de incorporación de cambios (PR). Un agente personalizado puede coordinar el proceso de QA y proporcionar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil Markdown. El perfil describe el propósito, las instrucciones y las herramientas disponibles para el agente. En este taller, definirás un rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones que has utilizado tienen funciones distintas. Las instrucciones del repositorio describen los estándares del equipo. La habilidad `quality-checks` agrupa comprobaciones repetibles. Playwright MCP proporciona herramientas de navegador. El perfil de QA indica a Copilot cómo utilizar esas capacidades para evaluar los requisitos y comunicar los resultados. No sustituye a los demás elementos ni requiere otra conversación.

## Crea el perfil de QA

Antes de abrir la solicitud de incorporación de cambios, pedirás a Copilot que cree un perfil de QA reutilizable. El perfil definirá tanto las comprobaciones de QA como los límites que debe respetar.

1. Vuelve a tu codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el nuevo agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Inspecciona el perfil

Antes de usar el nuevo agente, revisa su perfil para confirmar que Copilot ha recogido el flujo de QA y sus límites. Así evitarás que un agente incompleto o demasiado permisivo cambie la funcionalidad cuando solo quieres verificarla.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, pero incluirlo da al agente un nombre claro en pantalla.
3. Lee las instrucciones del perfil y confirma que QA parte de los requisitos, sigue las instrucciones del repositorio, ejecuta la habilidad `quality-checks` y utiliza Playwright MCP.
4. Confirma que QA aporta pruebas que respaldan sus conclusiones, pide permiso antes de cambiar el código de implementación y no crea commits ni abre solicitudes de incorporación de cambios.
5. Si al perfil generado le falta alguna de estas responsabilidades o límites, pide al agente predeterminado que lo revise antes de continuar.

## Ejecuta QA con la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Reanuda la misma conversación de filtrado después de crear el perfil y selecciona QA para que pueda utilizar la incidencia y las decisiones tomadas durante la planificación.

1. Activa el agente con el siguiente prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, puede que no aparezca en la lista de agentes disponibles. Aun así, el comando anterior lo activará.

2. Usa el siguiente prompt para pedir al agente de QA que revise la funcionalidad:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. El agente de QA empieza a trabajar.
4. Cuando termine, lee el informe que te proporciona.

## Resumen y siguientes pasos

Has añadido un rol especializado reutilizable al flujo de trabajo y has revisado su trabajo. En esta lección, has:

- explorado cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP.
- creado e inspeccionado un perfil de control de calidad reutilizable.
- seleccionado el agente de QA y revisado sus conclusiones respecto a la incidencia de filtrado.

La implementación, la actualización de la habilidad, el perfil de QA, las pruebas y el informe de verificación están listos para revisarse. A continuación, [los reunirás en una solicitud de incorporación de cambios y utilizarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
