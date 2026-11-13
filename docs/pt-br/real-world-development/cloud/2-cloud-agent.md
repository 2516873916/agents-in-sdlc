---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Poucas organizações, se é que alguma, não enfrentam problemas de dívida técnica. Ela pode incluir problemas de segurança não resolvidos, código legado que precisa de atualizações ou solicitações de funcionalidades que ficaram paradas no backlog por falta de tempo para implementá-las. O agente de nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina o trabalho, ele gera um rascunho de pull request (PR) para revisão de um desenvolvedor. Isso permite delegar tarefas repetitivas, acelerar o processo de desenvolvimento e liberar os desenvolvedores para se concentrarem em assuntos mais importantes.

Neste exercício, você explorará o agente de nuvem do Copilot para:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- entender a importância de delimitar bem as issues.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas para resolver. Os prestadores de serviços contratados inicialmente para criar a primeira versão do site deixaram a documentação em um estado insatisfatório: você perceberá que ela está completamente ausente. Como primeira etapa, a equipe quer adicionar comentários de documentação TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design está pronta para melhorar a descoberta de jogos. Ela quer exibir jogos relacionados — outros títulos da mesma categoria — na página de detalhes de cada jogo para que as pessoas possam continuar navegando. A equipe ainda não precisa de uma implementação refinada; quer algo que possa usar para testes de aceitação da experiência do usuário. Isso está bloqueando o trabalho, embora haja outras issues de prioridade mais alta no momento.

Esses dois exemplos podem facilmente ser adiados e são boas tarefas para atribuir ao agente de nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona enquanto o desenvolvedor se concentra em outras tarefas e, depois, volta para revisar o trabalho do Copilot e confirmar que está tudo certo.

## Conheça o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode realizar tarefas em segundo plano, de maneira semelhante a um desenvolvedor. Assim como no trabalho com um desenvolvedor, há várias maneiras de atribuir tarefas, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois que você atribui a issue, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar na tarefa. Você pode acompanhar a sessão do Copilot enquanto o trabalho está em andamento ou depois de concluído. Quando a solução proposta estiver pronta para revisão, o Copilot marcará você no pull request.

## A importância de instruções bem delimitadas

