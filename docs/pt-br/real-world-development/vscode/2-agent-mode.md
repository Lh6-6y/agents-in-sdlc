---
title: "Exercício 2 - Adicione novas funcionalidades com o modo agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até as atualizações mais simples de um aplicativo costumam exigir alterações em vários arquivos e a execução de operações como testes. O fluxo de trabalho típico de um desenvolvedor envolve localizar todos os arquivos necessários, fazer as alterações, executar os testes, depurar, descobrir qual arquivo ficou de fora, fazer outra alteração... e assim por diante.

É aí que entra o modo agente do Copilot.

O modo agente do Copilot foi criado para atuar com mais autonomia no IDE. Ele se comporta de maneira semelhante a um desenvolvedor: começa explorando a estrutura do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como usar o modo agente para adicionar funcionalidades ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo agente e o agente de nuvem foram criados para experiências diferentes. O modo agente executa tarefas no IDE, permitindo interações e ciclos rápidos de feedback. O agente de nuvem funciona como um colega de programação, trabalha de forma assíncrona e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- o modo agente do GitHub Copilot pode implementar recursos na interface e na camada de dados.
- revisar alterações e testes gerados pelo modo agente antes de mesclá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem por categoria e editora. No exercício anterior, você já adicionou um helper de editoras; agora concluirá com o modo agente do Copilot o trabalho restante na camada de dados, na interface e nos testes.

## Execute o site Tailspin Toys

Antes de fazer alterações, explore o site Tailspin Toys para entender as funcionalidades atuais.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com tema de desenvolvimento. Ele permite listar jogos e exibir detalhes sobre eles. Trata-se de um único aplicativo Astro que gera páginas HTML estáticas durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter usando o Drizzle ORM — não há uma API de back-end separada nem um framework de interface no cliente. Os helpers de acesso a dados reutilizáveis ficam em `src/lib/`, e a interatividade é implementada com pequenos elementos `<script>` Astro com escopo definido, usando as APIs padrão do DOM.

### Inicie o site

Para facilitar a execução do site, há uma tarefa `npm run dev` que inicia o único servidor de desenvolvimento Astro. Execute-a no GitHub Codespace:

1. Volte ao codespace. Depois do exercício anterior, você deve estar na branch `main`.
2. Abra um novo terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie e selecione uma nova branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o seguinte comando para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, aparecerá uma mensagem com a URL, semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se aparecer uma caixa de diálogo pedindo para abrir uma janela do navegador para `http://localhost:4321`, feche-a selecionando **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, selecionar o link `localhost` no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`. Esse é um túnel privado para o codespace, que agora hospeda seu servidor Web.

### Explore o site

Com o site em execução, explore suas funcionalidades principais:

- **Página inicial**: mostra uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá uma página com mais informações, incluindo título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes dos exercícios abaixo, volte ao codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa informa o que selecionar quando necessário.

A implementação inicial do site funciona, mas queremos adicionar recursos. Vamos começar examinando o backlog. Ao criar o projeto com o template, um backlog de issues foi criado automaticamente. Peça ao GitHub Copilot que mostre esses itens.

1. No seletor de agentes da visualização Chat, selecione **Agent**. Esse agente planeja e implementa alterações em vários arquivos de forma autônoma, executa comandos de terminal e chama ferramentas.

   ![Captura de tela mostrando o seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione **Claude Sonnet 4.5** na lista de modelos disponíveis.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo em detrimento de outro. Durante a criação do workshop, usamos o Claude Sonnet 4.5 e o mencionamos nas instruções para tentar manter as sugestões de código relativamente consistentes. Como os LLMs são probabilísticos, as sugestões podem ser diferentes das mostradas no workshop. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente, mas ainda deve conseguir concluir a tarefa.

3. Peça ao Copilot que mostre o backlog enviando este prompt:

   ```plaintext
   Mostre o backlog de itens do meu repositório do GitHub. Ajude a priorizá-los de acordo com o que será mais útil para o usuário.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista gerada.

Observe que o Copilot também priorizou os itens com base no que considera mais útil para o usuário.

## Revise os arquivos de instruções

Antes de pedir ao agente que gere código, revise o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, capacidade de teste e acessibilidade.
   - links para arquivos de instruções específicos, incluindo os de Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem referenciar outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos à localização do arquivo de instruções. Isso permite reutilizar orientações, dividir instruções complexas em arquivos menores e fornecer exemplos e templates.

## Implemente a funcionalidade de filtragem

Para concluir a filtragem, são necessárias pelo menos três alterações no aplicativo:

- adicionar ou refinar a lógica restante de filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar testes para o comportamento dos filtros.
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Além disso, os testes precisam ser executados e aprovados antes de mesclar as alterações à base de código. O modo agente do Copilot pode executar essas tarefas. Vamos adicionar a funcionalidade.

1. Continue na conversa atual do Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Captura de tela mostrando um exemplo de seleção do arquivo de instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. No seletor de agentes da visualização Chat, confirme que **Agent** continua selecionado.

   ![Captura de tela mostrando o seletor de agentes na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade com base na issue relacionada do backlog:

   ```plaintext
   Atualize o site para incluir filtros por editora e categoria de acordo com os requisitos da issue relacionada no backlog do GitHub. Um helper de editoras já existe desde o exercício anterior; preserve e refine esse trabalho conforme necessário ao concluir as alterações restantes na camada de dados, na interface e nos testes. Confirme que todos os testes passam antes de terminar. O servidor já está em execução, então não é necessário iniciá-lo.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos relacionados ao recurso: os helpers da camada de dados, a interface e os testes. Depois, ele começará a modificar os arquivos e executar os testes.

   ![Captura de tela mostrando o Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, modificar arquivos e executar testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e da base de código. Durante o processo, os botões **Keep** e **Undo** podem aparecer no editor. Quando o Copilot terminar, você poderá manter ou desfazer todas as alterações; não é necessário selecioná-los enquanto o trabalho estiver em andamento.

7. Quando o Copilot solicitar, selecione **Continue** para executar os testes.

   ![Captura de tela de uma caixa de diálogo no painel Copilot Chat solicitando confirmação para executar os testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Talvez haja pausas e alguns testes falhem durante o processo. Isso é normal! O Copilot alterna entre gerar código e executar testes até concluir a tarefa sem detectar erros.

   ![Captura de tela de uma sessão completa do Copilot Chat no modo agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado e procure possíveis problemas.

> [!CAUTION]
> Sempre revise o código gerado pelo Copilot ou por qualquer outra ferramenta de IA.

10. Volte ao navegador com o site em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Parabéns! Neste exercício, exploramos como usar o modo agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Você aprendeu como:

- implementar novos recursos na interface e na camada de dados com o modo agente do GitHub Copilot.
- explorar o projeto, identificar arquivos relevantes e coordenar alterações com o modo agente.
- revisar alterações e testes gerados pelo modo agente antes de mesclá-los à base de código.

Agora, vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício extra de exploração — Implemente a paginação

À medida que a lista de jogos crescer, será necessário adicionar paginação. Use as habilidades deste exercício e peça ao Copilot que atualize o site:

- siga as práticas recomendadas existentes, incluindo o uso dos arquivos de instruções do projeto.
- decida como implementar a paginação: se o usuário poderá escolher o tamanho da página ou se ele será fixo.
- forneça ao Copilot as orientações necessárias no prompt para obter a implementação desejada.
- talvez seja necessário iterar com o GitHub Copilot, pedindo alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot!

## Recursos

- [Copilot: modos ask, edit e agent — para que servem e quando usá-los][choose-mode]
- [Modo agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
