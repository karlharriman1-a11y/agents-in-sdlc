---
title: "Ejercicio 5 - Supervisión y gestión de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando activas el modo agente de GitHub Copilot, trabaja de forma autónoma: explora la base de código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo se realiza en tu equipo y en tiempo real, Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición conforme se producen. Puedes revisar los diff en línea, aceptar o rechazar cambios individuales y orientar la conversación con solicitudes de seguimiento para perfeccionar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás las ediciones propuestas en los archivos;
- orientarás al agente durante la sesión para añadir un control del modo claro junto al modo de alto contraste.

## Situación

El agente personalizado de accesibilidad que utilizaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha solicitado también un control del modo claro para quienes consideran demasiado oscuro el tema predeterminado. En vez de iniciar una conversación desde cero, orientarás la sesión existente para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada modificación aparece en el chat con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver sus llamadas a herramientas, lecturas y ediciones de archivos, y los comandos de terminal que solicita ejecutar.
3. Cuando el agente proponga una edición, el diff aparecerá en línea:
    - Selecciona **Keep** para aceptar el cambio.
    - Selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos que ha modificado durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta requiere permiso (por ejemplo, para ejecutar un comando del terminal), el agente se detendrá y mostrará la solicitud **Allow**. Aprueba solo los comandos que consideres adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final que resuma los cambios.

## Orienta al agente para añadir el modo claro

Ahora que el modo de alto contraste está implementado, ampliarás la misma conversación para añadir un control del modo claro. Al continuar en la misma sesión de chat, el agente conserva el contexto del trabajo anterior: los componentes que modificó, el enfoque de estilos y el patrón de almacenamiento local.

1. En el cuadro de entrada de Copilot Chat, envía esta solicitud de seguimiento:

    ```plaintext
    Buen trabajo. Ahora añade un control del modo claro junto al control de alto contraste. El modo claro debe seguir el mismo patrón: conservarse en el almacenamiento local y estar disponible desde la misma zona de la interfaz de usuario (UI). El usuario debe poder activar el modo claro y el de alto contraste de forma independiente.
    ```

2. El agente retomará el trabajo, explorará los cambios ya aplicados y propondrá añadir el modo claro.
3. Revisa cada cambio nuevo conforme aparezca, como hiciste con el modo de alto contraste. Utiliza **Keep** o **Undo** en cada diff.
4. Si el agente toma un rumbo que no te convence (por ejemplo, intenta refactorizar los controles de una forma que no te gusta), detenlo con el botón **Stop** junto al cuadro de entrada y envía una aclaración.

> [!TIP]
> Orientar una sesión en curso es más rápido que empezar de nuevo. Utiliza solicitudes de seguimiento para pedir ajustes («simplifica las hojas de estilo en cascada (CSS) y utiliza variables CSS en lugar de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para los dos controles»).

## Revisa los archivos modificados

Antes de confirmar el trabajo, revisa rápidamente todo lo que haya modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos modificados y revisa los diff. Comprueba que se aplican los patrones de accesibilidad del agente personalizado: atributos de Accessible Rich Internet Applications (ARIA), navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has utilizado el modo agente de Copilot Chat para supervisar y orientar una sesión en curso: has revisado cada edición, aceptado o rechazado cambios y enviado solicitudes de seguimiento para ampliar el trabajo sin perder contexto.

En esta lección has explorado:

- la supervisión del trabajo de un agente en tiempo real;
- la aceptación y el rechazo de ediciones individuales;
- la orientación de una sesión en curso mediante solicitudes de seguimiento.

En [la siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una pull request y revisarás los cambios generados por el agente local personalizado durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
