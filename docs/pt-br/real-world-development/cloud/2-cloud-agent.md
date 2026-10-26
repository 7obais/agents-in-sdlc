---
title: "Exercício 2 - Agente na nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Poucas organizações — se é que alguma — não enfrentam dificuldades com dívida técnica. Ela pode incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de recursos que ficaram paradas no backlog por falta de tempo para implementá-las. O agente na nuvem do GitHub Copilot foi desenvolvido para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele cria um PR de rascunho para que um desenvolvedor possa revisá-lo. Assim, você pode delegar tarefas repetitivas, acelerar o desenvolvimento e se concentrar em atividades mais amplas.

Com o agente na nuvem do Copilot, você vai explorar:

- como personalizar o ambiente de geração de código.
- como garantir que as operações sejam executadas com segurança.
- a importância de issues com escopo bem definido.
- como atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem algumas dívidas técnicas para resolver. Os profissionais contratados para criar a primeira versão do site deixaram a documentação em um estado inadequado — na verdade, ela está totalmente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer facilitar a descoberta de jogos. Ela gostaria que a página de detalhes de cada jogo exibisse jogos relacionados — outros títulos da mesma categoria — para que as pessoas continuem explorando o catálogo. Ainda não é necessário criar uma implementação refinada; a equipe quer apenas uma versão para testes de aceitação da experiência. Isso está bloqueando o trabalho, mas há issues de prioridade maior no momento.

Esses são exemplos de tarefas que podem acabar ficando para depois e que são ótimas candidatas para o agente na nuvem do Copilot. Ele pode trabalhar nelas de forma assíncrona enquanto você se dedica a outras atividades, e depois você revisa o resultado para confirmar que está tudo certo.

## Apresentação do agente na nuvem do GitHub Copilot

