---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo de agente, o GitHub Copilot trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece na sua máquina em tempo real, o Copilot Chat no VS Code mostra cada chamada de ferramenta e edição de arquivo. Você pode revisar diffs, aceitar ou rejeitar alterações e orientar a conversa com prompts de acompanhamento sem sair do editor.

Neste exercício, você vai:

- acompanhar o trabalho do agente de acessibilidade em tempo real.
- revisar e aceitar as edições propostas.
- orientar o agente durante a sessão para adicionar um seletor de modo claro junto com o modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade do [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho acontece, a equipe de design solicita também um modo claro para quem considera o tema padrão escuro demais. Em vez de começar uma conversa do zero, você orientará a sessão existente para ampliar o trabalho.

## Acompanhe o progresso do Copilot

A visualização Copilot Chat mostra as etapas executadas pelo agente: arquivos explorados, comandos executados e alterações propostas. Cada edição aparece na conversa com um diff e os controles **Keep** e **Undo**.

1. Volte à visualização Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra a conversa e acompanhe as chamadas de ferramentas, leituras, edições e comandos de terminal solicitados.
3. Quando o agente propuser uma edição, o diff aparecerá na conversa:
   - Selecione **Keep** para aceitar a alteração.
   - Selecione **Undo** para revertê-la e permitir que o agente tente novamente.
4. Abra **Files changed**, na parte superior do chat, para ver todos os arquivos alterados durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão, por exemplo, para executar um comando de terminal, o agente exibirá o pedido **Allow**. Aprove somente os comandos com os quais você se sente confortável.

5. Aguarde o agente concluir o modo de alto contraste. Ele apresentará uma mensagem final com o resumo das alterações.

## Oriente o agente para adicionar o modo claro

Agora que o modo de alto contraste está pronto, você continuará a conversa para adicionar um seletor de modo claro. Como é a mesma sessão, o agente mantém o contexto das alterações anteriores, dos componentes editados, da abordagem de estilo e do padrão de armazenamento local.

1. Na entrada do Copilot Chat, envie este prompt de acompanhamento:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. O agente retomará o trabalho, explorará as alterações já feitas e proporá mudanças para o modo claro.
3. Revise cada nova edição e use **Keep** ou **Undo**, como fez para o modo de alto contraste.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar os seletores de uma forma que você não quer —, interrompa-o com **Stop**, ao lado da entrada do chat, e envie uma mensagem de acompanhamento para esclarecer.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que recomeçar. Use prompts de acompanhamento para solicitar ajustes ("simplify the CSS — use CSS variables instead of duplicating selectors"), corrigir o rumo ("don't add a third toggle, integrate light mode into the existing settings panel") ou ampliar o escopo ("also add keyboard shortcuts for both toggles").

## Revise o conjunto de alterações

Antes do commit, revise os arquivos alterados pelo agente.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista de arquivos alterados. Você deverá ver componentes Astro, estilos e testes relacionados.
3. Abra alguns arquivos e examine os diffs. Confirme se as práticas de acessibilidade do agente estão aplicadas, como atributos ARIA, navegação por teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou alterações e enviou prompts de acompanhamento para ampliar o trabalho sem perder o contexto.

Neste exercício, você:

- acompanhou o trabalho do agente em tempo real.
- aceitou e rejeitou edições individuais.
- orientou a sessão durante o trabalho com prompts de acompanhamento.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em um pull request e revisará as alterações produzidas pelo agente personalizado local.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