Não há mágica no GitHub Copilot: não é possível deixar de pensar. Até operações aparentemente simples ficam complexas quando analisadas em detalhes. Por isso, [delimite cuidadosamente as tarefas atribuídas ao agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a inclusão de IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram as instruções personalizadas e como elas orientam o Copilot. Se você já fez um exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, esta é uma boa hora para dar uma olhada. Antes de atribuir trabalho ao agente de nuvem do Copilot, leia os arquivos de instruções já incluídos neste repositório sem alterá-los para observar o efeito deles mais tarde.

Abra os arquivos a seguir na interface Web do GitHub para seu repositório ou em um codespace, se já houver um em execução:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, principalmente as expectativas sobre convenções TypeScript e comentários de documentação TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que associa essas instruções aos arquivos `**/*.test.ts`.

Quando você atribuir a issue `Falta documentação no código` ao agente de nuvem no próximo exercício, procure comentários de documentação TSDoc, convenções TypeScript e cabeçalhos de comentários no pull request resultante. Esses elementos vêm dos arquivos de instruções que você está revisando. Voltaremos a esse assunto ao revisar os PRs em um exercício posterior.

## Configure o ambiente de desenvolvimento do agente de nuvem do Copilot

Para criar código, independentemente de quem esteja trabalhando, normalmente é necessário configurar um ambiente específico e executar alguns scripts de configuração para garantir que tudo esteja pronto. O mesmo se aplica às tarefas atribuídas ao Copilot, que trabalha de maneira semelhante a um engenheiro de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizar esse ambiente criando um [workflow de configuração especial][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml`, para ser executado antes de o agente começar o trabalho. Assim, ele terá acesso às ferramentas de desenvolvimento e dependências necessárias. Esse workflow já foi configurado para facilitar o fluxo do laboratório e permitir essa experiência. Ele fornece acesso ao Node.js, às dependências do projeto, aos binários de navegador para testes de ponta a ponta e ao banco de dados SQLite migrado e preenchido com dados para o aplicativo Astro único:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões no nível mínimo necessário para *suas etapas*. O Copilot receberá seu próprio token para as operações dele.
    permissions:
      # Se você quiser clonar o repositório como parte das etapas de configuração, por exemplo, para instalar dependências, precisará da permissão `contents: read`. Se não clonar o repositório nas etapas de configuração, o Copilot fará isso automaticamente depois que as etapas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo: Node.js (o aplicativo agora é todo Astro + Drizzle/libSQL).
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migre e preencha o banco de dados SQLite local com dados para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo parece qualquer outro workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único trabalho, chamado `copilot-setup-steps`. Esse trabalho é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar se o workflow funciona corretamente, sem precisar esperar que o Copilot o execute.

## Adicione documentação

Embora todos entendam a importância da documentação, a maioria dos projetos tem informações desatualizadas ou não tem documentação. Esse tipo de dívida técnica muitas vezes fica sem solução, o que reduz a produtividade e dificulta a manutenção da base de código ou a integração de novos desenvolvedores à equipe. Felizmente, o Copilot é muito bom em criar documentação, e esta é uma issue ideal para atribuir ao agente de nuvem do Copilot. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o trabalho.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. Defina **Title** como `Falta documentação no código`.
6. Defina **Description** como:

    ```plaintext
    Nossa organização exige que funções e métodos tenham comentários de documentação TSDoc quando forem úteis. Infelizmente, as atualizações recentes não seguiram esse padrão. Precisamos atualizar o código existente para incluir comentários de documentação onde eles esclarecerem o comportamento.
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

  ![Detalhes da atribuição da issue ao Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-gerado, com um título semelhante a `[WIP]: Falta documentação no código`. Se nenhum PR novo aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar vários minutos para aparecer no PR. O Copilot está criando o ambiente, executando o workflow mencionado anteriormente, analisando o projeto e determinando a melhor abordagem para resolver o problema.

13. Revise a lista e as tarefas que o agente pretende concluir.
14. Role a linha do tempo do pull request para baixo. Você deverá ver uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

  ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão em tempo real e acompanhar como o Copilot resolve o problema. Ele explora o código e entende o estado atual, faz pausas para pensar e decidir a abordagem adequada e, então, cria o código.

O processo provavelmente levará vários minutos. Um dos principais objetivos do agente de nuvem do Copilot é realizar tarefas de forma assíncrona, liberando você para se concentrar em outras tarefas. Vamos aproveitar justamente esse recurso atribuindo outra tarefa ao agente de nuvem do Copilot e, em seguida, voltando nossa atenção ao desenvolvimento de uma funcionalidade para o aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes do jogo

Como já destacamos, uma das grandes vantagens do agente de nuvem do GitHub Copilot é a possibilidade de dividir o trabalho: você pode se concentrar em um conjunto de tarefas enquanto ele trabalha em outro. Talvez a inclusão de uma seção de jogos relacionados para a equipe de design não leve muito tempo, mas ainda assim você poderia usar esse tempo em outras tarefas. Vamos atribuí-la ao agente de nuvem do Copilot!

1. Volte ao seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. Defina **Title** como `Exibir jogos relacionados na página de detalhes do jogo`.
6. Defina **Description** como:

    ```markdown
    Queremos ajudar as pessoas a descobrir mais jogos exibindo jogos relacionados na página de detalhes de cada jogo. A equipe de design quer explorar a experiência do usuário e fazer testes de aceitação. Estes são os requisitos:

   - Adicionar um auxiliar de acesso a dados em `src/lib/` que retorne outros jogos da mesma categoria de um jogo, sem incluir esse jogo
   - Exibir uma seção "Jogos relacionados" na página de detalhes do jogo que use o auxiliar
   - Tratar o caso em que um jogo não tenha jogos relacionados
   - Criar testes unitários para o novo auxiliar
   - Antes de criar o PR, confirmar que todos os testes foram aprovados
   ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

  ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você deverá ver alguns 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela!

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

Agora o Copilot está trabalhando na sua segunda solicitação! O agente de nuvem do Copilot funciona de maneira semelhante a um engenheiro de software, então não é necessário acompanhá-lo ativamente. Basta revisar o trabalho quando ele terminar. Agora, vamos nos concentrar na criação e no uso de agentes personalizados.

## Resumo e próximos passos

Este exercício explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com o agente de nuvem, você pode atribuir issues ao Copilot para que ele trabalhe nelas de forma assíncrona. Use o Copilot para lidar com dívidas técnicas, criar funcionalidades ou ajudar a migrar código de uma estrutura de desenvolvimento para outra.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- entender a importância de delimitar bem as issues.
- atribuir issues ao Copilot.

Enquanto o agente de nuvem do Copilot trabalha em segundo plano, vamos nos concentrar na criação e no uso de agentes personalizados. O [agente de nuvem do Copilot também pode usar servidores do Model Context Protocol (MCP)][cloud-agent-mcp] e tem acesso às instruções personalizadas exploradas em módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
