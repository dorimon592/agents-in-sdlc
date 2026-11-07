---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos exercícios anteriores, você pediu ao agente de nuvem do Copilot que executasse três tarefas para melhorar a experiência e adicionar funcionalidade. Embora o agente trabalhe de forma assíncrona e autônoma, é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] do GitHub.com. Nesse Mission Control, você vê as tarefas com pull requests abertos, explora as operações executadas e orienta uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a tarefa de criar um modo de alto contraste, a equipe percebeu que seria útil adicionar também um modo claro. Como o agente já estava atualizando o estilo do site e adicionando seletores, você orientará o trabalho para incluir o modo claro junto com o alto contraste.

## Revise as tarefas do agente de nuvem do Copilot

Vamos verificar o estado das tarefas atribuídas ao agente de nuvem.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe as tarefas listadas nos painéis principal e esquerdo. Entre elas, você deverá encontrar:
   - atualizar a documentação da base de código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as operações do Copilot, que podem incluir:
   - obter o código do repositório.
   - configurar o ambiente de trabalho.
   - configurar servidores MCP.
   - executar etapas para concluir a tarefa.

> [!NOTE]
> As etapas mostradas variam conforme o estado do trabalho e a abordagem do Copilot.

4. Observe também o painel de pull request à direita, que mostra o PR e os arquivos alterados.

## Oriente o agente de nuvem

Agora que você viu as tarefas em andamento, peça ao Copilot para incluir o modo claro junto com o modo de alto contraste.

1. Selecione a sessão relacionada à criação do modo de alto contraste. O título exato depende do nome escolhido pelo Copilot e do estado do trabalho.

   ![Sessão de acessibilidade no Mission Control](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até o agente concluir a configuração e iniciar o trabalho.
3. Na caixa de diálogo **Steer active session while Copilot is working**, envie este prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Captura de tela da tarefa do agente de nuvem na página de agentes com a caixa de diálogo para orientar a sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot confirma a solicitação e a incorpora ao trabalho.

## Deixe o Copilot trabalhar

O Copilot incorporará a nova solicitação quando terminar a etapa que estava executando. Isso levará alguns minutos; aproveite para refletir sobre o que você aprendeu.

## Resumo e próximos passos

Este exercício apresentou a página de agentes do Copilot, o centro de monitoramento e orientação das tarefas do agente de nuvem. Você pode acompanhar tarefas ativas e concluídas, revisar o trabalho e redirecionar uma sessão em andamento.

Você:

- explorou o Mission Control e a página de agentes.
- redirecionou uma sessão em andamento para solicitar funcionalidade adicional.

Enquanto o Copilot conclui os recursos de acessibilidade, vamos [iterar sobre os pull requests criados][next-lesson].

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
