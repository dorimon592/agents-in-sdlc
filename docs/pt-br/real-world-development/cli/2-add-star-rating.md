---
title: "Lição 2 - Adicionar avaliações por estrelas: uma melhoria rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cards de jogos, revisar as diferenças no navegador encaminhado e integrar seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e experimentou uma conversa, é hora de fazer sua primeira alteração no projeto. Ela será pequena: os jogos já têm uma avaliação por estrelas nos dados, mas os cards da página inicial ainda não a exibem. Você pedirá ao agente que mostre essa informação, revisará a alteração e a integrará como seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma conversa focada em uma branch de recurso.
- pedir ao agente que faça uma pequena alteração no projeto.
- revisar a alteração com `/diff`.
- executar o aplicativo para confirmar a alteração em um navegador encaminhado.
- abrir e integrar seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes do jogo. Os cards da página inicial, porém, mostram somente o título, a categoria, a editora e a descrição. Como primeiro exercício, peça ao agente que exiba a avaliação existente em cada card. Essa alteração pequena e independente é ideal para sua primeira sessão.

## Anatomia de uma conversa

Uma **conversa** é o espaço em que você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo Copilot, uma conversa normal da CLI usa o repositório e a branch atualmente selecionados no terminal, em vez de criar uma worktree dedicada. As conversas salvas permitem retomar a discussão depois, enquanto os arquivos e a branch continuam sendo o estado comum do Git no disco.

Em uma conversa, você verá três elementos: seus prompts e as respostas do agente; a atividade de ferramentas enquanto ele explora e edita arquivos; e as alterações que você pode inspecionar com `/diff`.

## Inicie uma conversa e solicite a alteração

Vamos iniciar uma nova conversa para implementar o recurso.

1. Volte ao seu Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se ainda não estiver em execução, inicie o Copilot com o seguinte comando:

   ```bash
   copilot --yolo
   ```

4. Inicie uma sessão nova com o comando de barra `/new` e pressione <kbd>Enter</kbd>.
5. Use este prompt para solicitar a alteração:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

O Copilot explora o projeto, localiza os arquivos usados para exibir os detalhes dos jogos e cria o código necessário. Você acaba de adicionar um recurso com o Copilot CLI.

## Revise o diff

Toda alteração gerada por IA deve ser revisada antes da integração, mesmo quando for pequena. Vamos examinar as alterações diretamente no Copilot CLI.

1. Digite `/diff` e inspecione todos os arquivos alterados.
2. Confirme que o card do jogo exibe a avaliação numérica quando ela está disponível e `No rating yet` quando `starRating` é `null`.
3. Confirme que os testes abrangem os dois casos.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija qualquer falha.
5. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diff.

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, não determinístico, então seu código exato pode variar. Revise o comportamento em vez de esperar uma implementação idêntica.

## Verifique as alterações

Não basta ler o código e presumir que funciona. Vamos pedir ao Copilot que inicie o site para podermos examinar a interface atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme que os cards dos jogos exibem as avaliações em uma escala de cinco estrelas.
4. Volte ao Copilot e peça que pare o servidor iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abra e integre seu primeiro pull request

Você criou o recurso. Agora é hora de abrir um pull request (PR) para integrar o código ao projeto.

1. Peça ao agente padrão que faça o commit da alteração:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Digite `/pr create`. Ao criar o PR, o Copilot CLI envia o commit existente e mostra a URL do PR.
3. Abra o PR mantendo <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionada e selecionando a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme o merge.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize sua branch `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você entregou sua primeira alteração usando o GitHub Copilot CLI. Você:

- iniciou uma conversa focada em uma branch de recurso.
- orientou o agente a fazer uma pequena alteração nos cards dos jogos.
- revisou as alterações com `/diff`.
- executou o aplicativo e confirmou a avaliação em um navegador encaminhado.
- abriu e integrou seu primeiro pull request.

Em seguida, você [partirá da issue de filtragem e usará os modos Plan e Autopilot][next-lesson] para criar um recurso maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
