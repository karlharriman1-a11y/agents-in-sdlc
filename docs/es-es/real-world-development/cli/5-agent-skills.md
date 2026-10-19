---
title: "Lección 5 - Personaliza y utiliza la habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato del informe y utilízala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste únicamente en escribir código. Ya hemos comprobado manualmente que el código funciona y hemos utilizado archivos de instrucciones para asegurarnos de que sigue nuestros estándares. Pero ¿qué hay de las pruebas, el lint y los demás aspectos de la integración continua (CI) con la interfaz de línea de comandos (CLI) de GitHub Copilot?

Para este tipo de tareas, las **habilidades de agente** son la opción adecuada. Las habilidades ayudan a Copilot a entender cómo ejecutar correctamente estas operaciones.

En esta lección:

- explorarás la habilidad existente `quality-checks`;
- personalizarás el formato de sus resultados;
- volverás a cargar la habilidad y la ejecutarás.

## Situación

Tailspin Toys utiliza la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos complementarios que el agente carga cuando los necesita. En esencia, son carpetas que llevan el nombre de la habilidad y contienen un archivo Markdown `SKILL.md`. Este archivo contiene frontmatter con un nombre y una descripción que definen la habilidad, una descripción general de su función y directrices sobre cuándo utilizarla. La carpeta también puede incluir subcarpetas con scripts y otros recursos.

> [!NOTE]
> No es necesario añadir otras carpetas ni archivos a una habilidad. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md`, ya que utiliza los comandos existentes del proyecto.

Las habilidades pueden estar en `.github/skills` del proyecto para que sean recursos del repositorio que el equipo pueda compartir y reutilizar, o en la carpeta de habilidades del usuario `~/.copilot/skills`.

## Explora la habilidad

Explora la habilidad `quality-checks` que ha creado el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee `name` y `description` al principio. La descripción ayuda a Copilot a determinar cuándo utilizar la habilidad.
3. Lee las instrucciones y observa cómo orientan a Copilot durante las pruebas y el lint.
4. Fíjate en que la habilidad aún no incluye una sección **Formato de los resultados**.

## Ejecuta la habilidad antes de modificarla

Puedes invocar las habilidades directamente desde Copilot CLI o mediante lenguaje natural. Pide a Copilot que ejecute las tres comprobaciones de la habilidad.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía esta solicitud:

   ```plaintext
   Ejecuta la habilidad quality-checks para las pruebas unitarias, el lint y las comprobaciones de tipos.
   ```

3. Lee el informe final.

## Personaliza el informe

Queremos mejorar el informe para que indique qué se ha ejecutado, si ha terminado correctamente y qué resultados han comunicado las herramientas. Actualiza la habilidad para que genere ese informe.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade esta sección al final del archivo:

   ```markdown
   ## Formato de los resultados

   Al finalizar, informa de cada comando ejecutado y de si se ha superado, ha fallado o ha quedado bloqueado. Incluye recuentos de pruebas, duraciones, errores, advertencias y otras métricas solo si las proporciona la herramienta. Indica el siguiente paso para cada fallo o bloqueo y nunca describas como superada una comprobación omitida o incompleta.
   ```

3. El archivo se guardará automáticamente.

## Ejecuta la habilidad actualizada

Ahora que has aplicado el cambio, comprueba el resultado. Copilot CLI puede volver a cargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía exactamente la misma solicitud que antes:

   ```plaintext
   Ejecuta la habilidad quality-checks para las pruebas unitarias, el lint y las comprobaciones de tipos.
   ```

3. Lee el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y utilizado una habilidad de agente existente. En esta lección:

- has explorado la habilidad `quality-checks` para las pruebas unitarias, el lint y las comprobaciones de tipos;
- has personalizado el formato de sus resultados;
- has vuelto a cargar y ejecutado la habilidad.

Ese cambio acompañará al filtrado en la pull request (PR) de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio mediante el servidor Playwright, que utiliza el Protocolo de contexto de modelo (Model Context Protocol, MCP), en la [siguiente lección][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad son referencias, no tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y un script de exportación incluido: `drawio`][drawio-example]
- [Pruebas en el navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
