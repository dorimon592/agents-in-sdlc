---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Explore os modos de agente: use Plan para definir uma abordagem, Autopilot para criar a filtragem a partir de uma issue e Interactive para revisar e verificar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Começamos adicionando um pequeno recurso ao projeto. Alterações maiores, porém, exigem um processo mais robusto. O aplicativo GitHub Copilot foi criado para trabalhar com o fluxo existente de uma organização e ajudar a desenvolver as soluções certas da maneira adequada. Nesta primeira de várias lições, você seguirá um processo de desenvolvimento orientado por agentes: partirá de uma issue para criar um recurso, confirmará que o código é válido e que o recurso funciona como esperado e, por fim, preparará as alterações para integração ao projeto.

> [!NOTE]
> Você usará a mesma sessão ao longo do fluxo de trabalho desse recurso. Normalmente, você usaria sessões ou PRs diferentes para cada tipo de arquivo, mas vamos simplificar para destacar os conceitos principais.

Nesta lição, você vai:

- iniciar uma nova sessão de agente a partir de uma issue do GitHub.
- definir os requisitos no modo **Plan**.
- implementar o recurso no modo **Autopilot**.
- revisar o código.
- validar manualmente o recurso em um canvas de navegador.

Ao continuar o trabalho nesse recurso, você atualizará as instruções do repositório, personalizará a skill `quality-checks`, adicionará validação MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo e os visitantes precisam filtrar jogos por categoria e editora. A issue do backlog descreve o recurso, mas detalhes como a combinação de categorias precisam ser definidos antes de escrever código. Você usará o modo Plan para resolver essas decisões e, depois, autorizará uma implementação com escopo limitado no modo Autopilot.

## Contexto

Adicionar agentes de programação de IA ao fluxo de desenvolvimento não muda os fundamentos — na verdade, torna-os ainda mais importantes. O processo de muitos desenvolvedores se parece com este:

1. Abrir uma issue registrada com os detalhes do trabalho.
2. Criar um plano do que precisa ser desenvolvido.
3. Desenvolver e revisar o código.
4. Executar testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois da revisão do código e da aprovação da integração contínua, integrar o código.

> [!NOTE]
> Os detalhes exatos variam de acordo com a equipe e a organização, mas a maioria dos processos segue uma variação dessas etapas.

Ao seguir esse processo padrão, você garante que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de validação que o código escrito manualmente.

## Modos de sessão

O **modo de sessão** controla o nível de autonomia do agente. Você pode defini-lo no menu suspenso abaixo do campo de prompt e alterá-lo a qualquer momento:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano primeiro. Você o revisa e aprova antes de o agente executá-lo.
- **Autopilot**: o agente trabalha de forma totalmente autônoma, escrevendo código, executando testes e iterando sem aguardar sua entrada.

Comece no modo Plan, revise o plano e, em seguida, use o Autopilot para implementá-lo.

## Inicie uma sessão a partir da issue

Antes de começar, confirme que o PR de avaliação por estrelas foi integrado e que sua branch `main` local está atualizada.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys e, na navegação superior, selecione **Issues**. Abra **Allow users to filter games by category and publisher**. Essa issue já existe no backlog; não é necessário criar outra.
2. Selecione **New session** e escolha uma **new working tree** baseada na `main` atualizada.

   ![Visualização da issue no aplicativo GitHub Copilot com uma seta apontando para o botão New session](../../../_images/app-new-session-from-issue.png)

3. Confirme que a issue está anexada à sessão e selecione **Plan** no seletor de modo.

> [!NOTE]
> Se o Node.js ou o npm não forem encontrados, peça ao agente para executar `node --version` e `npm --version`. Se os comandos funcionarem no terminal, mas não na sessão, verifique o PATH ou a configuração do gerenciador de versões e feche o aplicativo completamente antes de reabri-lo. Tente novamente a operação e as verificações com falha; não reinstale o npm apenas porque o agente não o encontrou.

## Planeje o recurso de filtragem

