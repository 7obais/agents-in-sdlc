---
slug: pt-br/real-world-development/cloud
title: "Agente na nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

O **[agente na nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** permite que o GitHub Copilot trabalhe de forma assíncrona na nuvem. Você atribui tarefas no GitHub, e o agente na nuvem as executa em segundo plano — explorando o repositório, fazendo alterações e abrindo um pull request — enquanto você se dedica a outras atividades.

Nestes exercícios, você adicionará instruções personalizadas para o agente na nuvem e atribuirá uma issue do GitHub para que ele implemente o trabalho. Você revisará e usará agentes personalizados para orientar a abordagem, acompanhará e direcionará sessões pelo painel de agentes e, por fim, revisará os pull requests e fará iterações sobre os resultados.

## Exercícios

| Exercício | Tópico | Descrição |
|----------|-------|-----------|
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e Codespace |
| [1. Instruções personalizadas][ex1] | Contexto | Adicione instruções personalizadas para o agente na nuvem |
| [2. Agente na nuvem][ex2] | Agente assíncrono | Atribua issues ao agente na nuvem do Copilot |
| [3. Agentes personalizados][ex3] | Agentes especializados | Revise e use agentes personalizados |
| [4. Gerenciar agentes][ex4] | Monitoramento | Acompanhe e direcione sessões de agentes |
| [5. Iterar][ex5] | Revisão | Revise PRs, faça iterações no trabalho do Copilot e escolha os próximos passos |

## Pré-requisitos

Antes de participar deste workshop, confira se você tem:

- [ ] Uma conta do GitHub com um plano ativo **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] O agente na nuvem do Copilot habilitado para sua conta ou organização

> [!TIP]
> Não tem um plano pago? Estudantes com status verificado podem obter o GitHub Copilot gratuitamente pelo [GitHub Education][callout-student-plan-education]. O plano **Copilot Student** inclui o agente, o MCP, a revisão de código e os recursos do Copilot CLI usados neste workshop — portanto, você pode concluir todos os ambientes com ele.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> O agente na nuvem está disponível nos planos **Copilot Student, Pro, Pro+, Business e Enterprise** — todos, exceto o Copilot Free. No Copilot Business ou Enterprise, um administrador precisa habilitá-lo para a organização.

> [!NOTE]
> Este ambiente não aborda MCP. Para explorar o uso de servidores MCP com o Copilot, consulte o [ambiente da CLI](../cli/) ou o [ambiente do VS Code](../vscode/).

## Começar

**[Comece pelo Exercício 0: Pré-requisitos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
