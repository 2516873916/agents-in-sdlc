---
title: "Exercício 3 - Teste sua funcionalidade com o servidor Playwright MCP"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar a funcionalidade de filtragem com o modo de agente. Antes de abrir um pull request, confirme que ela funciona no navegador. Em vez de testar o aplicativo manualmente, você conectará o **servidor Playwright MCP** e deixará que o Copilot controle um navegador real para testar a funcionalidade. Depois, publicará sua branch e abrirá o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot,
- adicionar o servidor Playwright MCP ao workspace,
- pedir ao Copilot que o use para testar manualmente a funcionalidade de filtragem no navegador,
- publicar sua branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente se torna muito mais poderoso quando pode acessar recursos além do editor. O Copilot usa o Model Context Protocol (MCP) para isso: é um padrão que permite ao agente conversar com ferramentas e serviços externos.

![Diagrama mostrando o funcionamento interno do modo de agente e a interação com contexto, modelo de linguagem de grande escala (LLM) e ferramentas, incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos. Com MCP, os agentes podem acessar informações atualizadas usando recursos e executar ações em seu nome usando ferramentas.

Essas ferramentas e esses recursos são acessados por um servidor MCP, que atua como uma ponte entre o agente de IA e as ferramentas e os serviços externos. O servidor MCP gerencia a comunicação entre o agente de IA e as ferramentas externas, como interfaces de programação de aplicações (APIs) existentes ou ferramentas locais, por exemplo, pacotes NPM. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Alguns servidores MCP conhecidos são:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: fornece acesso a um conjunto de APIs para gerenciar repositórios do GitHub. Ele permite que o agente de IA crie repositórios, atualize os existentes e gerencie issues e pull requests.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: fornece recursos de automação de navegador usando o Playwright. Ele permite que o agente de IA acesse páginas Web, preencha formulários e selecione botões.

Há muitos outros servidores MCP disponíveis que fornecem acesso a diferentes ferramentas e recursos. O GitHub hospeda um [registro de MCP](https://github.com/mcp) para facilitar a descoberta e as contribuições ao ecossistema.

> [!CAUTION]
> Em relação à segurança, trate os servidores MCP como qualquer outra dependência do seu projeto. Antes de usar um servidor MCP, revise cuidadosamente o código-fonte, verifique o publicador e considere as implicações de segurança. Use somente servidores MCP confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revise a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste workspace do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Você verá um servidor `github` já configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

Essa entrada `github` vem do modelo do projeto. Por isso, o Copilot conseguiu ler o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], então não é necessário instalar nada localmente: o VS Code se conecta por HTTP e você se autentica no GitHub quando o Copilot usa uma das ferramentas pela primeira vez.

## Adicione o servidor Playwright MCP

Agora você adicionará um segundo servidor. O [servidor Playwright MCP][playwright-mcp-server] fornece ao Copilot um navegador que ele pode controlar, exatamente o que você precisa para testar a funcionalidade.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`, de modo que o arquivo fique assim:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Salve o arquivo.

Diferentemente do servidor remoto `github`, `playwright` é um servidor **local**: o VS Code o inicia na sua máquina executando `npx @playwright/mcp@latest`. O sinalizador `--headless` faz com que o Playwright execute o navegador sem uma janela visível, o que é necessário em um codespace, que não tem um desktop para exibi-la.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright para testes de ponta a ponta, então o navegador necessário normalmente já está instalado. Se o Copilot informar mais tarde que o navegador está ausente, peça que ele execute `npx playwright install chromium` e tente novamente.

## Inicie e confie no servidor

O VS Code inicia os servidores MCP **sob demanda**: na primeira vez que o Copilot precisa das ferramentas de um servidor, o VS Code o inicia e pede que você confirme que confia nele.

1. Em `.vscode/mcp.json`, aparece uma ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor agora e confirmar que a configuração está correta.
2. Se o VS Code pedir que você confirme que confia no servidor, revise a configuração e escolha confiar nele para permitir que o servidor seja iniciado.

Iniciá-lo manualmente é opcional. Se você ignorar essa etapa, o VS Code iniciará o servidor e pedirá que você confirme se confia nele na primeira vez que o Copilot precisar das ferramentas, na próxima etapa.

## Teste a funcionalidade de filtragem

O [servidor Playwright MCP][playwright-mcp-server] fornece ao Copilot um navegador real para controlar. Em vez de navegar manualmente pelo aplicativo para verificar o trabalho, o agente pode abrir uma página, navegar, aplicar filtros, ler os resultados e apresentar um resumo. Essa é a maneira mais rápida de confirmar que a funcionalidade se comporta como esperado sem sair da conversa.

Nos bastidores, o servidor Playwright MCP usa a [árvore de acessibilidade][playwright-mcp-server] da página, em vez de capturas de tela. Isso significa que o agente analisa elementos estruturados e rotulados — botões, links, itens de lista — da mesma maneira que as tecnologias assistivas. Assim, uma verificação funcional rápida também pode servir como uma checagem básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste a funcionalidade de filtragem criada:

```text
Usando o servidor Playwright MCP, abra um navegador para o aplicativo em execução em http://localhost:4321 e verifique a nova funcionalidade de filtragem de jogos:

1. Acesse a página de jogos e observe quantos jogos estão listados.
2. Aplique um filtro de categoria e confirme que a lista é atualizada para exibir somente jogos dessa categoria.
3. Limpe o filtro e aplique um filtro de editora. Confirme que a lista é atualizada para exibir somente os jogos dessa editora.
4. Combine um filtro de categoria e um de editora e confirme que os resultados correspondem aos dois.

Relate o que observou em cada etapa e indique tudo que não se comportar como esperado.
```

O Copilot abrirá um navegador pelo servidor Playwright MCP, percorrerá cada etapa e informará o que encontrou. Compare o resumo com os critérios de aceitação da issue. Se algo parecer incorreto, faça perguntas complementares ou peça que ele corrija o código antes de abrir o pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321` para esse teste. Se você tiver parado o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o servidor Playwright MCP, talvez seja necessário baixar um navegador. Se ele informar que o navegador está ausente, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publique a branch e crie um pull request

Agora que confirmou que a funcionalidade funciona, você está pronto para abrir um pull request (PR) para que a equipe possa revisá-la. Primeiro, publique a branch `filtering-vscode`.

1. Acesse o painel **Source Control** no codespace e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Captura de tela do painel Source Control mostrando as alterações](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Crie o pull request

Há várias maneiras de criar um pull request, incluindo o github.com e a interface de linha de comando (CLI) do GitHub. Como você já está trabalhando com o GitHub Copilot, vamos pedir a ele que crie o PR. Ele pode localizar a issue relevante e associá-la ao PR.

1. Acesse o painel Copilot Chat e selecione **New Chat** para iniciar uma nova sessão.
2. Confirme que **Agent** está selecionado na lista suspensa de agentes para que o Copilot possa usar as ferramentas do GitHub.
3. Peça ao Copilot que crie um PR:

   ```plaintext
   Encontre no repositório a issue relacionada à filtragem por categoria e editora. Crie um novo pull request para a branch atual e associe-o à issue correta.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot execute as tarefas necessárias para reunir informações e realizar operações. Na primeira vez que ele usar uma ferramenta do GitHub, talvez seja solicitada a autenticação no GitHub. Siga as instruções para permitir o acesso.
5. Observe como o Copilot pesquisa as issues, encontra a correta e cria o PR.
6. Selecione o link gerado pelo Copilot para revisar o pull request, mas **não o mescle ainda**.

## Resumo e próximos passos

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam as ferramentas do Copilot,
- adicionou o servidor Playwright MCP ao workspace,
- usou o servidor para testar manualmente a funcionalidade de filtragem no navegador antes de entregá-la,
- publicou sua branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* uma funcionalidade, mas o MCP é apenas uma das maneiras de ampliar o Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para agilizar tarefas focadas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [Registro de MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
