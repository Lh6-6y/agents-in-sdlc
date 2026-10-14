---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser executada de uma maneira específica — ou se houver informações que o Copilot deva conhecer —, esse contexto precisa estar acessível. Os [arquivos de instruções][instruction-files] fornecem orientações para que o Copilot entenda não só *o que* você quer, mas também *como* o trabalho deve ser feito.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de codificação e os padrões de documentação chegam ao Copilot por meio das instruções personalizadas do repositório e de arquivos com escopo por caminho.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente das trilhas de VS Code e CLI, você não fará aqui uma comparação com um prompt *antes/depois*. O agente de nuvem do Copilot trabalha de forma assíncrona em issues do GitHub, então é mais difícil demonstrar sua influência lado a lado em tempo real. Você verá o efeito das instruções mais adiante, ao revisar os pull requests criados pelo agente.

## Arquivos de instruções

### Cenário

Como toda boa equipe de desenvolvimento, a Tailspin Toys tem um conjunto de diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- A camada de dados sempre precisa de testes unitários.
- A interface do usuário deve usar o modo escuro e ter um visual moderno.
- A documentação deve ser adicionada ao código na forma de comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para ajudá-lo a entender melhor o estilo de codificação e os requisitos do projeto. Você pode especificar convenções, bibliotecas e até os tipos de comentários que prefere. Crie instruções para todo o repositório ou para tipos específicos de arquivos e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações gerais do projeto relevantes para a maioria das interações, como a pilha tecnológica, uma visão geral do trabalho e padrões globais.
- Arquivos `.github/instructions/*.instructions.md`, que podem orientar linguagens, tipos de arquivo ou tarefas específicas, como componentes de interface ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat — não para conclusões de código nem sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam instruções do repositório e arquivos `*.instructions.md` com metadados `applyTo` ao gerar código.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], incluindo AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Uma conversa completa sobre a criação de arquivos de instruções está fora do escopo deste workshop, mas o projeto de exemplo mostra uma abordagem representativa:

- Mantenha `copilot-instructions.md` focado em orientações gerais, como descrição, estrutura do projeto e padrões globais de codificação.
- Use arquivos `*.instructions.md` para tipos de arquivo (testes unitários, componentes Astro, camada de dados) ou tarefas específicas.
- Use linguagem natural, orientações claras e exemplos de código adequado e inadequado.

Não existe uma única maneira correta de criar arquivos de instruções ou usar IA. A experimentação ajuda a descobrir o que funciona melhor para o projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter uma coleção sólida de arquivos de instruções. Neste projeto há orientações para vários tipos de tarefa, incluindo [alterações na interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada ambiente oferece esse recurso de uma maneira diferente (por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI). A lição correspondente explica quando usá-lo.
>
> Para encontrar templates e exemplos, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções personalizadas deste projeto

Este repositório inclui um arquivo principal `copilot-instructions.md` e vários arquivos `*.instructions.md` para diferentes tarefas. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas de **GitHub Actions Workflows**. Elas se aplicam a todas as interações com o Copilot.
3. Abra a pasta `.github/instructions` e explore os arquivos. Há instruções para Astro, a camada de dados Drizzle, testes e mais.
4. Abra `.github/instructions/unit-tests.instructions.md`. O campo `applyTo` no início define um glob relativo à raiz do repositório. Neste caso, ele corresponde a arquivos de teste TypeScript, por exemplo, `**/*.test.ts`.
5. Observe as instruções específicas para criar testes unitários neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Isso permite dividir instruções extensas em arquivos menores e reutilizáveis e apontar o Copilot para exemplos. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** de `copilot-instructions.md` documenta os padrões do projeto, mas ainda não exige documentação dentro do código. Nas próximas etapas, você adicionará regras para comentários TSDoc e comentários no cabeçalho dos arquivos.

## Adicione um padrão ao repositório

Você fará uma edição neste exercício: adicionar uma regra de todo o projeto para que a documentação do código use comentários TSDoc e um comentário no cabeçalho de cada arquivo. O agente de nuvem seguirá essa orientação nas issues atribuídas mais adiante.

Antes de editar, crie uma branch (o agente de nuvem lê as instruções da branch de destino da issue, mas você fará commit das alterações nesta branch):

1. No terminal do codespace, crie e selecione uma branch:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como mencionado, `.github/copilot-instructions.md` fornece informações gerais do projeto ao Copilot. Vamos documentar os padrões de codificação para melhorar as sugestões.

1. Reabra `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Ela documenta os padrões do projeto, mas ainda não define uma regra para documentação no código.
3. Adicione as linhas Markdown abaixo dos padrões existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, os arquivos de instruções podem ficar no nível do repositório (`.github/copilot-instructions.md`) para orientações globais ou em arquivos `*.instructions.md` para linguagens, tipos de arquivos e tarefas específicas. O arquivo do repositório é o lugar certo para padrões gerais, como a regra de comentários de documentação adicionada aqui.

## Faça commit, envie e mescle a atualização das instruções

O agente de nuvem lê os arquivos de instruções da branch de destino da issue. Ao atribuir uma issue ao Copilot no próximo exercício, ele criará uma branch a partir de `main`. Portanto, as instruções precisam ser mescladas em `main` para que o agente as use.

1. Prepare e faça commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request da branch `custom-instructions-cloud` para `main` e mescle-o. Assim, o agente de nuvem poderá ler as instruções ao trabalhar nas issues atribuídas no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente na branch `main` neste workshop, pule as etapas de criação de branch e faça commit diretamente em `main`. A etapa de branch foi incluída para reproduzir o fluxo de um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções do projeto e adicionou um novo padrão válido para todo o repositório em `.github/copilot-instructions.md`. Você verá esse padrão aplicado nos pull requests criados pelo agente de nuvem ao longo desta trilha.

Em seguida, você vai [atribuir sua primeira issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
