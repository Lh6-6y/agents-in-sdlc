---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser executada de uma maneira específica — ou houver informações que o Copilot deva conhecer —, esse contexto precisa estar acessível. Os [arquivos de instruções][instruction-files] são uma das principais formas de fornecer orientações sobre a geração de código.

Neste exercício, você vai:

- explorar como o contexto específico do projeto, as diretrizes de codificação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e arquivos com escopo por caminho.
- gerar a primeira parte dos dados de filtragem (um helper de editoras) com as instruções *atuais* em vigor.
- adicionar um novo padrão válido para todo o repositório em `.github/copilot-instructions.md`.
- executar novamente o mesmo prompt e observar o código gerado adotar o novo padrão.
- fazer commit da atualização das instruções e da base do recurso de filtragem em `main` para que o Copilot use essas orientações no próximo exercício.

> [!CAUTION]
> O código gerado pode divergir de alguns dos padrões definidos. O Copilot é não determinístico. O objetivo é observar a *tendência* de mudança no comportamento depois de atualizar as instruções, não reproduzir exatamente cada caractere da saída.

## Arquivos de instruções

### Cenário

Como toda boa equipe de desenvolvimento, a Tailspin Toys tem um conjunto de diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa de testes unitários.
- A interface do usuário deve usar o modo escuro e ter um visual moderno.
- O código deve incluir documentação na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas permitem fornecer contexto e preferências ao Copilot para que ele entenda melhor seu estilo de codificação e seus requisitos. Esse recurso ajuda a orientar as sugestões de código. Você pode especificar convenções de codificação, bibliotecas e até os tipos de comentários que prefere. É possível criar instruções para todo o repositório ou para tipos específicos de arquivos.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações do projeto relevantes para a maioria das interações, como a pilha tecnológica, uma visão geral do que está sendo desenvolvido, práticas recomendadas e orientações gerais.
- Arquivos `.github/instructions/*.instructions.md`, que podem fornecer orientações para linguagens (como TypeScript ou Astro), tipos de arquivo ou tarefas específicas, como criar componentes de interface ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat — não para conclusões de código nem sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam instruções do repositório e arquivos `*.instructions.md` com metadados `applyTo` ao gerar código.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], incluindo AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

O projeto inicial demonstra uma abordagem representativa:

- Mantenha `copilot-instructions.md` focado em orientações para o projeto, como a descrição do que está sendo desenvolvido, sua estrutura e padrões globais de codificação.
- Use arquivos `*.instructions.md` para orientar tipos específicos de arquivos (testes unitários, componentes Astro, camada de dados) ou tarefas.
- Use linguagem natural e mantenha as orientações claras. Inclua exemplos de código que deve e que não deve ser gerado.

Não há uma única maneira correta de criar arquivos de instruções ou usar IA. A experimentação ajuda a descobrir o que funciona melhor para cada projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter uma boa coleção de arquivos de instruções. No projeto, você encontrará arquivos para várias tarefas, incluindo [alterações na interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada ambiente oferece esse recurso de uma maneira diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI). A lição correspondente explicará quando usar cada opção.
>
> Para encontrar templates e exemplos, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções personalizadas do projeto

