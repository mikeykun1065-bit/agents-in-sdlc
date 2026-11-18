---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido a GitHub Copilot cloud agent que se ocupe de tres tareas distintas relacionadas con la mejora de la experiencia de usuario y la incorporación de funcionalidades. Aunque el cloud agent está diseñado para trabajar de forma asíncrona y autónoma, sigue siendo importante poder supervisar estas tareas.

Hay varias herramientas para gestionar las tareas asignadas al cloud agent, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes que tienen solicitudes de incorporación de cambios abiertas. Puedes explorar las operaciones realizadas e incluso dirigir una sesión en curso para orientarla.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del cloud agent.
- dirigirás una sesión en curso para solicitar una funcionalidad adicional.

## Escenario

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo se dio cuenta de que también sería buen momento para añadir un modo claro. Como ya se estaba trabajando en actualizar el estilo del sitio y añadir una opción de alternancia, parecía lógico incluir esta funcionalidad. Quieres orientar el trabajo del agente para asegurarte de que también añada el modo claro.

## Revisa las tareas de GitHub Copilot cloud agent

Vamos a consultar el estado actual de todas las tareas asignadas a GitHub Copilot cloud agent.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
    - actualizar la documentación del código base.
    - generar API para modificar juegos.
    - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones realizadas por Copilot, que pueden incluir:
    - obtener el código del repositorio.
    - preparar el entorno para que trabaje Copilot.
    - configurar servidores MCP.
    - realizar varios pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos exactos que aparecen dependerán del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de solicitud de incorporación de cambios que aparece a la derecha. Desde ahí puedes consultar la solicitud y los archivos modificados para supervisar el trabajo.

## Dirige el cloud agent

Ahora que has visto las tareas activas, vamos a pedir a Copilot que añada la opción de modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que Copilot haya elegido y del estado actual del trabajo.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo sabrás cuando empieces a ver mensajes similares a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, añade este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de pantalla de la tarea del cloud agent en la página de agentes, con el cuadro de diálogo para dirigir una sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que lo ha recibido y lo incorpora a su flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud a su flujo cuando termine el paso en el que estaba trabajando al enviar el mensaje.

Al igual que antes, tardará varios minutos. Es un buen momento para hacer una pausa y reflexionar sobre todo lo que has aprendido y explorado hasta ahora.

## Resumen y pasos siguientes

En esta lección has explorado la página de agentes de Copilot, el centro desde el que se supervisan y dirigen las tareas de GitHub Copilot cloud agent. Puedes hacer seguimiento de todas las tareas activas y completadas, revisar el trabajo que se está realizando e incluso redirigir las tareas en curso para ajustar su alcance o proporcionar más indicaciones.

Has explorado estos conceptos:

- has explorado mission control y la página de agentes para supervisar las tareas del cloud agent.
- has redirigido una sesión en curso para solicitar funcionalidades adicionales.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
