---
title: "Exercício 5: Itere sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revise o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design testar e implementasse recursos de acessibilidade, incluindo controles para os modos de alto contraste e claro. Vamos explorar as alterações e, se necessário, enviar comentários ao Copilot para aprimorá-las.

### Cenário

Como já destacamos várias vezes, os fundamentos de design de software e DevOps não mudam com a chegada da IA generativa. Sempre revisamos o código gerado e seguimos o processo DevOps normal. Vamos revisar as sugestões do GitHub Copilot para a documentação, os jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot trabalha de forma assíncrona e sem supervisão, há restrições de segurança para proteger o ambiente. Entre elas:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch usada para o trabalho.
- O agente de nuvem é executado no GitHub Actions, em um ambiente isolado e temporário.
- Os fluxos de trabalho do GitHub Actions precisam de aprovação humana antes de serem executados.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo servidores MCP.

## Revise a documentação gerada

Vamos começar explorando o primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot: o PR que adiciona documentação ao código. Use a interface padrão de PR do GitHub.com.

> [!NOTE]
> Ao abrir o PR, talvez você veja um aviso de que o Copilot foi bloqueado por um firewall. **Isso é esperado**, pois o acesso a recursos externos, incluindo servidores MCP externos, é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de pull requests.
3. Abra o PR com um título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Aguarde até que o trabalho termine. Isso pode levar alguns minutos; aproveite para fazer uma pausa ou refletir sobre o que aprendeu.

4. Quando o PR estiver pronto, selecione a guia **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, que inclui novos comentários TSDoc e outra documentação. As alterações exatas podem variar.

   Procure comentários TSDoc, convenções de TypeScript e comentários de cabeçalho. Eles vêm dos arquivos de instruções personalizadas revisados no início do Exercício 2.

6. Quando terminar a revisão, volte à guia **Conversation** e role para baixo.
7. Procure um indicador de que alguns fluxos aguardam aprovação.
8. Se houver fluxos aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos deverão ser enfileirados na seção de verificações do pull request. Se tudo estiver certo, as verificações do projeto para o único aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.

## Peça alterações ao GitHub Copilot

Trabalhar com o Copilot em um pull request não é uma via de mão única. Você pode mencioná-lo em comentários do PR ou em comentários de linha no código, como faria com outra pessoa da equipe. O Copilot verá esses comentários e iniciará uma nova sessão para resolvê-los. Como os resultados são não determinísticos, não há um texto específico a recomendar. Algumas sugestões:

- adicionar comentários de cabeçalho no início de cada arquivo de código, descrevendo brevemente sua finalidade.
- adicionar comentários TSDoc a arquivos TypeScript e Astro.
- criar um README com a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**. Use uma das ideias acima ou solicite outra melhoria na documentação.
2. Selecione **View Session** para acompanhar o trabalho. Observe o Copilot iniciar uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao PR.

   ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, deverá aparecer um novo commit no pull request.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Depois, você pode marcar o PR como pronto para revisão e mesclá-lo à branch principal.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

Agora, volte ao PR criado pelo Copilot para resolver a issue sobre a exibição de jogos relacionados na página de detalhes.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Quando terminar a revisão, volte à guia **Conversation** e role para baixo.
6. Procure um indicador de que alguns fluxos aguardam aprovação.
7. Se houver fluxos aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

8. Os fluxos deverão ser enfileirados na seção de verificações do PR. Se tudo estiver certo, as verificações do projeto para o único aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** você pode selecionar esta branch no Codespace para testar manualmente o recurso de jogos relacionados. Abra o terminal e execute os comandos abaixo, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, `copilot/fix-8`:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou o recurso de jogos relacionados! Como antes, você pode iterar com o agente de nuvem e pedir ajustes, por exemplo, alterar a quantidade de jogos exibidos ou adicionar comentários de cabeçalho e TSDoc. Lembre-se de que a issue foi atribuída **antes** da atualização das instruções personalizadas. Para solicitar alterações, adicione um comentário na guia **Conversation**; o Copilot o verá e iniciará uma nova sessão.

## Revise os recursos de acessibilidade

Por fim, revise os recursos de acessibilidade implementados pelo agente personalizado de acessibilidade. Este PR deve incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no centro de controle no Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Aguarde até que o trabalho termine; isso pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, prestando atenção a:
   - componentes da interface para alternar entre os modos.
   - uso do armazenamento local para manter as preferências do usuário.
   - alterações de CSS ou estilo para os modos de alto contraste e claro.
   - atributos de acessibilidade, como rótulos ARIA e navegação por teclado.
   - código JavaScript/TypeScript que gerencia a alternância dos modos.
6. Quando terminar a revisão, volte à guia **Conversation** e role para baixo.
7. Procure um indicador de que alguns fluxos aguardam aprovação.
8. Se houver fluxos aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos deverão ser enfileirados na seção de verificações do PR. Se tudo estiver certo, as verificações do projeto para o único aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** selecione esta branch no Codespace para testar manualmente os recursos de acessibilidade. Abra o terminal e execute os comandos abaixo, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os controles dos modos de alto contraste e claro no navegador. Confirme que funcionam e persistem após recarregar a página.

Observe como o agente personalizado de acessibilidade ajudou o Copilot a implementar esses recursos seguindo práticas recomendadas de acessibilidade. Se encontrar problemas ou oportunidades de melhoria, mencione **@copilot** em um comentário e solicite atualizações, como nos PRs anteriores.

## Exercício opcional — continue delegando

O agente de nuvem funciona melhor quando você pode atribuir a ele itens reais do backlog e se concentrar em outras tarefas. Para criar esse hábito, registre mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- implementar paginação na lista de jogos.
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu a trilha do agente de nuvem. Ao longo das lições, você:

- **inspecionou os arquivos de instruções incluídos no repositório** para observar seu efeito no trabalho do agente de nuvem.
- **atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **criou e usou um agente personalizado** de acessibilidade para adicionar controles dos modos de alto contraste e claro.
- **usou a página de agentes do Copilot como centro de controle** para acompanhar e orientar a sessão de acessibilidade.
- **revisou e iterou sobre os pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando os fluxos.

## Revisão e próximos passos

Você concluiu a trilha do agente de nuvem. Para continuar explorando, as outras trilhas complementam o que você praticou:

- 🖥️ **[Trilha do VS Code](../../vscode/)** — explore o modo agente do Copilot Chat e a integração MCP diretamente no IDE.
- 💻 **[Trilha da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal usando os modos Plan e Autopilot, skills de agente, um agente de QA personalizado, Playwright MCP, Agent Merge e uma ponte `/delegate` opcional para o agente de nuvem.

No seu repositório, experimente:

- atribuir ao agente de nuvem uma issue de refatoração ou cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra pilha tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
