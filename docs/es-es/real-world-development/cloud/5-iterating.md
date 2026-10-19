---
title: "Ejercicio 5: Iteración sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le pediste que añadiera documentación al código, creara una función de juegos relacionados para el equipo de diseño e implementara funciones de accesibilidad, incluidos controles para los modos de alto contraste y claro. Vamos a revisar los cambios de código y, si es necesario, a proporcionar comentarios a Copilot para mejorar el trabajo.

### Situación

Como ya hemos señalado varias veces, los fundamentos del diseño de software y DevOps no cambian al añadir inteligencia artificial generativa. Siempre debemos revisar el código generado y seguir el proceso habitual de DevOps. Por tanto, revisaremos las sugerencias de GitHub Copilot para la documentación, los juegos relacionados y las funciones de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube de Copilot trabaja de forma asíncrona y sin supervisión directa, se han establecido restricciones de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utiliza para el código.
- El agente en la nube se ejecuta en GitHub Actions, donde crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar cualquier workflow de GitHub Actions antes de que se ejecute.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores del Protocolo de contexto de modelo (Model Context Protocol, MCP).

## Revisa la documentación generada

Empieza por la primera pull request (PR) generada por el agente en la nube de GitHub Copilot: la que añade documentación al código. Utilizarás la interfaz habitual de las PR en GitHub.com.