O planejamento permite revisar a abordagem antes que o Copilot escreva código. Como você iniciou a sessão pela issue, o Copilot já tem a solicitação como contexto. Envie:

```plaintext
Build this feature.
```

Responda às perguntas do Copilot e compare o plano com os critérios de aceitação da issue. Confirme que ele abrange a filtragem por categoria e editora, controles acessíveis, alterações de acesso a dados e testes. Discuta os comportamentos que não estiverem claros, como combinar várias categorias ou o que acontece quando nenhum jogo corresponde.

O plano deve incluir lint, testes unitários, testes E2E e verificação de tipos com as ferramentas existentes do projeto. Mantenha o escopo na implementação e nos testes da filtragem; você criará o PR após concluir o fluxo de qualidade. Peça alterações no plano antes de aprová-lo e guarde a URL da issue e os esclarecimentos acordados para a validação posterior.

## Aprove o Autopilot explicitamente

Quando estiver satisfeito com o plano, selecione **Approve and implement this plan** ou a opção equivalente na sua versão. Confira o indicador de modo, sem presumir que a aprovação o altera automaticamente. Confirme que ele mostra **Autopilot**.

O Copilot começará a implementação, percorrendo o plano, gerando código e executando testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano primeiro. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas. Pare somente os servidores que você iniciou.

## Revise e verifique a implementação

Assim como qualquer outro código, o código gerado precisa ser revisado antes de ser integrado.

1. Abra **Changes** e inspecione a implementação da filtragem e os testes.
2. Compare o resultado com a issue e com os esclarecimentos aprovados, incluindo combinações de categorias e editoras. Confira se as alterações seguem as instruções do repositório.
3. Examine os resultados de lint, testes unitários, testes E2E e verificação de tipos. Uma verificação ignorada não conta como aprovada.
4. Resolva as falhas e execute novamente as verificações afetadas antes de aceitar a implementação. A configuração E2E do Playwright compila e serve uma prévia, podendo reutilizar um servidor local; confirme que o servidor testado pertence a esta worktree, não a uma lição anterior.

## Explore a nova funcionalidade

O código parece correto, mas será que funciona? Vamos iniciar o aplicativo e abrir o site em um canvas de navegador.

1. Peça ao Copilot que inicie o aplicativo e abra a página no canvas:

   ```plaintext
   Start the app and open it in the browser canvas.
   ```

2. Aguarde o aplicativo iniciar e a janela do navegador aparecer dentro do aplicativo Copilot.
3. Selecione uma categoria e confirme que os resultados correspondem. Limpe o filtro e selecione uma editora; confirme os resultados.
4. Combine os filtros de categoria e editora e compare os resultados com a issue e o plano aprovado. Se houver suporte a várias categorias, verifique o comportamento combinado acordado.
5. Teste uma combinação sem jogos correspondentes e confira o estado vazio. Limpe ou redefina os filtros e confirme que a lista completa volta a aparecer.
6. Quando terminar, peça ao Copilot que pare o servidor de desenvolvimento iniciado para esta sessão:

   ```plaintext
   Stop the dev server and close the browser canvas.
   ```

## Mantenha esta sessão para as próximas lições

Ainda não crie nem integre o PR de filtragem. Mantenha esta sessão, a worktree e a branch para as Lições 4 a 8. Em seguida, você aplicará instruções personalizadas ao código de filtragem e adicionará melhorias à skill e ao agente de QA antes de criar e integrar o PR combinado na Lição 8. Depois de parar o servidor de desenvolvimento, você pode deixar a sessão ociosa e retomá-la na próxima lição.

## Resumo e próximos passos

Você usou diferentes modos de agente para desenvolver e revisar um recurso. Nesta lição, você:

- iniciou uma sessão de agente a partir de uma issue do GitHub.
- definiu os requisitos no modo **Plan**.
- implementou o recurso no modo **Autopilot**.
- revisou o código.
- validou manualmente o recurso em um canvas de navegador.

Em seguida, você aprenderá mais sobre a geração de código e como garantir que ela siga práticas documentadas ao [usar instruções personalizadas][next-lesson].

## Recursos

- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
