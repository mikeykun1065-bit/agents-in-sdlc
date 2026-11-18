---
title: "Lección 6 - Valida la funcionalidad con Playwright MCP"
description: "Inspecciona o configura Playwright MCP en Copilot CLI y utilízalo para explorar en un navegador la función de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como ya hemos destacado, escribir código implica mucho más que escribir código. Tenemos que trabajar con datos y servicios externos, e incluso permitir que Copilot utilice automatizaciones adicionales. Para ello se utilizan los servidores MCP. Permiten que Copilot vaya más allá de las funciones integradas en la CLI y le proporcionan más herramientas y servicios.

En esta lección:

- aprenderás qué es el Protocolo de contexto de modelo (MCP) y cómo lo utiliza Copilot CLI.
- añadirás el servidor MCP de Playwright si todavía no está disponible.
- pedirás al agente que controle un navegador y explore la función de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero para validar las actualizaciones de la interfaz de usuario hay que interactuar con ella. Quieres que Copilot utilice el sitio web como lo haría una persona y automatice más cambios para tener más seguridad de que funcionan como esperas.

## ¿Qué es el Protocolo de contexto de modelo (MCP)?

El [Protocolo de contexto de modelo (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos. Así pueden acceder a información actualizada y realizar acciones en tu nombre.

Se accede a estas herramientas y recursos a través de un servidor MCP, que actúa como puente entre el agente de IA y las herramientas y servicios externos. Cada servidor MCP representa un conjunto distinto de herramientas y recursos a los que puede acceder el agente de IA.

Algunos servidores MCP conocidos:

- **[Servidor MCP de GitHub][github-mcp]**: proporciona acceso a las API para gestionar repositorios de GitHub, incidencias y solicitudes de incorporación de cambios.
- **[Servidor MCP de Playwright][playwright-mcp-server]**: proporciona funciones de automatización del navegador con Playwright.

Hay muchos otros servidores MCP disponibles. GitHub aloja un [registro de MCP][mcp-registry] para facilitar la búsqueda de servidores y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de utilizar uno, revisa su código fuente, verifica quién lo publica y considera las implicaciones de seguridad.

## Añade el servidor MCP de Playwright

Si el servidor MCP de Playwright todavía no está disponible, añádelo para que Copilot pueda interactuar con el sitio web como lo haría una persona.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve al codespace.
2. En Copilot CLI, escribe el siguiente comando para abrir el cuadro de diálogo para añadir un servidor MCP:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Pulsa <kbd>Enter</kbd> para confirmar el tipo de servidor **STDIO** y, después, pulsa <kbd>Tab</kbd>.
5. Pega lo siguiente en el cuadro de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo MCP.

## Pide a Copilot que explore la función con Playwright

Antes has comprobado manualmente que la función se comporta como esperas. Ahora vamos a utilizar el servidor MCP de Playwright disponible en Copilot para hacer lo mismo.

1. Utiliza el siguiente prompt para pedir a Copilot que valide la función con el servidor MCP de Playwright:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Normalmente no es necesario indicar a Copilot que utilice el servidor MCP; suele elegirlo automáticamente. Sin embargo, como ya sabes cuál debería utilizar, no está de más indicárselo. Así se obtienen resultados más coherentes y se ahorran tokens.

2. Observa los pasos que va realizando Copilot en el navegador para comprobar que la función funciona.
3. Lee el informe y comprueba que todo se comporte como esperas.

Copilot iniciará el servidor, utilizará Playwright para interactuar con el sitio web, detendrá el servidor y te mostrará un informe.

## Resumen y pasos siguientes

Enhorabuena, has utilizado el servidor MCP de Playwright para explorar tu función en un navegador real desde Copilot CLI. En resumen:

- has aprendido qué es el Protocolo de contexto de modelo (MCP) y cómo lo utiliza Copilot CLI.
- has añadido el servidor MCP de Playwright si todavía no estaba disponible.
- has pedido al agente que controlara un navegador y explorara la función de filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combinará la habilidad y las herramientas del navegador en una función especializada.

## Recursos

- [Qué es MCP y por qué habla todo el mundo de ello][mcp-blog-post]
- [Servidor MCP de Microsoft Playwright][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
