---
title: "Ejercicio 4 - Supervisión y gestión de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente en la nube de Copilot que se ocupe de tres tareas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma autónoma y asíncrona, sigue siendo importante supervisar las tareas.

Tienes a tu disposición varias herramientas para gestionar las tareas asignadas al agente en la nube, como la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes que tienen pull requests (PR) abiertas. Puedes explorar las operaciones realizadas e incluso orientar una sesión que esté en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube;
- orientarás una sesión en curso para pedir funcionalidad adicional.

## Situación

Después de asignar al agente la creación de un modo de alto contraste, el equipo decidió que también sería oportuno añadir un modo claro. Como ya se estaban actualizando los estilos del sitio y añadiendo controles, parecía lógico incluir esta funcionalidad. Orientarás el trabajo del agente para que añada el modo claro además del alto contraste.

## Revisa las tareas del agente en la nube de Copilot

Consulta el estado actual de todas las tareas asignadas al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel izquierdo. Deberían aparecer las tareas asignadas a Copilot, entre ellas:
    - actualizar la documentación del código;
    - generar API para modificar juegos;
    - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en ejecución. Revisa las acciones que Copilot ha realizado. Entre ellas puede haber:
    - obtener el código del repositorio;
    - crear el entorno de trabajo de Copilot;
    - configurar servidores MCP;
    - realizar distintos pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos dependerán del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de pull requests (PR) que aparece a la derecha. Permite ver la PR y los archivos modificados para supervisar el trabajo.

## Orienta al agente en la nube

Ya has visto las tareas activas. Ahora pide a Copilot que incluya el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título concreto dependerá del nombre que haya utilizado Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos hasta que la sesión indique que ha terminado la configuración y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes similares a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe esta solicitud:

    ```plaintext
    Mientras trabajamos en el modo de alto contraste, añade también un modo claro. Debe haber un control para este modo que permita a los usuarios elegir el modo de visualización que prefieran.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes con el cuadro de diálogo de orientación resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar la solicitud.
5. Observa cómo Copilot la confirma y la incorpora al trabajo.

## Deja que Copilot trabaje

Como antes, Copilot se ocupará de la tarea actualizada. Incorporará la nueva solicitud al flujo de trabajo cuando termine el paso en el que estaba trabajando al recibir el mensaje.

El proceso tardará varios minutos, así que puedes aprovechar para repasar lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro de control desde el que puedes supervisar y orientar las tareas del agente en la nube de GitHub Copilot. Puedes hacer seguimiento de las tareas activas y completadas, revisar el trabajo y redirigir las sesiones en curso para ajustar el alcance o proporcionar indicaciones.

Has explorado estos conceptos:

- explorar el centro de control y la página de agentes para supervisar las tareas del agente en la nube;
- redirigir una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las pull requests que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Información sobre los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
