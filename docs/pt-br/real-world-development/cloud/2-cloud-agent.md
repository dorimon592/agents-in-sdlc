---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

Quase toda organização tem dívida técnica: problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações que ficaram paradas no backlog. O agente de nuvem do GitHub Copilot pode atualizar código e adicionar funcionalidade de forma autônoma. Ao concluir o trabalho, ele cria um pull request de rascunho para revisão. Assim, você pode delegar tarefas repetitivas e se concentrar em outras prioridades.

Neste exercício, você explorará como:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- criar issues com escopo claro.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys quer lidar com parte da dívida técnica. A documentação do código está ausente; como primeiro passo, a equipe quer adicionar comentários TSDoc às funções exportadas.

A equipe de design também quer melhorar a descoberta de jogos. Cada página de detalhes deve mostrar outros jogos da mesma categoria para ajudar as pessoas a continuar navegando. A equipe quer uma implementação inicial para testar a experiência; esse trabalho é um impedimento, mas há issues prioritárias.

Esses trabalhos costumam ser adiados e são bons candidatos para o agente de nuvem. Ele pode trabalhar de forma assíncrona enquanto você cuida de outras tarefas e, depois, você revisa o resultado.

## Apresentação do agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) trabalha em segundo plano de forma semelhante a uma pessoa desenvolvedora. Você pode iniciar o trabalho, por exemplo, [atribuindo uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). O Copilot cria um pull request de rascunho para acompanhar o progresso, prepara o ambiente e inicia a tarefa. Você pode acompanhar a sessão enquanto o trabalho acontece ou depois de concluído. Quando a solução estiver pronta para revisão, o Copilot marcará você no pull request.

## A importância de instruções com escopo claro

O GitHub Copilot não é mágico: você ainda precisa pensar. Mesmo tarefas aparentemente simples podem ter complexidades. [Defina claramente o escopo do trabalho para o agente de nuvem][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software continuam os mesmos.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram instruções personalizadas. Se ainda não as explorou, leia estes arquivos antes de atribuir trabalho ao agente de nuvem:

- `.github/copilot-instructions.md`: examine **Code standards**, principalmente as convenções de TypeScript e as expectativas para comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que aplica as instruções a arquivos `**/*.test.ts`.

Ao atribuir a issue `Code lacks documentation`, observe os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentário no pull request. As instruções são a origem dessas diretrizes; voltaremos a esse ponto ao revisar os PRs.

## Configure o ambiente de desenvolvimento do agente de nuvem

Criar código normalmente exige um ambiente específico e scripts de configuração. O agente de nuvem usa [GitHub Actions][github-actions] para preparar o ambiente. Você pode personalizá-lo com um [workflow de configuração][setup-workflow] em `.github/workflows/copilot-setup-steps.yml`, executado antes do início do trabalho.

O workflow de configuração já está preparado neste laboratório. Ele instala Node.js e dependências do projeto, baixa os navegadores do Playwright para testes de ponta a ponta e configura o banco de dados SQLite da aplicação Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot terá um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório como parte da configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório nas etapas de configuração, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo: Node.js (agora todo o aplicativo usa Astro + Drizzle/libSQL)
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

      # Migre e popule o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo se parece com outros workflows do GitHub, mas tem alguns pontos importantes:

- Ele contém um único job, `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar o trabalho no pull request.
- O gatilho `workflow_dispatch` permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para testar a configuração sem esperar o agente executá-la.

## Adicione documentação

A documentação costuma estar desatualizada ou ausente, o que aumenta a dificuldade de manter uma base de código e integrar pessoas desenvolvedoras. O Copilot pode ajudar a criar documentação e trabalhar nessa issue em segundo plano. Você revisará o resultado em um exercício futuro.

1. Em uma nova guia, abra seu repositório no github.com.
2. Selecione **Issues** e, em seguida, **New issue**.
3. Selecione **Blank issue**.
4. No campo **Title**, digite `Code lacks documentation`.
5. Em **Description**, digite:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

6. Selecione **Create** para criar a issue.
7. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição e selecione **Assign**.

   ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

   ![Detalhes da atribuição da issue ao Copilot](../../../_images/ex4-assign-copilot-details.png)

8. Selecione a guia **Pull Requests** e abra o novo PR, com um título semelhante a `[WIP]: Code lacks documentation`. Se ele não aparecer, aguarde um pouco e atualize o navegador.
9. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista pode levar alguns minutos para aparecer. O Copilot está preparando o ambiente com o workflow descrito anteriormente, analisando o projeto e definindo uma abordagem.

10. Revise as tarefas planejadas.
11. Na linha do tempo do PR, observe a atualização de que o Copilot começou a trabalhar.
12. Selecione **View session**.

   ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

13. Percorra a sessão ativa e observe como o Copilot explora o código, entende o estado do projeto, planeja a abordagem e cria código.

O trabalho pode levar alguns minutos. Um dos objetivos do agente de nuvem é executar tarefas de forma assíncrona para que você possa cuidar de outras atividades. Vamos atribuir outra tarefa ao agente e, em seguida, trabalhar em um recurso do aplicativo.

## Adicione uma seção de jogos relacionados à página de detalhes

Uma vantagem do agente de nuvem é dividir o trabalho: enquanto você se concentra em uma tarefa, o agente trabalha em outra. Vamos pedir que ele adicione jogos relacionados à página de detalhes.

1. Volte ao repositório no github.com, abra **Issues** e selecione **New issue**.
2. Escolha **Blank issue**.
3. No campo **Title**, digite `Show related games on the game details page`.
4. Em **Description**, digite:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

5. Selecione **Create** e, no painel à direita, **Assign to Copilot**; depois, selecione **Assign**.

   ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

Uma reação 👀 no primeiro comentário da issue indica que o Copilot começou a trabalhar.

![O emoji de olhos indica que o Copilot está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O agente de nuvem trabalhará na segunda solicitação. Como ele opera de forma semelhante a uma pessoa desenvolvedora, não é necessário acompanhá-lo continuamente; revise o resultado quando estiver pronto. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Ele permite atribuir issues ao Copilot para execução assíncrona. Você pode usá-lo para tratar dívida técnica, criar funcionalidades ou ajudar a migrar código entre frameworks.

Você explorou:

- personalizar o ambiente de geração de código.
- garantir a segurança das operações.
- definir issues com escopo claro.
- atribuir issues ao Copilot.

Enquanto o agente trabalha, você pode criar e usar agentes personalizados. O [agente de nuvem também pode usar servidores MCP][cloud-agent-mcp] e instruções personalizadas, que foram abordados anteriormente.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]

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