O projeto inclui um arquivo principal `copilot-instructions.md` e vários arquivos `*.instructions.md` para tarefas diferentes. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a descrição breve do projeto e as seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas sobre **GitHub Actions Workflows**. Elas se aplicam a qualquer interação com o Copilot.
3. Abra a pasta `.github/instructions` e examine os arquivos. Há instruções para Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md`. O campo `applyTo` no início define um glob (relativo à raiz do repositório) que determina os arquivos abrangidos. Neste caso, ele corresponde a arquivos de teste TypeScript, por exemplo, `**/*.test.ts`.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Esses caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** de `copilot-instructions.md` documenta os padrões de codificação, mas ainda não exige documentação dentro do código. Nas próximas etapas, você adicionará regras para comentários TSDoc e comentários no cabeçalho dos arquivos.

## Crie uma branch para as alterações

Vamos seguir as práticas recomendadas e criar uma branch para as alterações.

1. Volte ao Codespace usado no exercício anterior.
2. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e selecione uma branch:

   ```bash
   git checkout -b custom-instructions
   ```

## Use o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de começar as etapas abaixo, volte ao Codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa informa o que selecionar quando necessário.

Para observar o impacto das instruções personalizadas, envie primeiro um prompt usando o arquivo de instruções atual. Depois, atualize o arquivo e envie o mesmo prompt novamente para comparar.

1. Feche as guias de editor abertas dos exercícios anteriores para que o Copilot use somente o contexto necessário.
2. Abra `src/lib/publishers.ts` para indicar onde o helper deve ficar.
3. No seletor de agentes da visualização Chat, selecione **Agent** para que o Copilot possa aplicar alterações nos arquivos.

   ![Captura de tela mostrando o seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie o prompt:

   ```plaintext
   Crie ou atualize src/lib/publishers.ts com um helper de acesso a dados que retorne uma lista de todas as editoras, incluindo o nome e o id de cada uma. Aplique as alterações aos arquivos.
   ```

5. O Copilot explora o projeto e aplica atualizações de código, muitas vezes tanto no arquivo do helper quanto nos testes.
6. Observe que o helper proposto é uma função tipada que recebe um cliente `db` como primeiro argumento e retorna um array tipado de editoras — isso segue as convenções da camada de dados em `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observe que o código proposto **não inclui** comentários TSDoc nem um comentário no cabeçalho do arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele adicione comentários mesmo sem essa orientação. Se isso acontecer, tudo bem — o objetivo ainda é observar a melhoria da *consistência* depois da atualização das instruções.

## Adicione um novo padrão para o repositório

Como mencionado, `.github/copilot-instructions.md` fornece ao Copilot informações gerais do projeto. Vamos documentar os padrões de codificação do repositório para melhorar as sugestões.

1. Reabra `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Observe que ela descreve os padrões de codificação, mas ainda não exige documentação no código.
3. Adicione as linhas Markdown abaixo dos padrões existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, arquivos de instruções podem ficar na raiz do repositório (`.github/copilot-instructions.md`) para orientações globais ou em arquivos `*.instructions.md` para linguagens, tipos de arquivo e tarefas específicas. O arquivo do repositório é o lugar certo para padrões gerais, como a regra de comentários de documentação que você acabou de adicionar.

## Execute novamente o prompt e observe a mudança

1. Volte ao Copilot Chat e selecione **New Chat** para limpar o contexto.
2. Selecione `src/lib/publishers.ts` novamente para direcionar o Copilot ao arquivo correto.
3. Envie o **mesmo prompt** de antes:

   ```plaintext
   Crie ou atualize src/lib/publishers.ts com um helper de acesso a dados que retorne uma lista de todas as editoras, incluindo o nome e o id de cada uma. Aplique as alterações aos arquivos.
   ```

4. Observe que o arquivo proposto agora começa com um bloco de comentários semelhante a:

   ```typescript
   /**
    * Helpers de acesso a dados de editoras para a plataforma de financiamento coletivo Tailspin Toys.
    * Fornece funções para recuperar informações de editoras do banco de dados.
    */
   ```

5. Observe que a função proposta agora inclui um comentário TSDoc semelhante a:

   ```typescript
   /**
    * Retorna uma lista de todas as editoras com seus ids e nomes.
    *
    * @param db - Cliente do banco de dados Drizzle.
    * @returns Uma promessa que resolve para um array de objetos de editoras.
    */
   ```

Você orientou o Copilot a seguir um novo padrão e aplicá-lo ao código real que servirá de base para o próximo exercício.

## Faça commit das instruções e envie a branch

Os arquivos de instruções são gerenciados pelo controle de versão como qualquer outro recurso. Vamos fazer commit e enviar a branch ao repositório.

1. Abra um novo terminal pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. No terminal, confirme que as atualizações das instruções e do helper estão presentes:

   ```bash
   git status
   ```

3. Prepare e faça commit da atualização das instruções e de todos os arquivos alterados pelo Copilot para criar a base do helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras atualizações de apoio (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Crie e mescle um pull request

Com a branch enviada, crie um pull request associado à issue sobre os padrões de documentação no backlog. O Copilot pode localizar a issue, criar um PR associado a ela e mesclá-lo.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot executa ações no GitHub. O template do projeto já conecta as ferramentas do GitHub ao workspace, então não é necessário configurar nada. Na primeira utilização, o VS Code talvez solicite que você entre no GitHub. Siga as instruções para autorizar o acesso. Você aprenderá como essa conexão funciona (Model Context Protocol) em um exercício posterior.

1. Abra o Copilot Chat no Codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização Copilot Chat e confirme que **Agent** está selecionado.
3. Peça ao Copilot que localize a issue sobre a atualização dos arquivos de instruções e crie um PR da branch atual, descrevendo tanto as instruções quanto a base do helper de editoras:

   ```plaintext
   Encontre a issue relacionada à atualização do arquivo de instruções. Crie um novo PR a partir da branch atual, informe que o PR fecha essa issue e inclua que também adicionamos a base do helper de editoras para o próximo trabalho de filtragem.
   ```

4. O Copilot começará a localizar a issue e criar o PR.
5. Quando o Copilot pedir **Allow** para executar ações do GitHub em seu nome, revise a operação e selecione **Allow** conforme apropriado.
6. Depois que o PR for criado, peça ao Copilot que o mescle e retorne sua branch para `main`:

   ```plaintext
   Mescle o PR em main. Depois, volte localmente para main e obtenha o código mais recente.
   ```

7. Quando solicitado a permitir ações do GitHub e comandos do shell, revise cada operação e selecione **Allow** conforme apropriado.

Você criou e mesclou um pull request com a ajuda do GitHub Copilot!

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções do projeto e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções *existentes*.
- adicionar um novo padrão válido para todo o repositório em `.github/copilot-instructions.md`.
- executar novamente o mesmo prompt e observar o código proposto adotar o novo padrão.
- fazer commit das instruções e da base do helper em `main` para que o próximo exercício use essas alterações.

Em seguida, você colocará essas instruções em prática no [modo agente][next-lesson], enquanto o Copilot adiciona um recurso em vários arquivos.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
