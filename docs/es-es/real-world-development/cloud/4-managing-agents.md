---
title: "Ejercicio 4 - Supervisión y gestión de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores, has pedido al agente de Copilot en la nube que realice tres tareas diferentes para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente en la nube está diseñado para trabajar de forma autónoma y asíncrona, sigue siendo importante poder supervisar esas tareas.

Hay muchas herramientas para gestionar las tareas que se asignan al agente en la nube, entre ellas, la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes que tienen solicitudes de incorporación de cambios abiertas, consultar las operaciones que han realizado e incluso orientar una sesión que esté en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube;
- dirigirás una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de contraste alto, el equipo se dio cuenta de que también sería un buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo funciones para alternar los modos, parecía lógico incluir esta funcionalidad. Vamos a orientar el trabajo del agente para que añada tanto el modo claro como el de contraste alto.

## Revisa las tareas del agente de Copilot en la nube

Veamos el estado actual de todas las tareas asignadas al agente de Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas, tanto en el panel principal como en el panel de la izquierda. Deberías ver las tareas asignadas a Copilot, entre ellas:
    - actualizar la documentación del código base;
    - generar API para modificar juegos;
    - añadir un modo de contraste alto al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones que ha realizado Copilot, que pueden incluir:
    - descargar el código del repositorio;
    - crear el entorno de trabajo de Copilot;
    - configurar servidores MCP;
    - realizar varios pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos exactos que aparecen dependerán del estado del trabajo de Copilot y del enfoque que haya utilizado.

4. Fíjate también en el panel de solicitudes de incorporación de cambios, que aparece a la derecha. Desde ahí puedes consultar la solicitud y los archivos modificados para seguir el trabajo.

## Dirige al agente en la nube

Ya has visto las tareas activas. Ahora vamos a pedir a Copilot que añada el modo claro mientras trabaja en el modo de contraste alto.

1. Selecciona la sesión relacionada con el modo de contraste alto. El título exacto dependerá del nombre que haya elegido Copilot y del estado actual del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Verás mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este mensaje:

    ```plaintext
    Mientras trabajamos en un modo de contraste alto, añadamos también un modo claro. Debe haber un control para este modo que permita a los usuarios seleccionar el modo de visualización que prefieran.
    ```

    ![Captura de pantalla de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo Steer active session while Copilot is working resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el mensaje.
5. Observa que Copilot lo reconoce y lo incorpora a su flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud a su flujo de trabajo cuando termine el paso en el que estaba trabajando al recibir el mensaje.

El trabajo también tardará varios minutos. Es un buen momento para hacer una pausa y repasar todo lo que has aprendido y explorado hasta ahora.

## Resumen y pasos siguientes

En esta lección has explorado la página de agentes de Copilot, el centro principal para supervisar y orientar las tareas del agente de GitHub Copilot en la nube. Puedes realizar el seguimiento de todas las tareas activas y completadas, revisar el trabajo que se está llevando a cabo e incluso redirigir tareas en curso para ajustar su alcance u ofrecer indicaciones adicionales.

Has explorado estos conceptos:

- el centro de control y la página de agentes para supervisar las tareas del agente en la nube;
- cómo redirigir una sesión en curso para solicitar una funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

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
