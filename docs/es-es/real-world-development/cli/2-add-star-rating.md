---
title: "Lección 2 - Añade valoraciones con estrellas: una mejora rápida"
description: "Utiliza Copilot CLI para modificar las tarjetas de juego, revisa el cambio en un navegador reenviado y combínalo en tu primera pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has instalado la interfaz de línea de comandos (CLI) de GitHub Copilot y probado una conversación. Ahora harás tu primer cambio en el proyecto. Será pequeño: los datos de los juegos ya incluyen una valoración con estrellas, pero las tarjetas de la página de inicio aún no la muestran. Pedirás al agente que la muestre, revisarás el cambio y lo combinarás en tu primera pull request (PR).

En esta lección:

- iniciarás una conversación centrada en una rama de funcionalidad;
- pedirás al agente un pequeño cambio en el proyecto;
- revisarás el cambio con `/diff`;
- ejecutarás la aplicación para confirmar el cambio en un navegador reenviado;
- abrirás y combinarás tu primera pull request.

## Situación

Cada juego de Tailspin Toys puede tener una valoración con estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas de la página de inicio solo muestran el título, la categoría, la editorial y la descripción. Como ejercicio inicial, pedirás al agente que muestre en cada tarjeta la valoración existente: un cambio pequeño y acotado, ideal para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI utiliza el repositorio y la rama de Git que estén seleccionados en el terminal, en lugar de crear un árbol de trabajo independiente. Las conversaciones guardadas te permiten volver a la misma conversación; los archivos y la rama siguen siendo el estado normal de Git en el disco.

En una conversación verás tres elementos: tus solicitudes y las respuestas del agente, la actividad de sus herramientas mientras explora y edita archivos, y los cambios que puedes inspeccionar con `/diff`.

## Inicia una conversación y solicita el cambio

Inicia una conversación nueva para comenzar a implementar la funcionalidad.

1. Vuelve a tu Codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Si Copilot no está iniciado, utiliza este comando:

   ```bash
   copilot --yolo
   ```

4. Asegúrate de iniciar una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Envía esta solicitud:

   ```plaintext
   Muestra `starRating` sobre 5 en las tarjetas de juego de la página de lista. Si la valoración es `null`, muestra "No rating yet". Conserva el diseño de las tarjetas, añade pruebas y ejecuta las comprobaciones pertinentes.
   ```

Copilot explorará el proyecto, localizará los archivos que muestran los detalles de los juegos y creará el código necesario. Ya has añadido una función con Copilot CLI.

## Revisa el diff

Hay que revisar todos los cambios generados por inteligencia artificial antes de combinarlos, incluso los más pequeños. Revisa el cambio desde Copilot CLI.

1. Escribe `/diff` e inspecciona todos los archivos modificados.
2. Comprueba que la tarjeta del juego muestra la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Comprueba que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones que haya ejecutado Copilot y pídele que corrija cualquier fallo.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la pantalla diff.

> [!NOTE]
> Copilot, como cualquier herramienta de inteligencia artificial generativa, es probabilístico, no determinista. El código exacto puede variar. Revisa el comportamiento, no esperes una implementación idéntica.

## Comprueba los cambios

No basta con leer el código y dar por hecho que funciona. Pide a Copilot que inicie el sitio web para comprobar la interfaz actualizada en el navegador reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda revisar en el navegador el cambio de la valoración con estrellas. Dime el localizador uniforme de recursos (URL) y deja el servidor en ejecución.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Comprueba que las tarjetas de juego muestran las valoraciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que ha iniciado:

   ```plaintext
   Detén el servidor de desarrollo que has iniciado.
   ```

## Abre y combina tu primera pull request

Ya has creado la funcionalidad. Ahora crearás una pull request (PR) para combinar el código con el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Confirma los cambios revisados de la valoración con estrellas con un mensaje de confirmación adecuado.
   ```

2. Escribe `/pr create`. Copilot CLI publicará la confirmación existente al crear la PR y mostrará su URL.
3. Abre la PR manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) mientras seleccionas la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la combinación.
6. Vuelve a tu Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y siguientes pasos

Enhorabuena, has publicado tu primer cambio con GitHub Copilot CLI. En concreto:

- has iniciado una conversación centrada en una rama de funcionalidad;
- has pedido al agente un pequeño cambio en las tarjetas de juego;
- has revisado el cambio con `/diff`;
- has ejecutado la aplicación para confirmar la valoración con estrellas en un navegador reenviado;
- has abierto y combinado tu primera pull request.

A continuación, [partirás de la incidencia de filtrado y utilizarás los modos Plan y Autopilot][next-lesson] para crear una funcionalidad más amplia.

## Recursos

- [Información sobre GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
