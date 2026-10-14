---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados, adaptados a tarefas ou domínios do fluxo de desenvolvimento. Ao defini-los em arquivos Markdown na pasta `.github/agents` do repositório, você fornece instruções focadas, práticas recomendadas, padrões de código e conhecimento específico do domínio para orientar o Copilot. As equipes podem registrar sua experiência em agentes reutilizáveis — por exemplo, um agente de acessibilidade que verifica a conformidade com a [WCAG][wcag], um agente de segurança que segue práticas de codificação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos por arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem metadados YAML com pelo menos `name` e `description`, seguidos de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Agentes personalizados e skills de agente

Há alguma sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em Markdown e ensinam uma IA a executar tarefas. A maneira mais simples de distingui-los é: o **agente personalizado** é quem executa o trabalho, e as **skills** são as ferramentas.

Agentes personalizados têm sua própria janela de contexto e podem coordenar skills e até outros agentes. Neste laboratório, o agente personalizado de acessibilidade revisará e atualizará o site de acordo com diretrizes de acessibilidade. Para isso, ele poderia chamar skills para executar um fluxo de pull request ou executar e gerenciar testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente personalizado. Como em qualquer trabalho com IA, teste e itere para descobrir o que funciona em seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Você explorará os seguintes tópicos sobre agentes personalizados:

- como definir agentes personalizados.
- como atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys quer tornar sua plataforma de financiamento coletivo acessível a todas as pessoas, independentemente das capacidades ou preferências visuais. Comentários recentes destacam que algumas pessoas têm dificuldade para ler o tema escuro atual devido ao contraste insuficiente entre texto e plano de fundo. Para resolver isso, a equipe de design solicitou um modo de alto contraste que as pessoas possam ativar e desativar.

Como a acessibilidade é essencial, queremos implementar esse recurso rapidamente. Vamos usar um agente personalizado para gerar a funcionalidade.

## Revise o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado. Vamos revisar seu conteúdo para entender como ele orientará o Copilot.

Volte ao codespace e revise o arquivo do agente:

1. Abra `.github/agents/accessibility.md`.
2. Observe os metadados YAML com os campos `name` e `description`.

> [!CAUTION]
> Os metadados com `name` e `description` são obrigatórios para agentes personalizados.

3. Examine as seções seguintes, que destacam:
   - as responsabilidades principais ao gerar código para um site acessível.
   - as práticas recomendadas de acessibilidade.
   - exemplos de HTML, CSS e JavaScript.
   - problemas e erros comuns.

## Crie e atribua uma issue

O centro de controle é o local central para trabalhar com agentes no seu ambiente. Você pode atribuir tarefas ao agente de nuvem do Copilot, acompanhar o trabalho e fornecer novas orientações. Vamos atribuir uma tarefa para criar o modo de alto contraste.

1. Acesse seu repositório.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir o diálogo de nova issue.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Add high contrast mode to website`.
6. No campo **Description**, digite:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir o diálogo de atribuição.
9. Selecione **Accessibility agent** na lista de agentes personalizados.

    ![Captura de tela da atribuição do agente de nuvem, com o agente personalizado e Accessibility destacados](../../../_images/ex5-select-custom-agent.png)

10. Selecione **Assign**.
11. O Copilot começará a trabalhar na tarefa em segundo plano.

## Resumo e próximos passos

Neste exercício, você explorou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de IA especializados em tarefas e domínios específicos. Eles permitem registrar o conhecimento e os padrões da equipe em agentes reutilizáveis que orientam o Copilot.

Você explorou:

- como agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot implementa o modo de alto contraste, vamos [acompanhar e orientar a sessão do agente][next-lesson] no centro de controle.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar o uso de agentes personalizados na organização][org-custom-agents]
- [Preparar o uso de agentes personalizados na empresa][enterprise-custom-agents]

---

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitoramento e gerenciamento de agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
