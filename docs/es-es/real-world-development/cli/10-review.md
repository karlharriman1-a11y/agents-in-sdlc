---
title: "Lección 10 - Resumen y siguientes pasos"
description: "Repasa el flujo de trabajo de Copilot CLI, las dos pull requests, las personalizaciones reutilizables y los recursos adicionales."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has utilizado la interfaz de línea de comandos (CLI) de GitHub Copilot durante un flujo de trabajo continuo en Tailspin Toys. Has:

- preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado inicial;
- añadido valoraciones con estrellas, revisado el resultado en un navegador reenviado y combinado manualmente tu primera pull request (PR);
- partido de la incidencia de filtrado, definido el enfoque con el modo Plan, implementado la funcionalidad con Autopilot y revisado el resultado en modo Interactive;
- orientado al agente con instrucciones personalizadas, personalizado la habilidad existente `quality-checks` y la has utilizado para ejecutar pruebas unitarias, lint y comprobaciones de tipos;
- añadido el servidor del Protocolo de contexto de modelo (Model Context Protocol, MCP) de Playwright y lo has utilizado para explorar el filtrado en un navegador real;
- creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la habilidad y las pruebas en el navegador;
- revisado todos los cambios del filtrado y autorizado Agent Merge para la PR;
- explorado comandos de barra para el contexto, los modelos, el uso compartido y la delegación opcional en la nube.

## Qué has publicado

El taller tiene dos hitos de pull request, cada uno en su propia rama basada en la versión actualizada de `main`:

1. **Valoraciones con estrellas:** muestra en las tarjetas de juego el valor existente de `starRating` y un estado explícito para los juegos sin valoración.
2. **Filtrado y flujo de calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la funcionalidad, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la creación de su PR, has utilizado la misma conversación y rama. Reunimos todo el trabajo en una PR para simplificar el taller.

## Distintos tipos de verificación

Has comprobado el código de varias formas: con pruebas automatizadas, inspeccionándolo en el navegador y pidiendo a Copilot que explorara el navegador mediante MCP. La habilidad `quality-checks` ejecutó pruebas unitarias, lint y comprobaciones de tipos, y registró los resultados con el nuevo formato. QA reunió esos resultados con la revisión de requisitos y la cobertura de pruebas antes de crear la PR.

Las pruebas nuevas deben cubrir deficiencias reales; un análisis de QA que no necesite añadir pruebas puede ser correcto. Revisa el código y las pruebas antes de autorizar la combinación, y actualiza las pruebas afectadas después de aplicar cambios.

## Buenas prácticas

El contexto y las herramientas que proporcionas a Copilot influyen en su trabajo. En este taller has actualizado instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones en otras conversaciones y ajústalas cuando cambien las necesidades del equipo. Las instrucciones definen estándares, las habilidades describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados reales de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** adecuados para la tarea. Utiliza **Plan** para analizar el enfoque antes de empezar, **Interactive** para participar en cambios acotados y **Autopilot** para tareas bien definidas. Escoge un modelo más rápido para ediciones rutinarias y otro más capaz para trabajos complejos.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres que se cree, *por qué* y *cómo* cambia de forma significativa el resultado.

## Más opciones para explorar

Ya has completado el flujo principal. Aquí tienes algunas funciones más de CLI que puedes probar:

- `/review` para pedir al agente de revisión de código que analice los cambios;
- `/rubber-duck` para explicar un problema y obtener otra perspectiva;
- `/fleet` para coordinar subtareas independientes en paralelo;
- `/worktree` para aislar una tarea;
- `/delegate` para enviar una tarea al agente en la nube de Copilot.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir utilizándola. Úsala para código de producción, proyectos personales o esa pequeña aplicación que llevas años queriendo crear. Comparte lo que aprendas con tu equipo y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora los talleres de [VS Code][vscode], la [aplicación GitHub Copilot][app] o el [agente en la nube de Copilot][cloud].

## Recursos

- [Información sobre GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar pull requests con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
