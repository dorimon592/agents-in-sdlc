---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Use o Plan para definir uma abordagem, o Autopilot para implementar a filtragem de uma issue e o modo Interactive para revisar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Começamos adicionando um pequeno recurso ao projeto. Alterações maiores, porém, exigem um processo mais robusto. O GitHub Copilot CLI foi criado para se integrar ao fluxo de trabalho da organização e ajudar a desenvolver os recursos certos da maneira adequada. Nesta primeira de várias lições sobre um processo comum de desenvolvimento com agentes, você partirá de uma issue para criar um recurso, confirmar que o código é válido e que o recurso funciona como esperado e, por fim, integrá-lo ao projeto.

> [!NOTE]
> Você usará a mesma conversa e branch ao longo do fluxo de trabalho desse recurso. Em geral, você usaria branches ou PRs diferentes para cada tipo de arquivo, mas neste workshop vamos simplificar para destacar os conceitos principais.

Nesta lição, você vai:

- iniciar uma nova conversa do Copilot a partir de uma issue do GitHub.
- definir os requisitos no modo Plan.
- implementar o recurso no modo Autopilot.
- revisar o código.
- validar manualmente o recurso em um navegador encaminhado.

Durante o trabalho nesse recurso, você atualizará as instruções do repositório, personalizará a skill `quality-checks`, adicionará a validação MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve o recurso, mas detalhes como a combinação de categorias precisam ser definidos antes de escrever código. Você usará o modo Plan para resolver essas decisões e, depois, autorizará uma implementação com escopo limitado no modo Autopilot.

## Contexto

Adicionar agentes de programação de IA ao fluxo de desenvolvimento não muda os fundamentos — na verdade, torna-os ainda mais importantes. O processo de muitos desenvolvedores se parece com este:

1. Começar por uma issue registrada que descreva o trabalho.
2. Criar um plano do que precisa ser desenvolvido.
3. Criar e revisar o código.
4. Executar os testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois da revisão do código e da aprovação da integração contínua, integrar o código.

> [!NOTE]
> Os detalhes exatos variam de acordo com a equipe e a organização, mas a maioria dos processos segue uma variação dessas etapas.

Ao seguir esse processo padrão, você garante que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de validação que o código escrito manualmente.

## Modos de conversa

O **modo de conversa** controla o nível de autonomia do agente. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre os modos:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano e não pode editar os arquivos do projeto.
- **Autopilot**: o agente trabalha de forma autônoma: escreve código, executa testes e itera até concluir a tarefa.

Comece no modo Plan, revise o plano e, em seguida, use o Autopilot para implementá-lo.

## Comece pela issue

Antes de iniciar o trabalho de filtragem, volte ao Codespace e confira se o repositório e o terminal estão prontos.

1. Volte ao Codespace. Se ele estiver parado, reinicie-o.
2. Confirme que o PR de avaliação por estrelas foi integrado.
3. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Atualize `main` e crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicie o Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pressione <kbd>Tab</kbd> duas vezes para abrir a guia **Issues**.
7. Pressione <kbd>A</kbd> para exibir todas as issues.
8. Use as setas para destacar a issue **Allow users to filter games by category and publisher**.
9. Pressione <kbd>C</kbd> para adicionar a issue ao prompt e voltar à guia **Session**.

Observe que o prompt começa com `#7` (ou um número semelhante). O caractere `#` adiciona o contexto de uma issue ou de um pull request (PR) do GitHub.

## Planeje o recurso de filtragem

O planejamento permite definir a abordagem para implementar um recurso ou realizar uma tarefa antes de delegá-la ao Copilot. Para tarefas complexas, é sempre recomendável dedicar um tempo ao planejamento. Vamos mudar para o modo Plan e pedir ao Copilot que crie o plano.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para mudar para o modo Plan. Confirme que o indicador abaixo do prompt mostra **Plan**.
2. Depois da referência à issue adicionada na etapa anterior, digite:

   ```plaintext
   Create a plan for implementing this feature.
   ```

O Copilot começará a trabalhar no plano: primeiro explorará o projeto e, em seguida, determinará a melhor abordagem.

3. Durante o processo, o Copilot talvez pergunte como o recurso de filtragem deve funcionar. Responda de acordo com suas preferências; não há respostas incorretas.
4. Quando o plano estiver pronto, pressione <kbd>Control</kbd>+<kbd>E</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandi-lo.
5. Percorra o plano e revise-o.
6. Peça ao Copilot que altere qualquer parte que não corresponda às suas decisões.

## Aprove o Autopilot

Com o plano escrito e revisado, é hora de implementá-lo. Vamos deixar o Copilot trabalhar no modo Autopilot.

O Autopilot permite que o Copilot itere até considerar que o problema foi resolvido.

1. Selecione **Accept plan and build on autopilot (recommended)** ou a opção equivalente na versão instalada.
2. Confirme que o indicador abaixo do prompt mostra **Autopilot**.
3. Acompanhe o Copilot enquanto ele percorre o plano, gera código e executa testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano antes. Se o Copilot informar que faltam dependências ou que há conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas.

## Revise e verifique a implementação

Assim como qualquer outro código, o código gerado precisa ser revisado antes de ser integrado. Vamos revisar o código e executar o site para confirmar que tudo funciona.

1. Pressione <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar no modo Interactive. Confirme que o indicador não mostra **Plan** nem **Autopilot**.
2. Digite `/diff` e inspecione a implementação da filtragem e os testes.
3. Compare o resultado com a issue e com as decisões tomadas durante o planejamento.
4. Depois da revisão do código, pressione <kbd>Esc</kbd> para sair da visualização do diff.
5. Revise os resultados das verificações do projeto e peça ao Copilot que corrija as falhas.

## Explore a nova funcionalidade

O código parece correto, mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site pela porta encaminhada do Codespaces.

1. Peça ao Copilot que inicie o aplicativo:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Teste os filtros por categoria, por editora e as combinações acordadas no plano.
4. Confirme que o comportamento de redefinição e de resultados vazios corresponde à issue e às suas decisões.
5. Volte ao Codespace e peça ao Copilot que pare o servidor iniciado.

## Resumo e próximos passos

Você usou diferentes modos de conversa para criar e revisar um recurso. Nesta lição, você:

- iniciou uma nova conversa do Copilot a partir de uma issue do GitHub.
- definiu os requisitos no modo Plan.
- implementou o recurso no modo Autopilot.
- revisou o código.
- validou manualmente o recurso em um navegador encaminhado.

Em seguida, você aprenderá mais sobre a geração de código e como garantir que ela siga práticas documentadas ao [usar instruções personalizadas][next-lesson].

## Recursos

- [Autopilot no GitHub Copilot CLI][autopilot]
- [Referência de comandos do Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
