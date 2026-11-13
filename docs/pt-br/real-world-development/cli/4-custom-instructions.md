---
title: "Lição 4 - Oriente o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica, você deve disponibilizar essa orientação ao Copilot. Os [arquivos de instruções][instruction-files] descrevem não apenas *que* código você quer, mas também *como* ele deve ser estruturado. Agora que você criou a filtragem, vai explorar as instruções usadas pelo Copilot, adicionar um padrão de documentação e aplicá-lo ao código.

Nesta lição, você vai:

- explorar como as instruções do repositório e os arquivos de instruções associados a caminhos chegam ao agente.
- atualizar o arquivo de instruções para garantir que os padrões de código sejam seguidos.
- observar o impacto dos arquivos de instruções no código.

## Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- Os comentários devem explicar a intenção e as decisões que não são óbvias, em vez de repetir o código.
- As funções exportadas em `db/` e `src/lib/` devem documentar sua finalidade, seus parâmetros e valores de retorno com TSDoc/JSDoc, incluindo um argumento `db` injetável quando houver.
- Os componentes Astro reutilizáveis devem documentar seus contratos `Props`, e os comentários devem continuar atualizados quando o código relacionado mudar.
- A orientação existente de formatação e lint deve ser preservada.

Usando arquivos de instruções, você fornecerá ao Copilot o contexto necessário para realizar tarefas de acordo com essas práticas.

## Arquivos de instruções

As instruções personalizadas fornecem contexto e preferências ao Copilot, ajudando-o a compreender seu estilo de código e seus requisitos. Esse recurso permite orientar as sugestões para que sejam mais relevantes e estejam de acordo com suas convenções de código, bibliotecas e preferências de comentários. Você pode criar instruções para todo o repositório ou para tipos específicos de arquivos, fornecendo contexto para determinada tarefa.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo enviado ao Copilot em **todas** as solicitações para o repositório. Ele deve conter informações sobre o projeto relevantes para a maioria das solicitações.
- Arquivos `.github/instructions/*.instructions.md`, que fornecem diretrizes para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> Outros formatos de instruções e o suporte a eles variam de acordo com o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explore os arquivos de instruções personalizadas deste projeto

O projeto inicial já inclui alguns arquivos de instruções. Explore o conteúdo existente antes de fazer alterações para observar o impacto.

1. Volte ao Codespace.
2. No editor do Codespaces, não no terminal, abra `.github/copilot-instructions.md`.
3. Explore o arquivo e observe a breve descrição do projeto e as diretrizes de código. Essas instruções se aplicam a todas as interações com o Copilot neste repositório.
4. Abra a pasta `.github/instructions` e explore os arquivos. Observe que há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
5. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início do arquivo: ele define um glob que determina a quais arquivos as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções e arquivos existentes do projeto. Assim, você pode dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e indicar ao Copilot exemplos para seguir.

## Atualize os arquivos de instruções de acordo com as diretrizes da equipe

Os arquivos existentes são um bom começo, mas ainda falta uma regra. Vamos modificar o arquivo principal `copilot-instructions.md` para exigir comentários TSDoc no novo código TypeScript.

1. Em `.github/copilot-instructions.md`, localize a seção intitulada **Code formatting guidance**, que deve estar por volta da linha 35.
2. Adicione o seguinte como último item da lista:

   ```markdown
   - Todo novo código TypeScript deve conter comentários TSDoc para fins de documentação.
   ```

O arquivo será salvo automaticamente!

## Use as diretrizes atualizadas

A interface de linha de comando (CLI) do Copilot carrega as instruções do repositório quando uma conversa começa. Depois de salvar as instruções atualizadas, encerre e retome a conversa de filtragem para que o Copilot comece a usar as novas diretrizes sem perder o contexto da funcionalidade.

1. Se a conversa de filtragem ainda estiver aberta, digite `/exit` para encerrá-la. O Copilot CLI salva as conversas automaticamente.
2. Na raiz do repositório, na branch `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme que a branch `game-filters-cli` está selecionada.
3. Peça ao Copilot que aplique as diretrizes atualizadas:

   ```plaintext
   Acabamos de atualizar nossas instruções e diretrizes de código. Você pode atualizar o código gerado para que siga essas diretrizes?
   ```

4. Digite `/diff` e leia os arquivos TypeScript alterados. Observe os comentários TSDoc adicionados e confirme que descrevem o código com precisão.

## Resumo e próximos passos

Você explorou como o Copilot CLI usa o contexto dos arquivos de instruções e aplicou um novo padrão à funcionalidade. Especificamente, você:

- explorou como as instruções do repositório e os arquivos de instruções associados a caminhos chegam ao agente.
- atualizou o arquivo de instruções para garantir que os padrões de código sejam seguidos.
- observou o impacto dos arquivos de instruções no código.

Em seguida, você [personalizará e executará a skill reutilizável quality-checks][next-lesson] para garantir a execução consistente do lint e dos testes.

## Recursos

- [Adicionar instruções personalizadas ao Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
