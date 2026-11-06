---
title: "Lección 7 - Crea y usa un agente de control de calidad"
description: "Crea un agente personalizado de control de calidad que combine los requisitos de la incidencia, la aptitud quality-checks y MCP de Playwright."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la aptitud `quality-checks` para ejecutar comprobaciones automatizadas y MCP de Playwright para observar en un navegador cómo funciona el filtrado. Ahora combinarás esas capacidades en un agente personalizado con un proceso de control de calidad claramente definido.

En esta lección:

- explorarás cómo trabaja un agente personalizado con instrucciones, aptitudes y herramientas MCP.
- crearás y revisarás un perfil reutilizable de control de calidad.
- seleccionarás el agente de control de calidad y revisarás sus conclusiones según la incidencia de filtrado.

## Escenario

Antes de abrir una solicitud de incorporación de cambios (PR), Tailspin Toys quiere revisar de forma coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de las pruebas y el comportamiento en el navegador. Un agente personalizado puede coordinar este proceso de control de calidad y generar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil Markdown. El perfil describe el propósito, las instrucciones y las herramientas disponibles para el agente. En este taller, definirás el rol de control de calidad en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones que has usado tienen funciones distintas. Las instrucciones del repositorio describen los estándares del equipo. La aptitud `quality-checks` agrupa comprobaciones repetibles. MCP de Playwright proporciona herramientas para el navegador. El perfil de control de calidad indica a Copilot cómo utilizar esas capacidades para evaluar los requisitos e informar de sus conclusiones. No reemplaza las otras personalizaciones ni requiere otra conversación.

## Crea el perfil de control de calidad

Antes de abrir la PR de la característica, pide a Copilot que cree un perfil reutilizable de control de calidad. El perfil definirá tanto las comprobaciones que debe realizar como los límites que debe respetar.

1. Vuelve a tu Codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el agente personalizado:

   ```plaintext
   Crea un agente personalizado llamado QA en .github/agents/qa.agent.md. Debe comprobar las características según sus incidencias y los requisitos acordados, seguir las instrucciones del repositorio, ejecutar la aptitud quality-checks, usar MCP de Playwright para verificar el comportamiento y añadir pruebas si falta cobertura.

   Debe indicar cada requisito como superado, fallido o bloqueado y aportar pruebas que lo respalden. Tiene que preguntar antes de modificar el código de implementación y no debe confirmar cambios ni abrir solicitudes de incorporación de cambios.

   Por ahora, crea solo el perfil para que pueda revisarlo.
   ```

## Inspecciona el perfil

Antes de usar el agente nuevo, revisa su perfil para confirmar que Copilot ha incorporado el proceso de control de calidad y los límites previstos. Así evitarás que un agente incompleto o demasiado permisivo modifique la característica cuando solo quieres verificarla.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, pero incluirlo da al agente un nombre claro en la interfaz.
3. Lee las instrucciones del perfil y confirma que el control de calidad parte de los requisitos, sigue las instrucciones del repositorio, ejecuta la aptitud `quality-checks` y usa MCP de Playwright.
4. Confirma que el agente presenta pruebas que respaldan sus conclusiones, pregunta antes de modificar el código de implementación y no confirma cambios ni abre solicitudes de incorporación de cambios.
5. Si el perfil generado omite alguna de estas responsabilidades o límites, pide al agente predeterminado que lo revise antes de continuar.

## Ejecuta el control de calidad según la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Después de crear el perfil, retoma la misma conversación de filtrado y selecciona QA para que pueda utilizar la incidencia y las decisiones de planificación que ya forman parte del contexto.

1. Activa el agente con este comando:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, es posible que no aparezca en la lista de agentes disponibles. Aunque no aparezca, el comando anterior lo activará.

2. Usa este mensaje para pedir al agente de control de calidad que revise la característica:

   ```plaintext
   Revisa la característica de filtrado según la incidencia y las decisiones del plan. ¿Está lista para una solicitud de incorporación de cambios?
   ```

3. El agente de control de calidad empezará a trabajar.
4. Cuando termine, lee el informe.

## Resumen y siguientes pasos

Has añadido un rol especializado reutilizable al flujo de trabajo y has revisado su trabajo. En esta lección:

- has explorado cómo trabaja un agente personalizado con instrucciones, aptitudes y herramientas MCP.
- has creado y revisado un perfil reutilizable de control de calidad.
- has seleccionado el agente de control de calidad y revisado sus conclusiones según la incidencia de filtrado.

Ya tienes la implementación, la actualización de la aptitud, el perfil de control de calidad, las pruebas y el informe de verificación listos para revisar. A continuación, [los reunirás en una PR de la característica y usarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
