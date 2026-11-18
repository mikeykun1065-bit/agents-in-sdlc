---
title: "Lección 2 - Añade valoraciones por estrellas: una mejora rápida"
description: "Usa Copilot CLI para hacer un pequeño cambio en las tarjetas de juegos, revísalo en un navegador reenviado y fusiónalo como tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ahora que has instalado Copilot CLI y has probado una conversación, es el momento de hacer el primer cambio en el proyecto. Será un cambio pequeño: los juegos ya tienen una valoración por estrellas en sus datos, pero las tarjetas de juegos de la página de inicio todavía no la muestran. Pedirás al agente que la muestre, revisarás el cambio y lo fusionarás como tu primera solicitud de incorporación de cambios.

En esta lección:

- iniciarás una conversación de Copilot centrada en una funcionalidad, desde una rama.
- pedirás al agente que realice un cambio pequeño en el proyecto.
- revisarás el cambio con `/diff`.
- ejecutarás la aplicación para comprobar el cambio en un navegador reenviado.
- abrirás y fusionarás tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración por estrellas, que ya aparece en la página de detalles del juego. Sin embargo, las tarjetas de juegos de la página de inicio solo muestran el título, la categoría, el editor y la descripción. Para empezar, pedirás al agente que muestre la valoración existente en cada tarjeta. Es un cambio pequeño y autocontenido, perfecto para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI utiliza el repositorio y la rama de Git que están activos en el terminal, en lugar de crear un árbol de trabajo específico. Puedes volver a las conversaciones guardadas más adelante; los archivos y la rama siguen siendo el estado habitual de Git en el disco.

En una conversación verás tres elementos: tus prompts y las respuestas del agente, la actividad de las herramientas mientras explora y edita archivos, y los cambios que puedes examinar con `/diff`.

## Inicia una conversación y solicita el cambio

Vamos a iniciar una conversación nueva para implementar la función.

1. Vuelve al codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si Copilot todavía no está abierto, ejecuta este comando:

   ```bash
   copilot --yolo
   ```

4. Asegúrate de iniciar una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Utiliza el siguiente prompt para solicitar el cambio:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot explora el proyecto, localiza los archivos que muestran los detalles de los juegos y crea el código necesario. Acabas de añadir una nueva función con Copilot CLI.

## Revisa el diff

Todos los cambios generados por IA deben revisarse antes de fusionarlos, incluso los más pequeños. Vamos a explorar los cambios directamente en Copilot CLI.

1. Escribe `/diff` y revisa todos los archivos modificados.
2. Confirma que la tarjeta muestra la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones que ha ejecutado Copilot y pídele que corrija los errores.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista del diff.

> [!NOTE]
> Como Copilot, al igual que todas las herramientas de IA generativa, es probabilístico y no determinista, el código exacto puede variar. Revisa el comportamiento en lugar de esperar una implementación idéntica.

## Comprueba los cambios

Por supuesto, no basta con leer el código y dar por hecho que funciona. Vamos a pedir a Copilot que inicie el sitio web para poder examinar la interfaz actualizada en el navegador reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas de juegos muestran su valoración sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que ha iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abre y fusiona tu primera solicitud de incorporación de cambios

Ya has creado la función. Es el momento de crear una solicitud de incorporación de cambios (PR) para fusionar el código nuevo en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Escribe `/pr create`. Copilot CLI subirá la confirmación existente al crear la PR y mostrará la URL.
3. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la URL que muestra Copilot CLI para abrir la PR.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté lista, selecciona **Merge pull request** y confirma la fusión.
6. Vuelve al codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y pasos siguientes

Enhorabuena, has publicado tu primer cambio con GitHub Copilot CLI. En concreto:

- has iniciado una conversación de Copilot centrada en una funcionalidad, desde una rama.
- has pedido al agente que realice un pequeño cambio en las tarjetas de juegos.
- has revisado el cambio con `/diff`.
- has ejecutado la aplicación y comprobado la valoración por estrellas en un navegador reenviado.
- has abierto y fusionado tu primera solicitud de incorporación de cambios.

A continuación, [empezarás con la incidencia de filtrado y utilizarás los modos Plan y Autopilot][next-lesson] para crear una función más amplia.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
