---
title: "Lección 8 - Crear y fusionar la PR de la característica"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la aptitud, el perfil de QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la aptitud, el perfil de control de calidad (QA) y las pruebas están guardados en una sola rama. Es el momento de revisarlos juntos y abrir una solicitud de incorporación de cambios. Ya fusionaste personalmente la PR de valoración por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la característica, las actualizaciones de instrucciones y de la aptitud y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sola sesión y rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una solicitud de incorporación de cambios.
- inspeccionarás la PR completa de la característica y las pruebas de verificación.
- autorizarás Agent Merge solo después de revisarla y confirmarás que la PR se ha fusionado.

## Escenario

A lo largo del flujo de trabajo de filtrado, has usado Copilot para planificar, implementar y verificar una característica. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener bajo el control del desarrollador la autorización para fusionarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Cuando lo activas, la sesión lee la solicitud y resuelve los impedimentos: corrige las comprobaciones fallidas de integración continua (CI), responde a comentarios de revisión y actualiza la rama cuando es necesario. La fusiona en cuanto GitHub lo permite. Se ejecuta en segundo plano, sigue funcionando aunque reinicies la aplicación y se desactiva cuando se fusiona la solicitud.

Hasta ahora has seleccionado tú **Merge pull request**. Agent Merge puede encargarse de esa responsabilidad, pero sus permisos para editar código y fusionar siguen necesitando tu autorización explícita. Revisa las acciones que se le permiten y el trabajo antes de concederle permiso para fusionar.

## Usa Agent Merge para gestionar la PR

Ya has creado y revisado todo el código. Ahora permite que Agent Merge gestione el proceso de la PR.

1. Usa el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

Se inicia el proceso de Agent Merge, que:

- crea la solicitud de incorporación de cambios con un título y una descripción.
- si has iniciado la sesión desde una incidencia, la menciona en el cuerpo de la descripción.
- actualiza la rama o gestiona los posibles conflictos con la rama de destino.
- supervisa la integración continua para comprobar que se superen todas las comprobaciones.
- supervisa los comentarios de otros desarrolladores y de la revisión de código de Copilot, y actualiza el código para resolverlos.
- puede fusionar automáticamente la PR cuando todo se haya completado correctamente.

Permite que Agent Merge también fusione la PR cuando se superen todas las comprobaciones.

5. Revisa las diferencias de la PR y las pruebas de verificación antes de conceder permiso para fusionarla.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Confirma que la opción **Merge pull request** está marcada.
8. Pide explícitamente al agente que fusione la solicitud:

   ```plaintext
   Fusiona la PR cuando se hayan superado todas las comprobaciones y aprobaciones obligatorias. No omitas las protecciones del repositorio.
   ```

9. Supervisa el progreso y resuelve los bloqueos que se indiquen. Antes de continuar, confirma que el estado de la PR es **Merged**; tener habilitado el permiso de fusión o un agente en ejecución no confirma que la PR se haya fusionado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la verificación, y ahora el proceso de solicitud de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una solicitud de incorporación de cambios.
- has inspeccionado la PR completa de la característica y las pruebas de verificación.
- has autorizado Agent Merge solo después de revisar los cambios y has confirmado que la PR se ha fusionado.

A continuación, [usarás un lienzo existente y crearás uno de triaje][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Administrar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
