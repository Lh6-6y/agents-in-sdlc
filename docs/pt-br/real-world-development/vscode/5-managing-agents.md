---
title: "Exercício 5 - Monitore e gerencie agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando você usa o GitHub Copilot no modo agente, ele trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real na sua máquina, o Copilot Chat no VS Code mostra cada chamada de ferramenta e cada edição enquanto elas ocorrem. Você pode revisar cada diff no chat, aceitar ou rejeitar alterações e orientar a conversa com prompts complementares, sem sair do editor.

Neste exercício, você vai:

- acompanhar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições propostas nos arquivos.
- orientar o agente durante a sessão para adicionar um controle de modo claro além do alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho acontece, a equipe de design apresenta outro pedido: também quer um controle de modo claro para as pessoas que consideram o tema padrão escuro demais. Em vez de começar uma conversa nova, você orientará a sessão existente para ampliar o trabalho.

## Acompanhe o progresso do Copilot

A visualização Copilot Chat mostra cada etapa do agente: arquivos explorados, comandos executados e alterações propostas. Cada edição aparece no chat com um diff e os controles **Keep** e **Undo**.

1. Volte à visualização Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, role o chat para acompanhar as chamadas de ferramenta. Você verá leituras e edições de arquivos e eventuais comandos de terminal que ele pedir para executar.
3. Quando o agente propuser uma edição:
   - Selecione **Keep** para aceitar a alteração.
   - Selecione **Undo** para desfazê-la e permitir que o agente tente novamente.
4. Você também pode abrir o indicador **Files changed** na parte superior do chat para ver todos os arquivos alterados pelo agente durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão, por exemplo, para executar um comando de terminal, o agente fará uma pausa e exibirá uma solicitação **Allow**. Aprove somente os comandos que considerar seguros.

5. Aguarde até o agente terminar de implementar o modo de alto contraste. Ele deverá apresentar uma mensagem final resumindo as alterações.

## Oriente o agente para adicionar o modo claro

Agora que o modo de alto contraste está pronto, amplie a mesma conversa para adicionar um controle de modo claro. Como você continuará a sessão atual, o agente manterá o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilo e o padrão de armazenamento local.

1. No campo de entrada do Copilot Chat, envie este prompt complementar:

   ```plaintext
   Bom trabalho! Agora adicione um controle de modo claro junto ao controle de alto contraste. O modo claro deve seguir o mesmo padrão: persistir no armazenamento local e estar disponível na mesma área da interface. O usuário deve poder ativar o alto contraste e o modo claro de forma independente.
   ```

2. O agente continuará o trabalho, explorará as alterações já feitas e proporá as adições para o modo claro.
3. Revise cada nova edição conforme ela aparecer, usando **Keep** ou **Undo** em cada diff.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar a interface dos controles de uma maneira que você não quer —, interrompa-o com o botão **Stop** ao lado do campo de entrada e envie uma orientação complementar.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar outra. Use prompts complementares para solicitar ajustes (“simplifique o CSS — use variáveis CSS em vez de duplicar seletores”), corrigir a direção (“não adicione um terceiro controle; integre o modo claro ao painel de configurações existente”) ou ampliar o escopo (“adicione também atalhos de teclado para os dois controles”).

## Revise o conjunto de alterações

Antes de fazer commit, revise rapidamente tudo o que o agente alterou.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações em componentes Astro, estilos e testes relacionados.
3. Abra alguns arquivos alterados e examine os diffs. Confirme que a implementação segue os padrões de acessibilidade do agente personalizado: atributos ARIA, navegação por teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações pelo painel Source Control. Você as publicará [na próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo agente do Copilot Chat para acompanhar e orientar uma sessão em andamento: revisou cada edição de arquivo, aceitou ou rejeitou alterações e enviou prompts complementares para ampliar o trabalho sem perder o contexto.

Neste exercício, você:

- acompanhou o trabalho do agente em tempo real.
- aceitou e rejeitou edições individuais nos arquivos.
- orientou a sessão com prompts complementares.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em um pull request e revisará as alterações produzidas pelo agente personalizado local durante este laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
