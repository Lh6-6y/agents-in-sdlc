---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Poucas organizações, se houver alguma, não enfrentam dívida técnica: problemas de segurança pendentes, código legado que precisa de atualização ou solicitações de recursos que ficaram paradas no backlog por falta de tempo. O agente de nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Ao concluir o trabalho, ele gera um PR de rascunho para revisão de uma pessoa desenvolvedora. Isso permite delegar tarefas tediosas, acelerar o desenvolvimento e dedicar mais tempo a objetivos maiores.

Com o agente de nuvem do Copilot, você explorará:

- como personalizar o ambiente de geração de código.
- como garantir que as operações sejam executadas com segurança.
- a importância de issues com escopo claro.
- como atribuir issues ao Copilot.

## Cenários

A Tailspin Toys quer resolver alguns problemas técnicos pendentes. Os prestadores contratados para criar a primeira versão do site não incluíram documentação. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer facilitar a descoberta de jogos. Ela gostaria que a página de detalhes de cada jogo mostrasse outros títulos da mesma categoria para que as pessoas continuem explorando. Ainda não precisa ser uma implementação refinada; a equipe quer algo que possa usar nos testes de aceitação da experiência. Embora isso esteja bloqueando o trabalho, há outras issues mais prioritárias.

São exemplos de tarefas que acabam adiadas com frequência e que podem ser atribuídas ao agente de nuvem do Copilot. Ele trabalha de forma assíncrona, enquanto você cuida de outras tarefas, e depois você pode revisar o trabalho e confirmar que o resultado está correto.

## Conheça o agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano de maneira semelhante a uma pessoa desenvolvedora. Você pode, por exemplo, [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão durante a execução ou depois que ela terminar. Quando a solução estiver pronta para revisão, o Copilot marcará você no pull request.

## A importância de instruções com escopo claro

