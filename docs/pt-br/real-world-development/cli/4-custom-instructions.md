---
title: "Lição 4 - Oriente o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser executada de uma maneira específica, você deve disponibilizar essa orientação ao Copilot. Os [arquivos de instruções][instruction-files] descrevem não apenas *qual* código você quer, mas também *como* ele deve ser estruturado. Agora que você criou a filtragem, vai explorar as instruções usadas pelo Copilot, adicionar um padrão de documentação e aplicá-lo ao código.

Nesta lição, você vai:

- explorar como as instruções do repositório e os arquivos de instrução com escopo por caminho chegam ao agente.
- atualizar o arquivo de instruções para garantir que os padrões de codificação sejam seguidos.
- observar o impacto dos arquivos de instruções no código.

## Cenário

Como toda boa equipe de desenvolvimento, a Tailspin Toys tem um conjunto de diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- Os comentários devem explicar a intenção e as decisões não óbvias, em vez de repetir o código.
- As funções exportadas em `db/` e `src/lib/` devem documentar sua finalidade, seus parâmetros e valores de retorno com TSDoc/JSDoc, incluindo um argumento `db` injetável quando houver.
- Os componentes Astro reutilizáveis devem documentar seus contratos `Props`, e os comentários devem permanecer atualizados quando o código relacionado mudar.
- As orientações existentes de formatação e lint devem ser preservadas.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para realizar tarefas de acordo com essas práticas.

## Arquivos de instruções

As instruções personalizadas permitem fornecer contexto e preferências ao Copilot, ajudando-o a entender melhor seu estilo de codificação e seus requisitos. Esse recurso pode orientar as sugestões de código. Você pode especificar convenções de codificação, bibliotecas e até os tipos de comentários que prefere. É possível criar instruções para todo o repositório ou para tipos específicos de arquivos.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo enviado ao Copilot em **todas** as solicitações referentes ao repositório. Ele deve conter informações gerais do projeto, relevantes para a maioria das solicitações feitas ao Copilot pelo chat ou pela CLI.
- Arquivos `.github/instructions/*.instructions.md`, que fornecem orientações para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> Os outros formatos de instrução e o suporte disponível variam conforme o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explore os arquivos de instruções personalizadas do projeto

O projeto inicial já inclui alguns arquivos de instruções. Vamos explorá-los antes de fazer uma alteração e observar o impacto.

1. Volte ao Codespace.
2. No editor do Codespaces, abra `.github/copilot-instructions.md`.
3. Explore o arquivo e observe a breve descrição do projeto e as orientações de codificação. Essas instruções se aplicam a todas as interações com o Copilot neste repositório.
4. Abra a pasta `.github/instructions` e explore os arquivos. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
5. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início: ele define um glob que determina a quais arquivos as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções e arquivos existentes no projeto. Assim, você pode dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e indicar exemplos para o Copilot seguir.

> [!NOTE]
> A seção **Code formatting guidance** em `copilot-instructions.md` documenta os padrões de codificação do projeto, mas ainda não exige documentação dentro do código. Nas próximas etapas, você adicionará regras para comentários TSDoc e comentários no cabeçalho dos arquivos.

## Atualize os arquivos de instruções conforme as orientações da equipe

Os arquivos atuais são um bom começo, mas ainda há uma lacuna. Vamos modificar o arquivo principal `copilot-instructions.md` para garantir que o TypeScript gerado inclua comentários TSDoc.

1. Em `.github/copilot-instructions.md`, localize a seção **Code formatting guidance**, que deve estar por volta da linha 35.
2. Adicione o item abaixo como último tópico:

   ```markdown
   - Todo novo código TypeScript deve incluir comentários TSDoc para fins de documentação.
   ```

O arquivo será salvo automaticamente!

## Use as orientações atualizadas

O Copilot CLI carrega as instruções do repositório quando uma conversa é iniciada. Depois de salvar as instruções, saia e retome a conversa de filtragem para que o Copilot comece a usar as novas orientações sem perder o contexto do recurso.

1. Se a conversa de filtragem ainda estiver aberta, digite `/exit` para encerrá-la. As conversas são salvas automaticamente.
2. Na raiz do repositório, na branch `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme que a branch `game-filters-cli` está selecionada.
3. Peça ao Copilot que aplique as orientações atualizadas:

   ```plaintext
   Atualizamos nossas instruções e orientações de código. Você pode atualizar o código gerado para que siga essas orientações?
   ```

4. Digite `/diff` e leia os arquivos TypeScript alterados. Observe os novos comentários TSDoc e confirme que descrevem o código corretamente.

## Resumo e próximos passos

Você explorou como o Copilot CLI usa arquivos de instruções e aplicou um novo padrão ao recurso. Especificamente, você:

- explorou como as instruções do repositório e os arquivos de instrução com escopo por caminho chegam ao agente.
- atualizou o arquivo de instruções para garantir que os padrões de codificação sejam seguidos.
- observou o impacto dos arquivos de instruções no código.

Em seguida, você vai [personalizar e executar a skill reutilizável quality-checks][next-lesson] para garantir que o lint e os testes sejam executados de maneira consistente.

## Recursos

- [Adicionar instruções personalizadas ao Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot — coleção de arquivos de instrução e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