> [!NOTE]
> Puede que veas un aviso de que el firewall ha bloqueado a GitHub Copilot. Es **lo esperado**, ya que el acceso a recursos externos, incluidas las llamadas a servidores MCP externos, está limitado de forma predeterminada. Si quieres, puedes [personalizar o deshabilitar el firewall del agente en la nube de Copilot][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de pull requests.
3. Abre la pull request cuyo título sea parecido a **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, la pull request incluirá la marca **[WIP]**. Espera a que termine. Puede tardar unos minutos; mientras tanto, puedes descansar o repasar lo que has aprendido.

4. Cuando la pull request esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye nuevos comentarios TSDoc y otra documentación. Los cambios concretos pueden variar.

    Al revisar los cambios, busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentario. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del ejercicio 2.

6. Cuando hayas revisado las actualizaciones y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que algunos workflows están pendientes de aprobación.
8. Si hay workflows pendientes, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Los workflows deberían añadirse a la cola de comprobaciones de la pull request. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían superarse. El proceso puede tardar unos minutos.

## Solicita cambios a GitHub Copilot

La colaboración con Copilot en una pull request no es un proceso unidireccional. Puedes mencionarlo en comentarios de la pull request o en comentarios insertados en el código, como harías con otros miembros del equipo. Copilot verá los comentarios e iniciará otra sesión para atenderlos. Como los resultados no son deterministas, no podemos prescribir un texto exacto. Algunas ideas de cambios que puedes pedirle:

- Añadir un encabezado con una breve descripción al principio de cada archivo de código.
- Añadir comentarios TSDoc a archivos TypeScript y Astro.
- Crear un archivo README que describa la estructura de la aplicación Astro.

1. Añade un comentario en el que solicites un cambio en la documentación generada y menciona a **@copilot** como harías con cualquier usuario. Puedes usar una de las ideas anteriores u otra sugerencia de documentación para el código.
2. Selecciona **View Session** para ver cómo trabaja Copilot. Observa que inicia una sesión nueva para aplicar los cambios.
3. Selecciona **Back to pull request** para volver a la pull request.

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot haya terminado, verás una confirmación nueva en la pull request.
5. Selecciona **Files changed** para revisar los cambios.

Puedes seguir iterando hasta quedar conforme. Cuando estés satisfecho, puedes marcar la PR como lista para revisión y combinarla con la rama principal.

![Convert PR to ready](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Vuelve a la PR que Copilot generó para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Show related games on the game details page**.
4. Selecciona **Files changed** para revisar el código generado.
5. Cuando hayas revisado las actualizaciones y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador de que algunos workflows están pendientes de aprobación.
7. Si hay workflows pendientes, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Los workflows deberían añadirse a la cola de comprobaciones de la pull request. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían superarse. El proceso puede tardar unos minutos.
9. **Opcional:** Puedes cambiar a esta rama en el codespace para probar manualmente los juegos relacionados. Ve al codespace, abre el terminal y ejecuta los siguientes comandos (sustituye `<branch-name>` por el nombre de la rama que creó Copilot, por ejemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes iterar con el agente en la nube para solicitar cambios, por ejemplo, modificar el número de juegos relacionados o añadir encabezados de comentario y comentarios TSDoc (recuerda que esta tarea se asignó **antes** de actualizar las instrucciones). Para ello, añade otro comentario en la pestaña **Conversation**. Copilot lo verá e iniciará otra sesión.

## Revisa las funciones de accesibilidad

Por último, revisa las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta PR debería incluir el modo de alto contraste que asignaste en el ejercicio 3 y el modo claro que solicitaste desde el centro de control en el ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título sea parecido a **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot aún está trabajando, la pull request incluirá la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** para revisar el código generado.
5. Revisa la implementación y presta atención a:
   - los componentes de interfaz para cambiar de modo;
   - el almacenamiento local que conserva las preferencias del usuario;
   - los cambios de hojas de estilo en cascada (CSS) para los modos de alto contraste y claro;
   - los atributos de accesibilidad (etiquetas de Accessible Rich Internet Applications (ARIA), navegación con teclado, etc.);
   - el código JavaScript o TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado las actualizaciones y estés conforme, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que algunos workflows están pendientes de aprobación.
8. Si hay workflows pendientes, selecciona **Approve and run workflows**.

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Los workflows deberían añadirse a la cola de comprobaciones de la pull request. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían superarse. El proceso puede tardar unos minutos.
10. **Opcional:** Puedes cambiar a esta rama en el codespace para probar manualmente las funciones de accesibilidad. Ve al codespace, abre el terminal y ejecuta los comandos siguientes (sustituye `<branch-name>` por el nombre de la rama que creó Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    A continuación, inicia la aplicación y prueba en el navegador los controles de los modos de alto contraste y claro. Comprueba que funcionan y que las preferencias se conservan al recargar la página.

Observa cómo el agente personalizado de accesibilidad ha ayudado a orientar a Copilot para implementar estas funciones siguiendo buenas prácticas de accesibilidad. Si detectas problemas o posibles mejoras, puedes mencionar a **@copilot** en un comentario para solicitar cambios, como hiciste con las otras PR.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil cuando puedes asignarle tareas reales del backlog y ocuparte de otras cosas. Para adquirir el hábito, registra algunas incidencias más en tu repositorio y asígnaselas a Copilot. Algunas ideas:

- Crear un formulario para que los usuarios indiquen su interés en un juego en la página de detalles.
- Implementar la paginación en la página de la lista de juegos.
- Añadir validación de entradas y gestión de errores a los ayudantes de acceso a datos.

## Resumen

Has completado el entorno del agente en la nube. En estas lecciones:

- **Has revisado los archivos de instrucciones personalizadas** que incluye el repositorio para ver su efecto posterior en el agente en la nube.
- **Has asignado incidencias al agente en la nube de Copilot** y has observado cómo configura el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y utilizado un agente personalizado** de accesibilidad para añadir controles de los modos de alto contraste y claro.
- **Has utilizado la página de agentes de Copilot como centro de control** para supervisar y orientar la sesión de accesibilidad en curso.
- **Has revisado e iterado sobre las pull requests del agente en la nube**, mencionando a `@copilot` para solicitar cambios y aprobando workflows.

## Revisión y siguientes pasos

Has completado el entorno del agente en la nube. Si quieres seguir explorando, los otros entornos complementan lo que has practicado aquí:

- 🖥️ **[Entorno de VS Code](../../vscode/)**: explora el modo agente de Copilot Chat y la integración MCP directamente desde el IDE.
- 💻 **[Entorno de CLI](../../cli/)**: entrega desde el terminal dos cambios revisados con los modos Plan y Autopilot, habilidades de agente, un agente de control de calidad (QA), Playwright MCP, Agent Merge y el puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente en la nube una incidencia de refactorización o de cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Información sobre los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el workflow de configuración del agente en la nube de Copilot][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