Não há mágica no GitHub Copilot — não é possível deixar de pensar no problema. Mesmo tarefas aparentemente simples podem ter complexidades que surgem quando analisamos os detalhes; por isso, [defina com cuidado o escopo das tarefas para o agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como um colega de programação com IA: trabalhe em etapas, aprenda, experimente e adapte. Os fundamentos do desenvolvimento de software não mudam com a chegada da IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram as instruções personalizadas e como elas orientam o Copilot. Se você já concluiu o exercício prático de instruções personalizadas, viu `.github/copilot-instructions.md` em ação. Caso contrário, aproveite para examiná-lo. Antes de atribuir tarefas ao agente de nuvem, leia os arquivos de instruções incluídos no repositório para depois identificar sua influência.

Abra estes arquivos no github.com ou em um codespace em execução:

- `.github/copilot-instructions.md` — examine a seção **Code standards**, especialmente as convenções de TypeScript e os comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe os metadados `applyTo`, que aplicam essas instruções a arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente de nuvem na próxima seção, observe no PR os comentários TSDoc, as convenções de TypeScript e os comentários no início dos arquivos. Essas orientações vêm dos arquivos de instruções que você revisou. Voltaremos a elas ao examinar os PRs em um exercício futuro.

## Configure o ambiente de desenvolvimento do agente de nuvem

A criação de código normalmente exige um ambiente específico e a execução de scripts de configuração. O mesmo se aplica ao atribuir tarefas ao Copilot, que trabalha de maneira semelhante a um engenheiro de software.

O agente de nuvem usa o [GitHub Actions][github-actions] para preparar o ambiente. Você pode personalizá-lo criando um [fluxo de configuração especial][setup-workflow] no arquivo `.github/workflows/copilot-setup-steps.yml`, executado antes do início do trabalho. Isso permite instalar ferramentas e dependências necessárias. A configuração já foi preparada para este laboratório: ela disponibiliza ao Copilot Node.js, as dependências do projeto, binários de navegador para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido para o aplicativo Astro.

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório durante a configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Caso não clone o repositório nas etapas de configuração, o Copilot fará isso automaticamente quando elas terminarem.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo - Node.js (agora o aplicativo inteiro usa Astro + Drizzle/libSQL)
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

      # Migre e preencha o banco de dados SQLite local para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

Esse arquivo se parece com outros fluxos do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job chamado `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o fluxo manualmente na guia **Actions** do repositório. Isso é útil para testar se ele funciona sem esperar que o Copilot o execute.

## Adicione documentação

Embora todos reconheçam a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação alguma. Essa dívida técnica reduz a produtividade, dificulta a manutenção da base de código e torna mais difícil integrar novas pessoas à equipe. O Copilot é especialmente útil para criar documentação, então esta é uma boa issue para atribuir ao agente de nuvem. Ele trabalhará em segundo plano; em um exercício futuro, você revisará o resultado.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir o diálogo de nova issue.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Code lacks documentation`.
6. No campo **Description**, digite:

    ```plaintext
    Nossa organização exige que funções e métodos tenham comentários TSDoc quando forem úteis. Infelizmente, as atualizações recentes não seguiram esse padrão. Precisamos atualizar o código existente para incluir comentários de documentação quando eles esclarecerem o comportamento.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir o diálogo de atribuição.

   ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
 
   ![Detalhes da atribuição do Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request criado; o título será semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer, aguarde alguns instantes e atualize a página.
12. Depois de alguns minutos, deverá aparecer uma lista de tarefas criada pelo Copilot.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está preparando o ambiente (executando o fluxo de configuração descrito acima), analisando o projeto e definindo a melhor abordagem.

13. Revise a lista e as tarefas que serão executadas.
14. Role a linha do tempo do pull request e procure uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione **View session**.

   ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Percorra a sessão em tempo real e observe como o Copilot resolve o problema: explora o código e seu estado, pausa para pensar e definir um plano e, em seguida, cria código.

Esse processo pode levar vários minutos. Um dos principais objetivos do agente de nuvem é executar tarefas de forma assíncrona, liberando você para se dedicar a outras atividades. Vamos aproveitar esse recurso: atribua outra tarefa ao agente de nuvem e, enquanto isso, escreva código para adicionar funcionalidades ao aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Uma das grandes vantagens do agente de nuvem do GitHub Copilot é a possibilidade de dividir o trabalho: você pode se concentrar em um conjunto de tarefas enquanto ele trabalha em outro. Mesmo que a seção de jogos relacionados para a equipe de design não demore muito, esse tempo pode ser usado em outra tarefa. Vamos atribuí-la ao agente de nuvem.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue**.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Show related games on the game details page`.
6. No campo **Description**, digite:

    ```markdown
    Queremos ajudar as pessoas a descobrir mais jogos exibindo títulos relacionados na página de detalhes de cada jogo. A equipe de design quer explorar a experiência do usuário e fazer alguns testes de aceitação. Requisitos:

   - Adicione um helper de acesso a dados em `src/lib/` que retorne outros jogos da mesma categoria de um jogo, excluindo o próprio jogo.
   - Exiba uma seção "Jogos relacionados" na página de detalhes do jogo usando esse helper.
   - Trate o caso em que um jogo não tem jogos relacionados.
   - Crie testes unitários para o novo helper.
   - Antes de criar o PR, confirme que todos os testes foram aprovados.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot**.

   ![Atribuição do Copilot a uma issue](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Em pouco tempo, você verá um 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando nela.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando na segunda solicitação. Como o agente de nuvem funciona de modo semelhante a uma pessoa engenheira de software, não é necessário acompanhá-lo continuamente; revise o resultado quando estiver pronto. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Neste exercício, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para execução assíncrona. Ele pode resolver dívida técnica, criar funcionalidades ou ajudar a migrar código entre frameworks.

Você explorou:

- como personalizar o ambiente de geração de código.
- como garantir que as operações sejam executadas com segurança.
- a importância de issues com escopo claro.
- como atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha, você pode criar e usar agentes personalizados. O [agente de nuvem também pode usar servidores MCP][cloud-agent-mcp] e as instruções personalizadas exploradas em módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem][cloud-agent-best-practices]

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
