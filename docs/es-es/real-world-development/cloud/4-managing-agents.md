---
title: "Ejercicio 4 - Supervisión y gestión de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos últimos ejercicios has pedido al agente de Copilot en la nube que se encargue de tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidad. Aunque el agente está diseñado para trabajar de forma asíncrona y autónoma, sigue siendo importante poder supervisar esas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente, incluida [la página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes que tienen solicitudes de incorporación de cambios (PR) abiertas. También puedes explorar las operaciones realizadas e incluso dirigir una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- dirigirás una sesión en curso para solicitar funcionalidad adicional.

## Situación

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo se dio cuenta de que también sería buen momento para añadir un modo claro. Como ya se estaba trabajando en actualizar el estilo del sitio y añadir la función para alternar modos, parecía lógico incluir esta mejora. Quieres dirigir el trabajo del agente para asegurarte de que añada tanto el modo claro como el de alto contraste.

## Revisa las tareas del agente de Copilot en la nube

Veamos el estado actual de todas las tareas asignadas al agente de Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código base.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso y revisa las acciones que ha realizado Copilot. Pueden incluir:
   - obtener una copia del código del repositorio.
   - crear el entorno de trabajo de Copilot.
   - configurar servidores MCP.
   - realizar varios pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos varían según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Fíjate también en el panel de solicitudes de incorporación de cambios (PR) de la derecha. Desde allí puedes ver la PR y los archivos modificados para seguir el trabajo.

## Dirige al agente en la nube

Ya has visto las tareas activas. Ahora pide a Copilot que añada un selector de modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que le haya puesto Copilot y del estado actual del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo notarás cuando aparezcan mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este mensaje:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la página de agentes, con el cuadro de diálogo para dirigir una sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el mensaje.
5. Observa cómo Copilot lo confirma y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot se ocupará de la tarea actualizada. Incorporará la nueva solicitud al flujo cuando termine el paso en el que estaba trabajando en el momento de enviar el mensaje.

Como en ocasiones anteriores, esto tardará varios minutos. Puedes aprovechar el tiempo para detenerte y reflexionar sobre todo lo que has aprendido y explorado.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro principal para supervisar y dirigir las tareas del agente de Copilot en la nube. Desde allí puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir tareas en curso para ajustar el alcance o proporcionar instrucciones adicionales.

Has explorado estos conceptos:

- exploraste el centro de control y la página de agentes para supervisar las tareas del agente en la nube.
- redirigiste una sesión en curso para solicitar funcionalidad adicional.

Mientras Copilot termina el trabajo sobre las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iteración sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
