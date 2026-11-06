---
title: "Lección 5 - Personaliza y usa una aptitud de comprobaciones de calidad"
description: "Explora la aptitud existente quality-checks, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste únicamente en escribir código. Ya hemos comprobado manualmente que funciona y hemos usado archivos de instrucciones para garantizar que siga nuestros estándares. Pero ¿qué ocurre con las pruebas, el lint y el resto de la integración continua (CI)?

Para este tipo de tareas, las **aptitudes de agente** son la mejor opción. Las aptitudes ayudan a Copilot a entender cómo ejecutar operaciones como estas.

En esta lección:

- explorarás la aptitud existente `quality-checks`.
- personalizarás el formato de sus resultados.
- ejecutarás la aptitud y revisarás su salida.

## Escenario

Tailspin Toys usa la aptitud `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las aptitudes de agente reúnen instrucciones reutilizables para tareas, scripts ejecutables y recursos auxiliares que un agente carga cuando los necesita. En esencia, son una carpeta con el nombre de la aptitud y un archivo Markdown llamado `SKILL.md`. El archivo contiene metadatos de frontmatter con un nombre y una descripción que definen la aptitud, una introducción sobre su función e indicaciones sobre cuándo debe utilizarse. La carpeta también puede incluir subcarpetas con scripts y otros recursos que la aptitud necesite.

> [!NOTE]
> Las aptitudes no necesitan carpetas ni archivos adicionales. En este ejemplo, la aptitud ejecutará comandos `npm` para realizar las pruebas y el lint, así que no hacen falta archivos auxiliares.

Las aptitudes pueden estar en la carpeta `.github/skills` del proyecto para compartirlas y reutilizarlas en el equipo, o en la carpeta raíz de Copilot, que suele ser `~/.copilot/skills`.

## Explora la aptitud

Explora la aptitud `quality-checks` que ha creado el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Si todavía no tienes abierto un lienzo **Files**, selecciona **+** en el panel de revisión y, después, **File**.
2. Busca `.github/skills/quality-checks/SKILL.md`.
3. Lee `name` y `description` al principio. Observa que la descripción ayuda a Copilot a comprender cuándo debe invocar la aptitud.
4. Lee las instrucciones y fíjate en cómo guían a Copilot durante las pruebas y el lint.

## Ejecuta la aptitud antes de hacer cambios

Puedes llamar a las aptitudes directamente mediante un comando con barra diagonal (`/`) o mediante lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones de la aptitud.

1. Selecciona **Interactive** en el menú desplegable de modo para confirmar que Copilot lo utiliza.
2. Usa este mensaje para invocar la aptitud:

   ```plaintext
   Ejecuta la aptitud quality-checks para realizar las pruebas unitarias, el lint y las comprobaciones de tipos.
   ```

3. Lee el informe final.

## Personaliza el informe

Queremos un informe mejor que indique qué se ha ejecutado, si se ha superado y qué han comunicado realmente las herramientas. Actualiza la aptitud para que genere ese informe.

1. Vuelve al lienzo **Files**.
2. Si aún no está abierto, abre `.github/skills/quality-checks/SKILL.md`.
3. Añade esta sección al final del archivo:

   ```markdown
   ## Formato de salida de los resultados

   Al terminar, indica cada comando ejecutado y si se ha superado, ha fallado o ha quedado bloqueado. Incluye el recuento de pruebas, la duración, los errores, las advertencias y otras métricas solo si la herramienta los proporciona. Indica el siguiente paso ante cualquier error o bloqueo, y nunca describas una comprobación omitida o incompleta como superada.
   ```

El archivo se guardará automáticamente.

## Ejecuta la aptitud actualizada

Ahora que has actualizado la aptitud, comprueba el resultado. Usaremos exactamente el mismo mensaje que antes.

1. Selecciona **Interactive** en el menú desplegable de modo para confirmar que Copilot lo utiliza.
2. Usa este mensaje para invocar la aptitud:

   ```plaintext
   Ejecuta la aptitud quality-checks para realizar las pruebas unitarias, el lint y las comprobaciones de tipos.
   ```

3. Lee el informe final.

## Resumen y siguientes pasos

Has personalizado y usado una aptitud de agente existente. En esta lección:

- has explorado la aptitud `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- has personalizado el formato de sus resultados.
- has ejecutado la aptitud y revisado su salida.

Este cambio acompañará al filtrado en la PR de la característica. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de aptitudes

Estos ejemplos de la comunidad sirven de referencia y no son tareas adicionales. Revisa sus requisitos previos y su comportamiento antes de adoptarlos:

- [Especificación de Agent Skills][skill-spec].
- [Flujo de contribución: `make-repo-contribution`][contribution-example].
- [Documentos de requisitos: `prd`][prd-example].
- [Diagramas y script de exportación integrado: `drawio`][drawio-example].
- [Pruebas de navegador: `webapp-testing`][browser-example].

El ejemplo original de contribución se llama `make-repo-contribution`; las plantillas antiguas de Tailspin Toys usaban otro nombre, `make-contribution`. Este taller no depende de ninguna de las dos aptitudes de contribución.

[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
