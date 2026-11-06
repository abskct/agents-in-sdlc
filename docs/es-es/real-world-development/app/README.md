---
slug: es-es/real-world-development/app
title: "Aplicación GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La [**aplicación GitHub Copilot**](https://docs.github.com/copilot/concepts/agents/github-copilot-app) es una aplicación de escritorio basada en Copilot CLI que reúne el desarrollo dirigido por agentes en un único espacio de trabajo específico. Añade sesiones de agente en paralelo, modos de sesión intercambiables, lienzos compartidos y administración nativa de incidencias y solicitudes de incorporación de cambios de GitHub, incluido **Agent Merge**, que guía las solicitudes durante la actualización de ramas, los comentarios de revisión, las correcciones de integración continua (CI) y la fusión.

El taller sigue un flujo de trabajo continuo de Tailspin Toys:

1. Prepara el proyecto, instala la aplicación, conecta el repositorio y explora el espacio de trabajo y la lista de tareas pendientes.
2. Realiza un cambio acotado para mostrar las valoraciones por estrellas, revísalo en el navegador y fusiona manualmente tu primera solicitud de incorporación de cambios (PR).
3. Parte de la incidencia de filtrado, define el enfoque en modo **Plan**, impleméntalo en modo **Autopilot** y revísalo en modo **Interactive**.
4. Actualiza las instrucciones del repositorio y aplícalas al trabajo de filtrado.
5. Personaliza la aptitud existente `quality-checks` y úsala para ejecutar las comprobaciones del proyecto.
6. Añade el servidor Model Context Protocol (MCP) de Playwright y úsalo para explorar el filtrado en un navegador.
7. Crea un agente personalizado de control de calidad (QA) y úsalo para revisar los requisitos, la cobertura y las pruebas de verificación.
8. Revisa todos los cambios del filtrado y usa Agent Merge para la segunda PR.
9. Comprueba si hay lienzos disponibles y, después, usa Database Explorer para crear y probar un lienzo de triaje respaldado por el repositorio. Si no dispones de las capacidades necesarias, registra el bloqueo y usa la alternativa de solo lectura o continúa con el resumen.

Para mantener el taller centrado, crearás dos PR: una para las valoraciones por estrellas y otra para el filtrado con las actualizaciones de instrucciones y de la aptitud, el perfil de QA y las pruebas. Empieza cada una desde `main` actualizada. El flujo de filtrado y calidad comparte una sesión, un worktree y una rama para que puedas aprovechar el trabajo realizado mientras exploras cada herramienta. El ejercicio final del lienzo permanece en su sesión para que puedas centrarte en crear y probar la superficie compartida, en vez de repetir el flujo de PR.

## Lecciones

| Lección | Tema | Descripción |
|--------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Instala Node.js y crea tu copia del proyecto Tailspin Toys |
| [1. Instalar la aplicación Copilot][ex1] | Configuración | Instala la aplicación, conecta el proyecto y familiarízate con el espacio de trabajo |
| [2. Añadir valoraciones por estrellas: una mejora rápida][ex2] | Primer cambio | Muestra las valoraciones existentes y la alternativa para `null`; después, fusiona la primera PR |
| [3. Modos de agente: Plan y Autopilot][ex3] | Modos de agente | Planifica la característica desde su incidencia, impleméntala con Autopilot y revísala en modo Interactive |
| [4. Guiar a Copilot con instrucciones personalizadas][ex4] | Contexto | Explora y actualiza las instrucciones y aplícalas al filtrado |
| [5. Personalizar y usar una aptitud de comprobaciones de calidad][ex5] | Comprobaciones repetibles | Explora la aptitud existente, cambia el formato del informe y ejecútala |
| [6. Validar la funcionalidad con MCP de Playwright][ex6] | Observación en el navegador | Configura MCP mediante Customize y examina el comportamiento del filtrado |
| [7. Crear y usar un agente de QA][ex7] | Requisitos y cobertura | Crea y selecciona un perfil especializado y reúne las pruebas de verificación finales |
| [8. Crear y fusionar la PR de la característica][ex8] | Revisión y fusión | Revisa el filtrado, las instrucciones, la aptitud, el perfil de QA y las pruebas; después, usa Agent Merge para la segunda PR |
| [9. Explorar y crear lienzos][ex9] | Colaboración | Usa Database Explorer y, después, crea y prueba un lienzo de triaje respaldado por el repositorio |
| [10. Repaso y siguientes pasos][ex10] | Resumen | Revisa el flujo de trabajo, los recursos creados y otros recursos |

## Requisitos previos

Antes de participar en este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Un ordenador con **macOS, Linux o Windows**
- [ ] [Git instalado][install-git] en tu equipo

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes con estatus verificado pueden obtener GitHub Copilot gratis mediante [GitHub Education][callout-student-plan-education]. El plan **Copilot Student** incluye el agente, MCP, la revisión de código y las funciones de Copilot CLI que se usan en este taller, por lo que permite completar todos los itinerarios.

> [!NOTE]
> Como la aplicación Copilot se ejecuta en tu propio equipo y no en un Codespace, el [ejercicio de requisitos previos][ex0] te guiará para instalar Node.js y crear una copia del proyecto antes de instalar la aplicación.

> [!NOTE]
> Si usas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva **GitHub Copilot app**. Es una directiva independiente de la directiva de Copilot CLI.

## Empieza

**[Empieza por los requisitos previos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
