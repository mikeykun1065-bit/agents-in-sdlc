---
title: "Ejercicio 2 - Añade funciones con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. El flujo de trabajo de un desarrollador suele consistir en localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, realizar otra actualización... y así sucesivamente.

El modo de agente de Copilot está diseñado para encargarse de este trabajo.

El modo de agente de Copilot funciona de forma más autónoma en el IDE. Se comporta de manera parecida a un desarrollador: primero explora la estructura del proyecto, realiza las actualizaciones necesarias, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Vamos a explorar cómo puedes utilizar el modo de agente para añadir funciones al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el cloud agent ofrecen experiencias distintas. El modo de agente realiza las tareas en el IDE, lo que permite ciclos de respuesta rápida e interacción. El cloud agent está diseñado como compañero de programación y trabaja de forma asíncrona, como otro miembro del equipo, contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo de agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y coordinar los cambios.
- el modo de agente de GitHub Copilot puede implementar funciones en la interfaz de usuario y la capa de datos.
- revisar los cambios y las pruebas generados por el modo de agente antes de incorporarlos al código base.

## Escenario

A medida que crece la lista de juegos, quieres permitir que las personas filtren por categoría y editor. En el ejercicio anterior ya has añadido un método auxiliar para los editores; ahora completarás con el modo de agente de Copilot el resto del trabajo de la capa de datos, la interfaz de usuario y las pruebas.

## Ejecuta el sitio web Tailspin Toys

Antes de hacer cambios, exploremos el sitio web Tailspin Toys para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con temática de desarrollo. Permite mostrar una lista de juegos y ver sus detalles. Es una única aplicación Astro que genera las páginas como HTML estático durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde su frontmatter mediante Drizzle ORM: no hay una API de backend independiente ni un framework de interfaz del lado del cliente. Los métodos auxiliares reutilizables para acceder a los datos están en `src/lib/` y la interactividad se añade mediante un pequeño `<script>` de Astro con las API estándar del DOM.

### Inicia el sitio web

Para que sea más sencillo ejecutar el sitio web, se ha proporcionado la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Puedes ejecutarla en el codespace de GitHub siguiendo estos pasos:

1. Vuelve al codespace. Deberías estar en `main` después del ejercicio anterior.
2. Abre un terminal nuevo en el codespace con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama nueva para este trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta el siguiente comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo Astro esté listo, aparecerá un mensaje con la URL, similar a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si se abre un cuadro de diálogo para preguntarte si quieres abrir una ventana del navegador para `http://localhost:4321`, ciérralo seleccionando la **x**.

5. Para abrir el sitio web, mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En un codespace, al seleccionar un enlace a la URL de localhost en el terminal se te redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, que ahora aloja el servidor web.

### Explora el sitio web

Una vez iniciado el sitio web, puedes explorar sus funciones. Entre las principales:

- **Página de inicio**: muestra una lista de juegos de mesa con títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, el editor y la categoría.

## Explora las incidencias pendientes con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso lo indica cuando es necesario.

La primera versión del sitio web funciona, pero queremos añadir nuevas funciones. Empecemos por revisar el trabajo pendiente. Al crear el proyecto a partir de la plantilla, se generó automáticamente una lista de incidencias. Pídele a GitHub Copilot que te muestre esos elementos.

1. En la lista de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos del terminal e invoca herramientas.

   ![Captura de pantalla de la lista de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no prefieren un modelo sobre otro. Durante su desarrollo utilizamos Claude Sonnet 4.5 y por eso lo incluimos en estas instrucciones. Esperamos que las sugerencias de código sean relativamente consistentes para ofrecer una buena experiencia. Sin embargo, como los LLM son probabilísticos, es posible que recibas sugerencias distintas a las que se describen en el taller. Es normal y esperable.

> [!NOTE]
> Debido a la naturaleza probabilística de los LLM, Copilot podría utilizar un comando MCP diferente, pero debería poder completar la tarea.

3. Envía a Copilot el siguiente prompt para pedirle que muestre las incidencias pendientes:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Observa cómo Copilot también ha priorizado los elementos según lo que considera más útil para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que utilizarás para proporcionar contexto a Copilot. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que contiene información sobre cómo añadir funciones al sitio.

1. En el codespace, ve a `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funciones. Incluyen:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones específicos para distintos tipos de archivo, incluidos:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones permiten hacer referencia tanto a otros archivos de instrucciones como a archivos del proyecto. Las rutas se expresan en relación con la ubicación del archivo de instrucciones. Así se pueden reutilizar instrucciones, dividir conjuntos complejos en partes más manejables y proporcionar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado, tendrás que realizar al menos tres cambios distintos en la aplicación:

- añadir o perfeccionar la lógica de filtrado restante en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento del filtrado.
- actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de fusionar los cambios en el código base. El modo de agente de Copilot puede encargarse de estas tareas. Vamos a añadir la función.

1. Puedes continuar la conversación actual con Copilot o iniciar otra seleccionando **New Chat**.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de pantalla de la selección del archivo de instrucciones de interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** siga seleccionado en la lista de agentes de la vista Chat.

   ![Captura de pantalla de la lista de agentes en la vista Chat.](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que siga seleccionado **Claude Sonnet 4.5** como modelo.
5. Pide a Copilot que implemente la función según la incidencia relacionada del trabajo pendiente con este prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot empieza explorando el proyecto y localizando los archivos relacionados con la función. Debería encontrar tanto los métodos auxiliares de la capa de datos como la interfaz de usuario y las pruebas. Después, empezará a modificar los archivos y a ejecutar las pruebas.

   ![Captura de pantalla de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código base, puede tardar unos minutos. Mientras trabaja, es posible que aparezcan los botones **Keep** y **Undo** en el editor. Cuando termine, tendrás la opción de **Keep** o **Undo** para todos los cambios. No hace falta que los selecciones mientras el agente sigue trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla del cuadro de diálogo de Copilot Chat que pregunta si puede ejecutar las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas e incluso que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de pantalla de una sesión completa de Copilot Chat con el modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código que genera Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador con el sitio web en ejecución y explora la nueva función.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y pasos siguientes

En este ejercicio has aprendido a utilizar el modo de agente de GitHub Copilot para añadir funciones al sitio web Tailspin Toys. Has visto cómo:

- el modo de agente de GitHub Copilot implementa nuevas funciones en la interfaz de usuario y la capa de datos.
- el modo de agente de Copilot explora el proyecto, identifica los archivos pertinentes y coordina los cambios.
- revisar los cambios y las pruebas generados por el modo de agente antes de incorporarlos al código base.

A continuación, vamos a [probar la función con el servidor MCP de Playwright][next-lesson] y abrir una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar la paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Utiliza lo que has aprendido en este ejercicio para pedir a Copilot que actualice el sitio y la implemente. Algunas cuestiones que conviene tener en cuenta:

- sigue las prácticas recomendadas existentes, incluido el uso de los archivos de instrucciones actuales.
- decide cómo quieres implementar la paginación: por ejemplo, si permitirás que el usuario seleccione el tamaño de página o si lo fijarás en el código.
- al crear el prompt, proporciona a Copilot las instrucciones necesarias para que implemente la función como quieres.
- es posible que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto adicional. Es el flujo normal cuando se trabaja con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo se utilizan][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
