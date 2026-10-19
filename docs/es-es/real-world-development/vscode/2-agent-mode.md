---
title: "Ejercicio 2 - Añade funcionalidad con el modo agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. Como desarrollador, normalmente tienes que localizar todos los archivos necesarios, aplicar cambios, ejecutar pruebas, depurar, averiguar qué archivo has pasado por alto y volver a actualizarlo. Y así sucesivamente.

Aquí es donde entra en juego el modo agente de Copilot.

El modo agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Actúa de forma parecida a un desarrollador: primero explora la estructura del proyecto, aplica los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que detecta. Veamos cómo utilizar el modo agente para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres sean parecidos, el modo agente y el agente en la nube ofrecen experiencias distintas. El modo agente trabaja en el IDE, donde permite recibir comentarios e interactuar con rapidez. El agente en la nube trabaja de forma asíncrona como un compañero del equipo y se comunica contigo mediante incidencias y pull requests.

En este ejercicio aprenderás a:

- utilizar el modo agente de Copilot para explorar el proyecto, identificar los archivos pertinentes y coordinar cambios;
- implementar funcionalidades en la interfaz y en la capa de datos con el modo agente de GitHub Copilot;
- revisar los cambios y las pruebas generados por el modo agente antes de combinar el código.

## Situación

A medida que crece la lista de juegos, quieres que los usuarios puedan filtrar por categoría y editorial. En el ejercicio anterior añadiste un ayudante de editoriales; ahora completarás el resto del trabajo en la capa de datos, la interfaz y las pruebas con el modo agente de Copilot.

## Ejecuta el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web de Tailspin Toys para entender sus funciones actuales.

El sitio es una plataforma de financiación colectiva para juegos de mesa con temática de desarrollo de software. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan una base de datos SQLite local desde el frontmatter mediante Drizzle ORM; no hay una interfaz de programación de aplicaciones (API) de backend independiente ni un framework de interfaz en el cliente. Los ayudantes reutilizables de acceso a datos están en `src/lib/`, y la interactividad se añade con pequeños elementos `<script>` de Astro que utilizan las API estándar del modelo de objetos del documento (DOM).

### Inicia el sitio web

Para facilitar la ejecución del sitio, se ha proporcionado la tarea `npm run dev`, que inicia el servidor de desarrollo de Astro. Sigue estos pasos desde el codespace de GitHub:

1. Vuelve al codespace. Deberías estar en `main` después del ejercicio anterior.
2. Abre un terminal en el codespace con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta el siguiente comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, verás un mensaje con la dirección, similar a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pregunta si quieres abrir una ventana del navegador en `http://localhost:4321`, ciérralo con **x**.

5. Abre el sitio web manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace de localhost desde el terminal se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, que ahora aloja el servidor web.

### Explora el sitio web

Con el sitio en ejecución, explora sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre su página de detalles con información como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando corresponda.

La implementación inicial del sitio funciona, pero queremos añadir nuevas capacidades. Empecemos revisando el backlog. Al crear el proyecto a partir de la plantilla, se generó automáticamente un backlog de incidencias; pide a GitHub Copilot que te muestre esos elementos.

1. En el desplegable de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos del terminal e invoca herramientas.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no recomiendan un modelo por encima de otro. Durante su creación se utilizó Claude Sonnet 4.5 para que las sugerencias de código fueran relativamente coherentes y la experiencia fuera adecuada. Sin embargo, como los modelos de lenguaje de gran tamaño (LLM) son probabilísticos, las sugerencias pueden ser distintas de las que se muestran en el taller. Es algo normal y esperado.

> [!NOTE]
> Debido a la naturaleza probabilística de los modelos de lenguaje de gran tamaño (LLM), Copilot podría utilizar un comando del Protocolo de contexto de modelo (Model Context Protocol, MCP) distinto, pero debería poder completar la tarea.