O [agente na nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano, de forma semelhante a um desenvolvedor. Assim como no trabalho com outra pessoa, há várias maneiras de solicitar esse trabalho, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um pull request de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode explorar a sessão enquanto ela está em andamento ou depois de concluída. Quando a solução estiver pronta para revisão, o Copilot mencionará você no pull request!

## A importância de instruções com escopo bem definido

Não há mágica no GitHub Copilot — você não pode deixar de pensar no problema. Até operações aparentemente simples podem ficar complexas quando examinadas em detalhe; por isso, [defina com cuidado o escopo das tarefas para o agente na nuvem do Copilot][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe em etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a introdução da IA generativa.

## Instruções personalizadas neste repositório

Nos exercícios anteriores, você conheceu as instruções personalizadas e como elas orientam o Copilot. Se já fez o exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para dar uma olhada rápida. Antes de atribuir trabalho ao agente na nuvem do Copilot, faça uma leitura dos arquivos de instruções existentes neste repositório para poder observar a influência deles mais tarde.

Abra os arquivos a seguir na interface Web do GitHub ou em um Codespace, caso já tenha um aberto:

- `.github/copilot-instructions.md` — revise a seção **Code standards**, especialmente as expectativas para convenções de TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md` — observe o frontmatter `applyTo`, que aplica essas instruções a arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation` ao agente na nuvem na próxima seção, observe no pull request os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentário. Esses elementos são definidos pelos arquivos de instruções. Vamos destacá-los novamente ao revisar os PRs em um exercício futuro.

## Configurar o ambiente de desenvolvimento do agente na nuvem do Copilot

Normalmente, a criação de código exige um ambiente específico e a execução de scripts de configuração para deixar tudo pronto. O mesmo vale para tarefas atribuídas ao Copilot, que trabalha de forma semelhante a um engenheiro de software.

O agente na nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizá-lo criando um [fluxo de trabalho de configuração especial][setup-workflow] no arquivo `.github/workflows/copilot-setup-steps.yml`, executado antes de o agente começar. Assim, ele terá acesso às ferramentas e dependências necessárias. Essa configuração já foi preparada para o laboratório. Ela fornece ao Copilot acesso ao Node.js, às dependências do projeto, aos binários de navegador para testes de ponta a ponta e ao banco de dados SQLite migrado e preenchido com dados do aplicativo Astro:

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
      # Para clonar o repositório como parte da configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não fizer isso, o Copilot clonará o repositório automaticamente após a conclusão das etapas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo - Node.js (o aplicativo inteiro agora usa Astro + Drizzle/libSQL)
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

      # Migra e popula o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo parece com qualquer outro fluxo de trabalho do GitHub, mas tem alguns detalhes importantes:

- Ele contém um único job chamado `copilot-setup-steps`. Esse job é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- Observe o gatilho `workflow_dispatch`, que permite executar manualmente o fluxo de trabalho pela guia **Actions** do repositório. Isso é útil para testar se o fluxo funciona sem esperar que o Copilot o execute.

## Adicionar documentação

Todos entendem a importância da documentação, mas a maioria dos projetos tem informações desatualizadas ou não tem documentação. Esse tipo de dívida técnica costuma ficar sem solução, reduzindo a produtividade e dificultando a manutenção do código ou a integração de novos desenvolvedores. Felizmente, o Copilot é especialmente útil para criar documentação, e esta é uma issue ideal para atribuir ao agente na nuvem. Ele trabalhará em segundo plano para gerar a documentação necessária. Em um exercício futuro, você voltará para revisar o resultado.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir o diálogo de criação de issue.
4. Selecione **Blank issue**.
5. No campo **Title**, digite `Code lacks documentation`.
6. No campo **Description**, digite:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir o diálogo de atribuição.

  ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o pull request (PR) recém-criado, com um título semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer na lista, aguarde um ou dois minutos e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas do Copilot pode levar vários minutos para aparecer no PR. O Copilot está criando o ambiente (executando o fluxo de trabalho mencionado anteriormente), analisando o projeto e determinando a melhor abordagem para a tarefa.

13. Revise a lista e as tarefas que serão executadas.
14. Role a linha do tempo do pull request para baixo. Você deverá ver uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

  ![Exibição da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a página para ver o indicador atualizado.

16. Observe que você pode percorrer a sessão ao vivo e acompanhar como o Copilot está resolvendo o problema. Isso inclui explorar o código e entender o estado atual, pausar para analisar e definir a abordagem adequada e criar código.

O trabalho provavelmente levará alguns minutos. Um dos principais objetivos do agente na nuvem do Copilot é executar tarefas de forma assíncrona para que você possa se concentrar em outras atividades. Vamos aproveitar esse recurso atribuindo outra tarefa ao agente e, enquanto isso, escrevendo código para adicionar funcionalidades ao aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes do jogo

Como destacamos, uma das grandes vantagens do agente na nuvem do GitHub Copilot é poder dividir o trabalho: você pode se concentrar em um conjunto de tarefas enquanto o agente cuida de outro. Talvez adicionar uma seção de jogos relacionados para a equipe de design não demore muito, mas esse tempo poderia ser usado em outras atividades. Vamos atribuir a tarefa ao agente na nuvem do Copilot!

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir o diálogo de criação de issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. No campo **Title**, digite `Show related games on the game details page`.
6. No campo **Description**, digite:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecione **Create** para criar a issue.
8. No lado direito, selecione **Assign to Copilot** para abrir o diálogo de atribuição.

  ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você deverá ver um conjunto de 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar!

![O emoji de olhos indica que o Copilot está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot já está trabalhando na segunda solicitação! O agente na nuvem trabalha de forma semelhante a um engenheiro de software, então você não precisa acompanhá-lo ativamente; basta revisar o resultado quando estiver pronto. Agora vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Esta lição apresentou o [agente na nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para que sejam executadas de forma assíncrona. O Copilot pode ajudar a resolver dívidas técnicas, criar recursos ou migrar código para outra estrutura.

Você explorou estes conceitos:

- como personalizar o ambiente de geração de código.
- como garantir que as operações sejam executadas com segurança.
- a importância de issues com escopo bem definido.
- como atribuir issues ao Copilot.

Enquanto o agente na nuvem trabalha em segundo plano, vamos criar e usar agentes personalizados. O [agente na nuvem do Copilot também pode usar servidores MCP][cloud-agent-mcp] e tem acesso às instruções personalizadas exploradas nos módulos anteriores.

## Recursos

- [Sobre os agentes na nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente na nuvem do Copilot][cloud-agent-best-practices]

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
