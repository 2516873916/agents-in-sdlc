---
slug: es-es/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** permite que GitHub Copilot trabaje de forma asíncrona en la nube. Le asignas trabajo en GitHub y cloud agent se ocupa de él en segundo plano: explora el repositorio, hace cambios y abre una solicitud de incorporación de cambios mientras tú te dedicas a otras tareas.

En estos ejercicios añadirás instrucciones personalizadas que seguirá cloud agent y, después, le asignarás una incidencia para que implemente el trabajo. Revisarás y utilizarás agentes personalizados para orientar su enfoque, supervisarás y dirigirás las sesiones desde el panel de agentes y, por último, revisarás sus solicitudes de incorporación de cambios e iterarás sobre los resultados.

## Ejercicios

| Ejercicio | Tema | Descripción |
|----------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Crea el repositorio y el codespace |
| [1. Instrucciones personalizadas][ex1] | Contexto | Añade instrucciones personalizadas para cloud agent |
| [2. Cloud agent][ex2] | Agente asíncrono | Asigna incidencias a Copilot cloud agent |
| [3. Agentes personalizados][ex3] | Agentes especializados | Revisa y utiliza agentes personalizados |
| [4. Gestionar agentes][ex4] | Supervisión | Supervisa y dirige sesiones de agentes |
| [5. Iteración][ex5] | Revisión | Revisa solicitudes de incorporación de cambios, itera sobre el trabajo de Copilot y elige los siguientes pasos |

## Requisitos previos

Antes de participar en este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Copilot cloud agent habilitado en tu cuenta u organización

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis a través de [GitHub Education][callout-student-plan-education]. El plan **Copilot Student** incluye el agente, el Protocolo de contexto de modelo (Model Context Protocol, MCP), la revisión de código y las funciones de Copilot CLI que utiliza este taller; con él podrás completar todos los itinerarios.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent está disponible en los planes **Copilot Student, Pro, Pro+, Business y Enterprise**; en todos salvo Copilot Free. En Copilot Business o Enterprise, un administrador debe habilitarlo para la organización.

> [!NOTE]
> Este itinerario no incluye MCP. Para explorar el uso de servidores MCP con Copilot, consulta el [itinerario de CLI](../cli/) o el [itinerario de VS Code](../vscode/).

## Empieza

**[Empieza con el ejercicio 0: Requisitos previos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