3. Envía a Copilot esta solicitud sobre el backlog de incidencias:

   ```plaintext
   Muéstrame el backlog de elementos de mi repositorio de GitHub. Ayúdame a priorizarlos según cuáles serán más útiles para los usuarios.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista de incidencias generada.

Observa que Copilot también ha priorizado los elementos según lo que considera más útil para el usuario.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que utilizarás para proporcionar contexto a Copilot. Utilizarás el archivo de la [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo abordar los cambios de funcionalidad del sitio.

1. En el codespace, ve a `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funcionalidad, entre ellas:
   - una descripción de la arquitectura;
   - principios para el diseño de componentes, la capacidad de probarlos y la accesibilidad;
   - enlaces a archivos de instrucciones específicos para distintos tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones permiten hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así puedes reutilizar instrucciones, dividir conjuntos complejos en archivos más pequeños y manejables, y proporcionar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado, hay que realizar al menos tres cambios distintos en la aplicación:

- añadir o perfeccionar la lógica restante de filtrado en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento de filtrado;
- actualizar la página de lista de juegos para añadir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios en el código. El modo agente de Copilot puede realizar estas tareas.

1. Continúa en la conversación actual con Copilot o inicia otra con **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de la selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** siga seleccionado en el desplegable de agentes de la vista Chat.

   ![Captura del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** siga seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada del backlog:

   ```plaintext
   Actualiza el sitio para incluir el filtrado por editorial y categoría según los requisitos de la incidencia correspondiente de GitHub en el backlog. Ya existe un ayudante de editoriales del ejercicio anterior, así que conserva y perfecciona ese trabajo según sea necesario mientras completas el resto de los cambios en la capa de datos, la interfaz de usuario y las pruebas. Antes de terminar, comprueba que se superan todas las pruebas. El servidor ya está en ejecución, así que no tienes que iniciarlo.
   ```

6. Observa cómo Copilot explora el proyecto y busca los archivos relacionados con la funcionalidad, incluidos los ayudantes de la capa de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Copilot explora los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas: explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos, según la complejidad de la tarea y del código. Mientras tanto, quizá aparezcan los botones **Keep** y **Undo** en el editor. Cuando termine, podrás aceptar o deshacer todos los cambios; no hace falta que selecciones esas opciones mientras el agente sigue trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Cuadro de diálogo de Copilot Chat que pregunta si se pueden ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea sin detectar errores.

   ![Sesión completa de Copilot Chat con el modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y busca posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o por cualquier otra herramienta de inteligencia artificial.

10. Vuelve al navegador donde está abierto el sitio web y explora la nueva funcionalidad.
11. Cuando hayas comprobado que todo funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y siguientes pasos

En este ejercicio has explorado cómo utilizar el modo agente de GitHub Copilot para añadir capacidades al sitio web de Tailspin Toys. Has aprendido a:

- implementar nuevas funciones en la interfaz y en la capa de datos con el modo agente de GitHub Copilot;
- utilizar el modo agente para explorar el proyecto, identificar los archivos pertinentes y coordinar cambios;
- revisar los cambios y las pruebas generados por el modo agente antes de combinar el código.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una pull request.

### Ejercicio adicional: implementa la paginación

A medida que crezca la lista de juegos, será necesario añadir paginación. Utiliza las habilidades de este ejercicio y pide a Copilot que implemente la paginación en el sitio. Al preparar la solicitud:

- sigue las buenas prácticas existentes, incluidos los archivos de instrucciones;
- decide cómo implementar la paginación y si el usuario podrá seleccionar el tamaño de página o este será fijo;
- proporciona a Copilot las directrices necesarias para que la implementación se ajuste a tus preferencias;
- itera con GitHub Copilot, solicita cambios y proporciona contexto cuando sea necesario. Este es el flujo habitual de trabajo con Copilot.

## Recursos

- [Modos de Copilot: ask, edit y agent; qué hacen y cuándo utilizarlos][choose-mode]
- [Modo agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
