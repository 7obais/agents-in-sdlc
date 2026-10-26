---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos dois exercícios, você pediu ao agente na nuvem do Copilot que executasse três tarefas diferentes para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente na nuvem tenha sido desenvolvido para trabalhar de forma autônoma e assíncrona, ainda é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente na nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse controle central, você vê todas as tarefas dos agentes com pull requests (PRs) abertos. Também pode explorar as operações executadas e direcionar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para monitorar as tarefas do agente na nuvem.
- direcionar uma sessão em andamento para solicitar funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a tarefa de criar um modo de alto contraste, a equipe percebeu que também seria interessante adicionar um modo claro. Como o trabalho já envolvia atualizar o estilo do site e adicionar controles de alternância, pareceu lógico incluir esse recurso. Você vai direcionar o trabalho do agente para garantir que ele adicione o modo claro além do alto contraste.

## Revisar as tarefas do agente na nuvem do Copilot

Vamos verificar o estado atual de todas as tarefas atribuídas ao agente na nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
    - Atualizar a documentação do código.
    - Gerar APIs para modificar jogos.
    - Adicionar um modo de alto contraste ao site.
3. Selecione uma das tarefas em andamento. Revise as operações já executadas pelo Copilot. Entre elas, podem estar:
    - Obter o código do repositório.
    - Criar o ambiente de trabalho do Copilot.
    - Configurar servidores MCP.
    - Executar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas listadas variam conforme o estado do trabalho do Copilot e a abordagem adotada.

4. Observe também o painel de pull request (PR) à direita. Ele permite ver o PR e os arquivos alterados para acompanhar o trabalho.

## Direcionar o agente na nuvem

Agora que você viu as tarefas em andamento, vamos pedir ao Copilot que inclua o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente à criação do modo de alto contraste. O título exato dependerá do nome usado pelo Copilot e do estado atual do trabalho.

    ![Sessão de acessibilidade no controle central](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que ela indique que a configuração terminou e o trabalho começou. Você saberá que isso aconteceu quando surgirem mensagens parecidas com as abaixo.
3. No diálogo **Steer active session while Copilot is working**, adicione o prompt a seguir:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarefa do agente na nuvem com o diálogo para direcionar uma sessão ativa em destaque](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe que o Copilot confirma o recebimento do prompt e o incorpora ao fluxo de trabalho.

## Deixar o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada! Ele incorporará a nova solicitação ao fluxo depois de concluir a etapa em andamento quando a mensagem foi enviada.

O trabalho levará alguns minutos, então aproveite para fazer uma pausa e refletir sobre o que aprendeu e explorou até agora.

## Resumo e próximos passos

Esta lição apresentou a página de agentes do Copilot, o espaço central para monitorar e orientar tarefas do agente na nuvem do GitHub Copilot. Nesse controle central, você pode acompanhar todas as tarefas ativas e concluídas, revisar o trabalho realizado e redirecionar tarefas em andamento para ajustar o escopo ou fornecer orientações adicionais.

Você explorou estes conceitos:

- usou o controle central e a página de agentes para monitorar as tarefas do agente na nuvem.
- redirecionou uma sessão em andamento para solicitar funcionalidade adicional.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar nos pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
