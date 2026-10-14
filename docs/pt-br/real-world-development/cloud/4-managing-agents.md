---
title: "Exercício 4 - Monitore e gerencie agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas diferentes para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente de nuvem trabalhe de forma assíncrona e autônoma, é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse centro de controle, você pode ver todas as tarefas dos agentes com pull requests (PRs) abertos, explorar as operações executadas e orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar funcionalidades adicionais.

## Cenário

Depois de pedir ao agente que criasse um modo de alto contraste, a equipe percebeu que também seria uma boa hora para adicionar um modo claro. Como o estilo do site já está sendo atualizado e haverá controles de alternância, faz sentido incluir esse recurso. Você orientará o trabalho do agente para garantir que ele adicione o modo claro além do alto contraste.

## Revise as tarefas do agente de nuvem do Copilot

Vamos conferir o estado atual das tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma das tarefas em andamento e revise as operações executadas pelo Copilot, que podem incluir:
   - obter o código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores MCP.
   - executar várias etapas para concluir a tarefa.

> [!NOTE]
> As etapas exibidas variam de acordo com o estado do trabalho do Copilot e a abordagem escolhida.

4. Observe também o painel de pull request (PR) à direita, que mostra o PR e os arquivos alterados para facilitar o acompanhamento.

## Oriente o agente de nuvem

Agora que você viu as tarefas ativas, peça ao Copilot que inclua um controle de modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada à criação do modo de alto contraste. O título exato depende do nome escolhido pelo Copilot e do estado atual do trabalho.

   ![Sessão de acessibilidade no centro de controle](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que a configuração seja concluída e o trabalho comece. Você saberá que isso aconteceu quando surgirem mensagens semelhantes às mostradas abaixo.
3. No diálogo **Steer active session while Copilot is working**, envie este prompt:

   ```plaintext
   Enquanto trabalhamos no modo de alto contraste, vamos adicionar também um modo claro. Deve haver um controle para esse modo, para que as pessoas possam selecionar a exibição desejada.
   ```

   ![Captura de tela da tarefa do agente de nuvem na página de agentes, destacando o diálogo de orientação da sessão ativa](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe o Copilot confirmar o recebimento e incluir a orientação no fluxo de trabalho.

## Deixe o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada. Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa em andamento quando você enviou a mensagem.

Isso levará alguns minutos. Aproveite para fazer uma pausa e refletir sobre o que aprendeu e explorou até aqui.

## Resumo e próximos passos

Neste exercício, você explorou a página de agentes do Copilot, o centro de controle para acompanhar e orientar as tarefas do agente de nuvem do GitHub Copilot. Nela, você pode acompanhar tarefas ativas e concluídas, revisar o trabalho e redirecionar sessões em andamento para ajustar o escopo ou fornecer orientação adicional.

Você explorou:

- como acompanhar tarefas do agente de nuvem no centro de controle e na página de agentes.
- como redirecionar uma sessão em andamento para solicitar funcionalidades adicionais.

Enquanto o Copilot conclui as funcionalidades de acessibilidade, vamos [iterar sobre os pull requests criados por ele][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
