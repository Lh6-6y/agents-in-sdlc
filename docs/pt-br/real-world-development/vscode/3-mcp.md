---
title: "Exercício 3 - Teste seu recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar o recurso de filtragem com o modo agente. Antes de abrir um pull request, confirme que ele funciona no navegador. Em vez de navegar pelo aplicativo manualmente, conecte o **servidor MCP do Playwright** e deixe o Copilot controlar um navegador real para testar o recurso. Depois, publique sua branch e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP estendem os recursos do Copilot.
- adicionar o servidor MCP do Playwright ao workspace.
- pedir ao Copilot que o use para testar manualmente o recurso de filtragem no navegador.
- publicar sua branch e abrir um pull request para o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo agente fica muito mais poderoso quando pode acessar recursos além do editor. O Model Context Protocol (MCP) permite isso: é um padrão para o agente se comunicar com ferramentas e serviços externos.

![Diagrama mostrando o funcionamento interno do modo agente e como ele interage com contexto, LLM e ferramentas — incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real. Assim, eles podem acessar informações atualizadas (por meio de recursos) e executar ações em seu nome (por meio de ferramentas).

Essas ferramentas e esses recursos são acessados por um servidor MCP, que funciona como ponte entre o agente de IA e ferramentas e serviços externos, como APIs existentes ou ferramentas locais, incluindo pacotes npm. Cada servidor MCP representa um conjunto diferente de ferramentas e recursos que o agente pode acessar.

Alguns servidores MCP populares:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: fornece acesso a APIs para gerenciar repositórios do GitHub, criar e atualizar repositórios e gerenciar issues e pull requests.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: oferece automação de navegador com o Playwright, permitindo navegar em páginas, preencher formulários e selecionar botões.

Há muitos outros servidores MCP com acesso a diferentes ferramentas e recursos. O GitHub mantém um [registro MCP](https://github.com/mcp) para facilitar a descoberta e a contribuição para o ecossistema.

> [!CAUTION]
> Trate servidores MCP como qualquer outra dependência do projeto. Antes de usar um, revise cuidadosamente o código-fonte, verifique quem o publicou e considere as implicações de segurança. Use apenas servidores confiáveis e tenha cuidado ao conceder acesso a recursos ou operações sensíveis.

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

Essa configuração `github` faz parte do template do projeto. Por isso, o Copilot conseguiu ler o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], então não é necessário instalar nada localmente: o VS Code se conecta por HTTP, e você autentica no GitHub na primeira vez que o Copilot usa uma das ferramentas.

## Adicione o servidor MCP do Playwright

Agora, adicione um segundo servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador controlável, ideal para testar seu recurso.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`, para que o arquivo fique assim:

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

Diferentemente do servidor remoto `github`, `playwright` é um servidor **local**: o VS Code o inicia na sua máquina executando `npx @playwright/mcp@latest`. O sinalizador `--headless` instrui o Playwright a executar o navegador sem uma janela visível, o que é necessário em um codespace, que não tem um desktop para exibi-la.

> [!NOTE]
> O projeto Tailspin Toys já usa o Playwright nos testes de ponta a ponta, portanto o navegador necessário geralmente já está instalado. Se o Copilot informar depois que falta um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Inicie e autorize o servidor

O VS Code inicia os servidores MCP **sob demanda**: na primeira vez que o Copilot precisa das ferramentas de um servidor, o VS Code o inicia e pede que você confirme se confia nele.

1. Em `.vscode/mcp.json`, haverá uma ação **Start** acima da entrada `playwright`. Selecione-a para iniciar o servidor e confirmar que a configuração está correta.
2. Se o VS Code perguntar se você confia no servidor, revise a configuração e autorize-o para que possa iniciar.

Iniciá-lo manualmente é opcional. Se você não fizer isso, o VS Code iniciará o servidor e pedirá autorização na próxima etapa, quando o Copilot precisar das ferramentas.

## Teste o recurso de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] oferece ao Copilot um navegador real para controlar. Em vez de navegar pelo aplicativo para conferir seu trabalho, o agente pode abrir uma página, navegar, aplicar filtros e apresentar os resultados. É uma maneira rápida de confirmar o comportamento do recurso sem sair da conversa.

Internamente, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página, em vez de capturas de tela. Assim, o agente analisa elementos estruturados e identificados (botões, links, itens de lista) como faria uma tecnologia assistiva. Uma verificação funcional rápida também serve como uma verificação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste o recurso criado:

```text
Usando o servidor MCP do Playwright, abra um navegador para o aplicativo em execução em http://localhost:4321 e verifique o novo recurso de filtragem de jogos:

1. Acesse a página de jogos e informe quantos jogos estão listados.
2. Aplique um filtro de categoria e confirme que a lista mostra somente jogos dessa categoria.
3. Limpe o filtro e aplique um filtro de editora. Confirme que a lista mostra somente jogos dessa editora.
4. Combine um filtro de categoria e um de editora e confirme que os resultados respeitam ambos.

Informe o que observar em cada etapa e destaque qualquer comportamento inesperado.
```

O Copilot iniciará um navegador pelo servidor MCP do Playwright, executará cada etapa e apresentará o resultado. Compare o relatório com os critérios de aceitação da issue. Se algo parecer errado, faça perguntas complementares ou peça que corrija o código antes de abrir o pull request.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321`. Se você parou o servidor de desenvolvimento, inicie-o novamente antes de enviar o prompt. Na primeira vez que o Copilot usar o Playwright MCP, talvez seja necessário baixar um navegador. Se ele informar que o navegador está ausente, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publique a branch e crie um pull request

Depois de confirmar que o recurso funciona, abra um pull request (PR) para que a equipe possa revisá-lo. Primeiro, publique a branch `filtering-vscode`.

1. No codespace, abra o painel **Source Control** e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Captura de tela do painel Source Control mostrando as alterações](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Crie o pull request

Há várias maneiras de criar um pull request, inclusive pelo github.com e pela interface de linha de comando (CLI) do GitHub. Como você já está usando o GitHub Copilot, peça que ele crie o PR: ele pode localizar a issue relacionada e associá-la ao PR.

1. Abra o painel Copilot Chat e selecione **New Chat** para iniciar uma nova sessão.
2. No seletor de agentes, confirme que **Agent** está selecionado para permitir o uso das ferramentas do GitHub.
3. Peça ao Copilot que crie o PR:

   ```plaintext
   Encontre a issue do repositório relacionada à filtragem por categoria e editora. Crie um pull request para a branch atual e associe-o à issue correta.
   ```

4. Se necessário, selecione **Continue** para permitir que o Copilot obtenha informações e execute as operações necessárias. Na primeira utilização de uma ferramenta do GitHub, talvez seja necessário autenticar. Siga as instruções.
5. Observe o Copilot pesquisar as issues, encontrar a correta e criar o PR.
6. Selecione o link gerado pelo Copilot para revisar o pull request, mas **não o mescle ainda**.

## Resumo e próximos passos

Parabéns! Neste exercício, você:

- aprendeu o que é o Model Context Protocol (MCP) e como os servidores MCP estendem os recursos do Copilot.
- adicionou o servidor MCP do Playwright ao workspace.
- usou o servidor para testar manualmente o recurso de filtragem no navegador antes de publicá-lo.
- publicou a branch e abriu um pull request para o trabalho de filtragem.

Você usou um servidor MCP para *testar* um recurso — mas o MCP é apenas uma das maneiras de ampliar o Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para simplificar tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
