---
title: "Lección 6 - Validar la funcionalidad con MCP de Playwright"
description: "Configura MCP de Playwright mediante Customize y observa el filtrado en un navegador desde el worktree existente de la funcionalidad."
authors:
  - geektrainer
lastUpdated: 2026-07-09
---

Como ya hemos destacado, escribir código implica mucho más que limitarse a escribirlo. Necesitamos trabajar con datos y servicios externos y permitir que Copilot disponga de automatizaciones adicionales. Aquí es donde entran en juego los servidores MCP. Permiten a Copilot ir más allá de las funciones integradas en la aplicación y acceder a más herramientas y servicios.

En esta lección:

- comprenderás qué es el Model Context Protocol (MCP) y cómo lo utiliza la aplicación GitHub Copilot.
- añadirás el servidor MCP de Playwright.
- pedirás al agente que controle un navegador y explore la característica de filtrado.

## Escenario

Aunque las pruebas unitarias y de extremo a extremo son importantes, para validar las actualizaciones de la interfaz de usuario hay que interactuar con ella. Quieres que Copilot pueda utilizar el sitio web como lo haría una persona y automatizar más tareas, para tener mayor confianza en que las actualizaciones funcionan según lo previsto.

## ¿Qué es el Model Context Protocol (MCP)?

El [Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real. Así pueden acceder a información actualizada mediante recursos y realizar acciones en tu nombre mediante herramientas.

Se accede a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y los servicios externos. El servidor gestiona la comunicación con herramientas externas, como API existentes o herramientas locales en paquetes npm. Cada servidor ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP conocidos:

- **[Servidor MCP de GitHub](https://github.com/github/github-mcp-server)**: proporciona acceso a API para administrar repositorios, incidencias y solicitudes de incorporación de cambios. El agente puede crear repositorios, actualizar los existentes y administrar incidencias y solicitudes.
- **[Servidor MCP de Playwright][playwright-mcp-server]**: proporciona funciones de automatización del navegador mediante Playwright. El agente puede navegar por páginas web, rellenar formularios y seleccionar botones.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP](https://github.com/mcp) para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, comprueba quién lo publica y considera las implicaciones de seguridad. Usa únicamente servidores MCP de confianza y ten cuidado al conceder acceso a recursos o acciones sensibles.

## Añade el servidor MCP de Playwright

Los servidores MCP se administran desde **Customize** en la barra lateral. Es posible que los servidores configurados para tus repositorios o Copilot CLI ya estén disponibles en la aplicación, así que compruébalo antes de añadir otro. La [documentación sobre la personalización de la aplicación][customize-app] explica las opciones disponibles.

1. Selecciona **Customize** en la barra lateral.
2. Abre el menú desplegable **All types**, selecciona **MCP** y, después, comprueba en **Installed** si ya hay un servidor de Playwright.
3. Si Playwright aún no está instalado, búscalo entre los servidores disponibles y selecciona **Add server**.
4. Revisa quién publica el servidor, su configuración y cualquier solicitud de instalación antes de aprobarlas. Sigue las instrucciones para añadirlo; las directivas de la organización o la falta de requisitos previos pueden bloquear la configuración.
5. Vuelve a la sesión de filtrado en modo **Interactive** y confirma que están disponibles las herramientas MCP de Playwright.

Si la configuración falla, resuelve el problema de configuración o permisos antes de continuar.

## Pide a Copilot que explore la característica con Playwright

La incidencia y las decisiones de planificación ya están en el contexto. Detén cualquier servidor de desarrollo que hayas iniciado antes de pedir a Copilot que inicie uno.

1. Envía este mensaje para pedir a Copilot que valide la nueva funcionalidad:

   ```plaintext
   Inicia la aplicación y usa MCP de Playwright para comprobar el filtrado según la incidencia y el plan. Dime qué funciona y qué no, sin hacer cambios. Cuando termines, detén el servidor que has iniciado.
   ```

> [!NOTE]
> No es obligatorio decirle a Copilot qué servidor MCP debe usar; normalmente encuentra el adecuado según el contexto actual. Aun así, conviene indicarle lo que consideras importante.

2. Observa cómo trabaja.

Copilot iniciará el servidor, abrirá un navegador e interactuará con el sitio. Cuando termine, detendrá el servidor y te entregará un informe.

## Resumen y siguientes pasos

¡Enhorabuena! Has usado el servidor MCP de Playwright para explorar la característica en un navegador real desde la aplicación GitHub Copilot. En esta lección:

- has aprendido qué es el Model Context Protocol (MCP) y cómo lo utiliza la aplicación GitHub Copilot.
- has añadido el servidor MCP de Playwright.
- has pedido al agente que controle un navegador y explore la característica de filtrado.

A continuación, [crearás un agente personalizado de control de calidad][next-lesson] que combinará la aptitud y las herramientas del navegador en un rol especializado.

## Recursos

- [Qué es MCP y por qué habla todo el mundo de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Configurar servidores MCP en la aplicación GitHub Copilot][customize-app]

[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[customize-app]: https://docs.github.com/copilot/how-tos/github-copilot-app/customize-github-copilot-app
