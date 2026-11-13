---
title: "Exercício 2 - Adicione funcionalidades com o Modo de Agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até as atualizações mais simples de um aplicativo normalmente exigem alterações em vários arquivos e operações, como executar testes. Em geral, o fluxo de trabalho de um desenvolvedor envolve localizar todos os arquivos necessários, fazer as alterações, executar os testes, depurar, identificar o arquivo que ficou faltando, fazer outra atualização... E a lista continua.

É aí que entra o Modo de Agente do Copilot.

O Modo de Agente do Copilot trabalha de forma mais autônoma no ambiente de desenvolvimento integrado (IDE). Ele se comporta de maneira semelhante a um desenvolvedor: começa explorando a estrutura existente do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como você pode usar o Modo de Agente para adicionar novas funcionalidades ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente realiza tarefas no IDE, permitindo ciclos rápidos de feedback e interação. O agente de nuvem foi criado para atuar como um parceiro de programação que trabalha de forma assíncrona, como um membro da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o Modo de Agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- o Modo de Agente do GitHub Copilot pode implementar funcionalidades na interface do usuário e na camada de dados.
- revisar alterações e testes gerados pelo Modo de Agente do Copilot antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que os usuários filtrem os jogos por categoria e editora. No exercício anterior, você adicionou um auxiliar de editoras. Agora, concluirá o trabalho restante na camada de dados, na interface do usuário e nos testes usando o Modo de Agente do Copilot.

## Execute o site Tailspin Toys

Antes de fazer alterações, vamos explorar o site Tailspin Toys para entender suas funcionalidades atuais.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. Ele permite listar jogos e exibir detalhes sobre eles. O site é um único aplicativo Astro que renderiza suas páginas como linguagem de marcação de hipertexto (HTML) estática no momento da compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter usando o mapeador objeto-relacional (ORM) Drizzle. Não há uma interface de programação de aplicações (API) de back-end separada nem uma estrutura de interface do usuário do lado do cliente. Os auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é adicionada com um pequeno `<script>` Astro com escopo próprio que usa APIs padrão do modelo de objeto do documento (DOM).

### Inicie o site

Para facilitar a execução do site, há uma tarefa `npm run dev` que inicia o único servidor de desenvolvimento Astro. Você pode executá-la no GitHub Codespace seguindo estas etapas:

1. Volte ao codespace. Você deverá estar em `main` depois do exercício anterior.
2. Abra uma nova janela de terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie uma nova branch para o trabalho de filtragem e mude para ela:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o seguinte comando para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, você verá um banner com a URL, semelhante a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo pedir para abrir uma janela do navegador para `http://localhost:4321`, feche-a selecionando o **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Ao usar um codespace, selecionar o link da URL localhost no terminal do Codespace redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Este é um túnel privado para seu codespace, que agora hospeda o servidor Web.

### Explore o site

Com o site em execução, você pode explorar suas funcionalidades. Os principais recursos incluem:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: quando você seleciona um jogo, é direcionado a uma página de detalhes com mais informações, incluindo título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar os exercícios a seguir, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e a seleção do modelo variam de acordo com o exercício; cada etapa indica o que é necessário.

A implementação inicial do site funciona, mas queremos aprimorá-la com novos recursos. Vamos começar revisando o backlog. Ao configurar o projeto a partir do modelo, um backlog de issues foi criado automaticamente. Peça ao GitHub Copilot que mostre esses itens.

