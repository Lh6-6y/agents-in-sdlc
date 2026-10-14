---
slug: pt-br/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Construa, verifique e entregue duas mudanças na Tailspin Toys enquanto aprende modos do Copilot CLI, personalização, ferramentas MCP e automação de pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** coloca o GitHub Copilot no seu terminal como um assistente de codificação baseado em agentes. Ele explora bases de código, gera código, executa comandos e conecta ferramentas externas — tudo pela linha de comando, para que você permaneça no fluxo sem trocar para um editor gráfico.

O workshop segue um fluxo contínuo de trabalho da Tailspin Toys:

1. Prepare o projeto no GitHub Codespaces, instale o Copilot CLI e se familiarize com o ambiente.
2. Faça uma mudança focada de classificação por estrelas, revise-a no navegador e mescle seu primeiro pull request (PR) manualmente.
3. Comece pela issue de filtragem, defina a abordagem no modo Plan, construa no modo Autopilot e revise no modo Interactive.
4. Atualize as instruções do repositório e aplique-as ao trabalho de filtragem.
5. Personalize a skill existente `quality-checks` e use-a para executar as verificações do projeto.
6. Adicione o servidor Model Context Protocol (MCP) do Playwright e use-o para explorar a filtragem em um navegador.
7. Crie um agente personalizado de garantia de qualidade (QA) e use-o para revisar requisitos, cobertura e evidência de verificação.
8. Revise a alteração completa de filtragem e use o Agent Merge para o PR de filtragem.
9. Explore comandos de barra úteis para contexto, modelos, compartilhamento e delegação opcional para a nuvem.

Para manter o workshop focado, você criará dois PRs: um de classificação por estrelas e outro de filtragem com atualizações de instruções, atualização da skill, perfil de QA e testes. O fluxo de filtragem e qualidade compartilha uma conversa e uma branch para que você possa aproveitar seu trabalho ao explorar cada ferramenta.

## Lições

| Lição | Tópico | Descrição |
| ------ | ----- | --------- |
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e seu Codespace |
| [1. Instalar o Copilot CLI][ex1] | Instalação | Instale e autentique o Copilot CLI e se familiarize com o ambiente |
| [2. Adicionar classificação por estrelas: uma vitória rápida][ex2] | Primeira mudança | Exiba as avaliações existentes e o fallback nulo, e depois mescle seu primeiro PR |
| [3. Modos de agente: Plan e Autopilot][ex3] | Modos de agente | Planeje a funcionalidade a partir da issue, construa com o Autopilot e revise no modo Interactive |
| [4. Oriente o Copilot com instruções personalizadas][ex4] | Contexto | Explore e atualize instruções, depois aplique-as à filtragem |
| [5. Personalize e use uma skill de quality-checks][ex5] | Verificações reutilizáveis | Explore a skill existente, altere o formato do relatório e execute-a |
| [6. Valide a funcionalidade com o Playwright MCP][ex6] | Observação do navegador | Configure o MCP no CLI e inspecione o comportamento de filtragem |
| [7. Crie e use um agente de QA][ex7] | Requisitos e cobertura | Crie e selecione um perfil especializado e reúna a evidência final de verificação |
| [8. Crie e mescle o PR de funcionalidade][ex8] | Revisão e mesclagem | Revise a mudança completa, crie o PR e use o Agent Merge |
| [9. Comandos de barra no GitHub Copilot CLI][ex9] | Recursos do CLI | Explore contexto, modelos, compartilhamento e delegação opcional para o agente da nuvem |
| [10. Encerramento e próximos passos][ex10] | Resumo | Revise o fluxo de trabalho, as personalizações reutilizáveis e os recursos adicionais |
| [Opcional: incorpore o Foundry][foundry] | Agentes hospedados | Prepare um modelo, implante um agente fundamentado em catálogo e conecte-o ao site |

## Pré-requisitos

Antes de participar deste workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano ativo **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] Permissão para criar um repositório e um Codespace
- [ ] Familiaridade básica com terminal ou linha de comando

> [!TIP]
> Sem plano pago? Estudantes verificados podem receber o GitHub Copilot gratuitamente por meio do [GitHub Education][student-plan]. O plano **Copilot Student** inclui os recursos de agente, MCP, revisão de código e Copilot CLI usados neste workshop.

> [!NOTE]
> Se você estiver usando o Copilot Business ou o Copilot Enterprise, verifique se o administrador habilitou o Copilot CLI para uso.

## Começar

**[Comece pelos pré-requisitos →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
