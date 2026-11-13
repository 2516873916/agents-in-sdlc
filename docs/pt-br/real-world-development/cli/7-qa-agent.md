---
title: "Lição 7 - Crie e use um agente de garantia de qualidade (QA)"
description: "Crie um agente personalizado de QA que combine os requisitos da issue, a skill quality-checks e o Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você usou a skill `quality-checks` para executar verificações automatizadas e o Playwright MCP para observar a filtragem em um navegador. Agora, você reunirá esses recursos em um agente personalizado com um processo de QA bem definido.

Nesta lição, você vai:

- explorar como um agente personalizado trabalha com instruções, skills e ferramentas do Model Context Protocol (MCP).
- criar e inspecionar um perfil reutilizável de garantia de qualidade.
- selecionar o agente de QA e revisar as descobertas em relação à issue de filtragem.

## Cenário

Antes de abrir um pull request (PR), a Tailspin Toys quer uma revisão consistente dos requisitos, da qualidade do código, das verificações automatizadas, da cobertura de testes e do comportamento no navegador. Um agente personalizado pode coordenar esse processo de QA e fornecer um relatório reutilizável.

## O que é um agente personalizado?

Um agente personalizado é uma versão especializada do Copilot definida em um perfil Markdown. O perfil descreve a finalidade, as instruções e as ferramentas disponíveis para o agente. Neste workshop, você definirá a função de QA em `.github/agents/qa.agent.md` e a selecionará no Copilot CLI.

As personalizações usadas têm funções diferentes. As instruções do repositório descrevem os padrões da equipe. A skill `quality-checks` reúne verificações repetíveis. O Playwright MCP fornece ferramentas de navegador. O perfil de QA informa ao Copilot como usar esses recursos para avaliar os requisitos e relatar as descobertas. Ele não substitui esses recursos nem exige outra conversa.

## Crie o perfil de QA

Antes de abrir o PR da funcionalidade, peça ao Copilot que crie um perfil de QA reutilizável. O perfil definirá tanto as verificações de QA quanto os limites que o agente deve respeitar.

1. Volte ao Codespace e confirme que a conversa de filtragem está no modo Interactive.
2. Peça ao agente padrão que crie o novo agente personalizado:

   ```plaintext
   Crie um agente personalizado chamado QA em .github/agents/qa.agent.md. Ele deve verificar as funcionalidades com base nas issues e nos requisitos acordados, seguir as instruções do repositório, executar a skill quality-checks, usar o Playwright MCP para verificar o comportamento e adicionar testes quando faltar cobertura.

   Faça com que ele informe cada requisito como aprovado, reprovado ou bloqueado, com evidências que sustentem o resultado. Ele deve pedir autorização antes de alterar o código de implementação e não deve fazer commit nem abrir pull requests.

   Por enquanto, apenas crie o perfil para que eu possa revisá-lo.
   ```

## Inspecione o perfil

Antes de usar o novo agente, revise o perfil para confirmar que o Copilot registrou o workflow de QA e os limites pretendidos. Assim, você evita que um agente incompleto ou com escopo amplo demais altere a funcionalidade quando você só quer verificá-la.

1. Digite `/diff` e abra `.github/agents/qa.agent.md`.
2. Leia o frontmatter. `description` é obrigatório; `name` é opcional, mas incluí-lo dá ao agente um nome de exibição claro.
3. Leia as instruções do perfil e confirme que QA começa pelos requisitos, segue as instruções do repositório, executa a skill `quality-checks` e usa o Playwright MCP.
4. Confirme que QA apresenta evidências, pede autorização antes de alterar o código de implementação e não faz commits nem abre pull requests.
5. Se o perfil gerado não incluir alguma dessas responsabilidades ou algum desses limites, peça ao agente padrão que o revise antes de continuar.

## Execute QA com base na issue

A interface de linha de comando (CLI) do Copilot carrega os agentes do projeto quando uma conversa começa. Depois de criar o perfil, retome a mesma conversa de filtragem e selecione QA para que o agente possa usar a issue e as decisões de planejamento já presentes no contexto.

1. Ative o agente com o seguinte prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como você acabou de criar o agente, talvez ele ainda não apareça na lista de agentes disponíveis. Mesmo assim, o comando acima o ativará.

2. Use o seguinte prompt para pedir ao agente de QA que revise a funcionalidade:

   ```plaintext
   Revise a funcionalidade de filtragem com base na issue e nas decisões do nosso plano. Ela está pronta para um PR?
   ```

3. O agente de QA começará a trabalhar.
4. Depois que ele terminar, leia o relatório.

## Resumo e próximos passos

Você adicionou uma função especializada reutilizável ao workflow e revisou o trabalho do agente. Nesta lição, você:

- explorou como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criou e inspecionou um perfil reutilizável de garantia de qualidade.
- selecionou o agente de QA e revisou as descobertas em relação à issue de filtragem.

Agora você tem a implementação, a atualização da skill, o perfil de QA, os testes e o relatório de verificação prontos para revisão. Em seguida, você [reunirá tudo em um PR de funcionalidade e usará o Agent Merge][next-lesson].

## Recursos

- [Criar agentes personalizados para o Copilot CLI][create-agents]
- [Configuração de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
