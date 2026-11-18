---
title: "Lição 7 - Crie e use um agente de QA"
description: "Crie um agente personalizado de QA que combine os requisitos do issue, a skill quality-checks e o MCP do Playwright."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você usou a skill `quality-checks` para executar verificações automatizadas e o MCP do Playwright para observar a experiência de filtragem em um navegador. Agora, reúna esses recursos em um agente personalizado com um processo de QA claramente definido.

Nesta lição, você vai:

- explorar como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criar e inspecionar um perfil reutilizável de garantia de qualidade.
- selecionar o agente de QA e revisar as descobertas em relação ao issue de filtragem.

## Cenário

A Tailspin Toys quer revisar requisitos, qualidade de código, verificações automatizadas, cobertura de testes e comportamento no navegador de forma consistente antes de abrir um pull request (PR). Um agente personalizado pode coordenar esse processo de QA e produzir um relatório reutilizável.

## O que é um agente personalizado?

Um agente personalizado é uma versão especializada do Copilot, definida em um perfil Markdown. O perfil descreve a finalidade, as instruções e as ferramentas disponíveis para o agente. Neste workshop, você definirá a função de QA em `.github/agents/qa.agent.md` e a selecionará no Copilot CLI.

As personalizações usadas até aqui têm funções diferentes. As instruções do repositório descrevem os padrões da equipe. A skill `quality-checks` reúne verificações repetíveis. O MCP do Playwright fornece ferramentas de navegador. O perfil de QA orienta o Copilot a usar esses recursos para avaliar os requisitos e relatar os resultados. Ele não substitui as outras personalizações nem exige uma conversa separada.

## Crie o perfil de QA

Antes de abrir o PR da funcionalidade, peça ao Copilot que crie um perfil de QA reutilizável. O perfil definirá as verificações e os limites que o agente deve respeitar.

1. Volte ao Codespace e confirme se a conversa de filtragem está no modo Interactive.
2. Peça ao agente padrão que crie o agente personalizado:

   ```plaintext
   Crie um agente personalizado chamado QA em .github/agents/qa.agent.md. Ele deve verificar as funcionalidades em relação aos issues e aos requisitos acordados, seguir as instruções do repositório, executar a skill quality-checks, usar o MCP do Playwright para verificar o comportamento e adicionar testes quando faltar cobertura.

   Peça que ele classifique cada requisito como aprovado, reprovado ou bloqueado e apresente evidências que sustentem o resultado. Ele deve pedir autorização antes de alterar o código de implementação e não pode confirmar alterações nem abrir pull requests.

   Por enquanto, apenas crie o perfil para que eu possa revisá-lo.
   ```

## Inspecione o perfil

Antes de usar o novo agente, revise o perfil para confirmar se o Copilot registrou o fluxo e os limites desejados. Assim, você evita que um agente incompleto ou com escopo amplo demais altere a funcionalidade quando você só quer verificá-la.

1. Insira `/diff` e abra `.github/agents/qa.agent.md`.
2. Leia o frontmatter. `description` é obrigatório; `name` é opcional, mas fornece um nome de exibição claro.
3. Leia as instruções do perfil e confirme se o agente começa pelos requisitos, segue as instruções do repositório, executa a skill `quality-checks` e usa o MCP do Playwright.
4. Confirme se o agente apresenta evidências, pede autorização antes de alterar o código de implementação e não confirma alterações nem abre pull requests.
5. Se o perfil gerado não incluir alguma dessas responsabilidades ou limites, peça ao agente padrão que o revise antes de continuar.

## Execute o QA com base no issue

O Copilot CLI carrega os agentes do projeto quando uma conversa começa. Retome a mesma conversa de filtragem depois de criar o perfil e selecione QA para que o agente use o issue e as decisões de planejamento já disponíveis no contexto.

1. Ative o agente com este prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como você acabou de criar o agente, talvez ele ainda não apareça na lista de agentes disponíveis. Mesmo assim, o comando acima o ativará.

2. Use este prompt para pedir ao agente de QA que revise a funcionalidade:

   ```plaintext
   Revise o recurso de filtragem em relação ao issue e às decisões do nosso plano. Ele está pronto para um PR?
   ```

3. O agente de QA começará a trabalhar.
4. Quando terminar, leia o relatório.

## Resumo e próximos passos

Você adicionou uma função especializada e reutilizável ao fluxo de trabalho e revisou o resultado. Nesta lição, você:

- explorou como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criou e inspecionou um perfil reutilizável de garantia de qualidade.
- selecionou o agente de QA e revisou as descobertas em relação ao issue de filtragem.

Agora você tem a implementação, a atualização da skill, o perfil de QA, os testes e o relatório de verificação prontos para revisão. Em seguida, você vai [reunir tudo no PR da funcionalidade e usar o Agent Merge][next-lesson].

## Recursos

- [Criar agentes personalizados para o Copilot CLI][create-agents]
- [Configuração de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
