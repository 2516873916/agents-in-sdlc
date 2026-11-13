---
title: "Lição 6 - Valide a funcionalidade com Playwright MCP"
description: "Inspecione ou configure o Playwright MCP no Copilot CLI e use-o para explorar a filtragem no navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já destacamos, escrever código envolve mais do que apenas escrever código. Precisamos trabalhar com dados, serviços externos e até disponibilizar outras automações para o Copilot. É aí que entram os servidores do Model Context Protocol (MCP). Eles permitem que o Copilot vá além dos recursos integrados à interface de linha de comando (CLI) e tenha acesso a ainda mais ferramentas e serviços.

Nesta lição, você vai:

- entender o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa.
- adicionar o servidor Playwright MCP, caso ele ainda não esteja disponível.
- pedir ao agente que controle um navegador e explore sua funcionalidade de filtragem.

## Cenário

Testes unitários e de ponta a ponta são importantes, mas validar alterações na interface do usuário exige interagir com ela. Você quer permitir que o Copilot use o site em desenvolvimento como um usuário faria, automatizando ainda mais as alterações e aumentando a confiança de que elas se comportam como esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem com ferramentas e serviços externos. Com MCP, os agentes podem acessar informações atualizadas e executar ações em seu nome.

Essas ferramentas e esses recursos são acessados por um servidor MCP, que atua como uma ponte entre o agente de IA e as ferramentas e os serviços externos. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente de IA pode acessar.

Alguns servidores MCP conhecidos são:

- **[GitHub MCP Server][github-mcp]**: fornece acesso a APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: fornece recursos de automação de navegador usando o Playwright.

Há muitos outros servidores MCP disponíveis. O GitHub hospeda um [registro de MCP][mcp-registry] para facilitar a descoberta e as contribuições ao ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do seu projeto. Antes de usar um servidor, revise o código-fonte, verifique o publicador e considere as implicações de segurança.

## Adicione o servidor Playwright MCP

Se o servidor Playwright MCP ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como um usuário faria.

> [!NOTE]
> Se o Playwright MCP já estiver configurado no Copilot CLI, ignore estas etapas de configuração e continue na próxima seção.

1. Volte ao Codespace.
2. Abra a caixa de diálogo para adicionar um servidor MCP digitando o seguinte comando no Copilot CLI:

   ```plaintext
   /mcp add
   ```

3. Para o nome, digite `playwright` e pressione <kbd>Tab</kbd>.
4. Confirme o tipo de servidor **STDIO** pressionando <kbd>Enter</kbd> e, em seguida, pressione <kbd>Tab</kbd>.
5. Cole o seguinte na caixa de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o novo servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo MCP.

## Peça ao Copilot para explorar a funcionalidade com o Playwright

Antes, você confirmou manualmente que a funcionalidade se comporta como esperado. Agora vamos usar o servidor Playwright MCP disponível no Copilot para fazer o mesmo!

1. Use o seguinte prompt para pedir ao Copilot que valide a funcionalidade com o servidor Playwright MCP:

   ```plaintext
   Inicie o aplicativo e use o Playwright MCP para verificar a filtragem de acordo com a issue e nosso plano. Diga o que funciona e o que não funciona, sem fazer alterações. Quando terminar, pare o servidor iniciado.
   ```

> [!NOTE]
> Normalmente, não é necessário dizer ao Copilot para usar o servidor MCP, pois ele costuma identificá-lo por conta própria. Mas, como você sabe qual servidor deve ser usado, pode indicar isso para ajudar a obter resultados mais consistentes e economizar alguns tokens.

2. Acompanhe as etapas que o Copilot lista enquanto interage com o navegador para confirmar se a funcionalidade funciona.
3. Leia o relatório e confira se tudo se comporta como esperado.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, interromperá o servidor e apresentará um relatório.

## Resumo e próximos passos

Parabéns, você usou o servidor Playwright MCP para explorar sua funcionalidade em um navegador real a partir do Copilot CLI! Recapitulando, você:

- aprendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o usa.
- adicionou o servidor Playwright MCP, caso ele ainda não estivesse disponível.
- pediu ao agente que controlasse um navegador e explorasse sua funcionalidade de filtragem.

Em seguida, você [criará um agente personalizado de QA][next-lesson] para combinar a skill e as ferramentas do navegador em uma função especializada.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
