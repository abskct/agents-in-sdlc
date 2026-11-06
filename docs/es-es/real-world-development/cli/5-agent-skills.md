---
title: "Lección 5 - Personaliza y usa una aptitud de comprobaciones de calidad"
description: "Explora la aptitud existente quality-checks, personaliza el formato del informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste únicamente en escribir código. Ya hemos comprobado manualmente que el código funciona y hemos usado archivos de instrucciones para garantizar que siga nuestros estándares. Pero ¿qué ocurre con las pruebas, el lint y el resto de la integración continua (CI)?

Para este tipo de tareas, las **aptitudes de agente** son la mejor opción. Las aptitudes ayudan a Copilot a entender cómo ejecutar operaciones como estas.

En esta lección:

- explorarás la aptitud existente `quality-checks`.
- personalizarás el formato de los resultados.
- volverás a cargar la aptitud y la ejecutarás.

## Escenario

Tailspin Toys usa la aptitud `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las aptitudes de agente reúnen instrucciones reutilizables para tareas, scripts ejecutables y recursos auxiliares que un agente carga cuando los necesita. En esencia, son una carpeta con el nombre de la aptitud y un archivo Markdown llamado `SKILL.md`. El archivo Markdown contiene metadatos de frontmatter con un nombre y una descripción que definen la aptitud, una introducción sobre su función e indicaciones sobre cuándo debe utilizarse. La carpeta también puede incluir subcarpetas con scripts y otros recursos que la aptitud necesite.

> [!NOTE]
> Las subcarpetas y los archivos adicionales son opcionales. La aptitud `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque utiliza los comandos existentes del proyecto.

Las aptitudes pueden estar en la carpeta `.github/skills` del proyecto para compartirlas y reutilizarlas en el equipo, o en la carpeta de aptitudes del usuario `~/.copilot/skills`.

## Explora la aptitud

Explora la aptitud `quality-checks` que ha creado el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a determinar cuándo debe usar la aptitud.
3. Lee las instrucciones y fíjate en cómo guían a Copilot durante las pruebas y el lint.
4. Observa que todavía no hay una sección **Results output formatting**.

## Ejecuta la aptitud antes de hacer cambios

Puedes llamar a las aptitudes directamente desde Copilot CLI o mediante lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones de la aptitud.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Usa este mensaje:

   ```plaintext
   Ejecuta la aptitud quality-checks para realizar las pruebas unitarias, el lint y las comprobaciones de tipos.
   ```

3. Lee el informe final.

## Personaliza el informe

Queremos un informe mejor que indique qué se ha ejecutado, si se ha superado o no, y qué resultados han devuelto realmente las herramientas. Actualiza la aptitud para que genere ese informe.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Formato de salida de los resultados

   Al terminar, indica cada comando ejecutado y si se ha superado, ha fallado o ha quedado bloqueado. Incluye el recuento de pruebas, la duración, los errores, las advertencias y otras métricas solo si la herramienta los proporciona. Indica el siguiente paso ante cualquier error o bloqueo, y nunca describas una comprobación omitida o incompleta como superada.
   ```

3. El archivo se guarda automáticamente.

## Ejecuta la aptitud actualizada

Ahora que has actualizado la aptitud, comprueba el resultado. Copilot CLI puede volver a cargar las aptitudes editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Usa exactamente el mismo mensaje que antes:

   ```plaintext
   Ejecuta la aptitud quality-checks para realizar las pruebas unitarias, el lint y las comprobaciones de tipos.
   ```

3. Lee el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y usado una aptitud de agente existente. En esta lección:

- has explorado la aptitud `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- has personalizado el formato de los resultados.
- has vuelto a cargar y ejecutado la aptitud.

Este cambio acompañará al filtrado en la solicitud de incorporación de cambios. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de aptitudes

Estos ejemplos de la comunidad sirven de referencia y no son tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación integrado: `drawio`][drawio-example]
- [Pruebas de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