1. Selecione **Agent** na lista suspensa de agentes da visualização Chat. O agente **Agent** planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos no terminal e invoca ferramentas.

   ![Captura de tela mostrando o seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo em particular. Durante a criação do workshop, usamos Claude Sonnet 4.5 e o incluímos nas instruções para que as sugestões de código sejam relativamente consistentes e proporcionem uma boa experiência. No entanto, como os grandes modelos de linguagem (LLMs) são probabilísticos, as sugestões podem ser diferentes das indicadas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot poderá usar um comando do Model Context Protocol (MCP) diferente, mas ainda deverá concluir a tarefa.

3. Envie o seguinte prompt ao Copilot para perguntar sobre o backlog de issues:

   ```plaintext
   Mostre o backlog de itens do meu repositório do GitHub. Ajude-me a priorizá-los com base na utilidade para os usuários.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista de issues gerada.

Observe que o Copilot também priorizou os itens de acordo com o que considera mais útil para os usuários.

## Revise os arquivos de instruções

Antes de pedir ao agente que gere o código, é uma boa hora para revisar o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No codespace, acesse `.github/instructions/ui.instructions.md`.
2. Observe as diretrizes gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para arquivos de instruções específicos de vários tipos de arquivo, incluindo:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> Os arquivos de instruções permitem fazer referência a outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao local do arquivo de instruções. Assim, você pode reutilizar orientações, dividir instruções complexas em partes menores e gerenciáveis e fornecer exemplos e modelos.

## Implemente a funcionalidade de filtragem

Para concluir a filtragem, você precisará fazer pelo menos três alterações separadas no aplicativo:

- adicionar ou aprimorar a lógica restante de filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento de filtragem.
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes de mesclar tudo à base de código. O Modo de Agente do Copilot pode realizar essas tarefas para você. Vamos adicionar a funcionalidade.

1. Você pode continuar na conversa atual com o Copilot ou iniciar outra selecionando **New Chat**.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Captura de tela mostrando um exemplo de seleção do arquivo de instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado na lista suspensa de agentes da visualização Chat.

   ![Captura de tela mostrando o seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade com base na issue relacionada no backlog usando o seguinte prompt:

   ```plaintext
   Atualize o site para incluir a filtragem por editora e categoria com base nos requisitos da issue relacionada do GitHub no backlog. Já existe um auxiliar de editoras do exercício anterior; preserve e refine esse trabalho conforme necessário para concluir o restante da camada de dados, da interface e dos testes. Confirme que todos os testes foram aprovados. O servidor já está em execução, então não é necessário iniciá-lo.
   ```

6. Observe como o Copilot começa explorando o projeto e localizando os arquivos relacionados à funcionalidade desejada. Você verá que ele localiza os auxiliares da camada de dados, a interface e os testes. Em seguida, começa a modificar os arquivos e executar os testes.

   ![Captura de tela mostrando o Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e executar testes. O processo pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, os botões **Keep** e **Undo** poderão aparecer no editor de código. Quando o Copilot terminar, você terá a opção **Keep** ou **Undo** para todas as alterações, então não precisa selecioná-las enquanto o trabalho estiver em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela mostrando uma caixa de diálogo no painel do Copilot Chat pedindo confirmação para executar os testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Alguns testes podem falhar durante o processo. Não há problema: o Copilot alterna entre a geração de código e os testes até concluir a tarefa e não detectar erros.

   ![Captura de tela mostrando uma sessão completa do Copilot Chat no Modo de Agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Lembre-se: é sempre importante revisar o código gerado pelo Copilot ou por outras ferramentas de IA.

10. Volte ao navegador em que o site está em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, exploramos como usar o Modo de Agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Aprendemos como:

- o Modo de Agente do GitHub Copilot pode implementar funcionalidades na interface do usuário e na camada de dados.
- o Modo de Agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- revisar alterações e testes gerados pelo Modo de Agente do Copilot antes de mesclá-los à base de código.

Agora vamos [testar sua funcionalidade com o servidor Playwright MCP][next-lesson] e abrir um pull request.

### Exercício opcional de exploração — implemente a paginação

À medida que a lista de jogos cresce, será necessário habilitar a paginação. Usando as habilidades que você aprendeu neste exercício, peça ao Copilot para atualizar o site e implementar a paginação. Considere:

- seguir as práticas recomendadas existentes, incluindo o uso dos arquivos de instruções atuais.
- decidir como a paginação deve funcionar, por exemplo, se os usuários podem selecionar o tamanho da página ou se ele deve ser fixo.
- fornecer ao Copilot as orientações necessárias no prompt para que a implementação corresponda ao que você deseja.
- fazer iterações com o GitHub Copilot, solicitando alterações e fornecendo contexto. Esse é o fluxo normal ao trabalhar com o Copilot!

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
