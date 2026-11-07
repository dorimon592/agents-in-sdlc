---
title: "Lição 9 - Explorar comandos de barra do Copilot CLI"
description: "Explore comandos de barra para gerenciar o contexto, selecionar modelos, compartilhar sessões e delegar tarefas opcionais à nuvem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como toda boa ferramenta de CLI, o GitHub Copilot CLI inclui vários comandos de barra para interagir com ele. Esses comandos expõem funcionalidades avançadas, informações de bastidores e outras opções de configuração. Você já usou comandos como `/diff`, `/mcp`, `/skills`, `/agent` e `/pr`. Vamos explorar outros comandos úteis.

Nesta lição, você vai:

- usar `/context` e `/compact` para entender como o Copilot gerencia o contexto da conversa.
- usar `/model` para explorar os modelos disponíveis.
- aprender como `/share` pode exportar ou compartilhar uma sessão.
- explorar comandos opcionais para trabalho em paralelo, worktrees e delegação para o agente de nuvem.

## Cenário

Você concluiu o fluxo de trabalho principal da CLI. Agora, vamos explorar alguns recursos adicionais: gerenciamento de contexto, troca de modelo, compartilhamento de sessões e delegação opcional para o [agente de nuvem do Copilot][about-cloud-agent].

## Explore o contexto do Copilot CLI

Ao trabalhar em tarefas maiores ou mais complexas, talvez você atinja o limite da janela de contexto do modelo. O Copilot CLI compacta a conversa automaticamente quando necessário. Você também pode inspecionar ou compactar o contexto manualmente com comandos de barra.

1. Volte ao Codespace e inicie o Copilot CLI na raiz do repositório, se ele ainda não estiver aberto.
2. Digite:

   ```plaintext
   /context
   ```

3. Observe o modelo, o uso atual de tokens e a divisão do contexto entre instruções do sistema, ferramentas, mensagens e espaço livre.
4. Compacte a conversa:

   ```plaintext
   /compact
   ```

5. Digite `/context` novamente e compare os resultados. A diferença pode ser pequena se a conversa ainda for curta.

> [!NOTE]
> O Copilot CLI compacta o contexto automaticamente à medida que a janela se enche. Use `/compact` para escolher o momento. Use `/clear` ou `/new` ao mudar para uma tarefa sem relação com a atual e quiser iniciar uma conversa limpa.

## Escolha seu modelo

Modelos diferentes têm pontos fortes diferentes, e cada pessoa desenvolvedora pode ter suas próprias preferências. O Copilot CLI permite listar e selecionar o modelo desejado.

1. Digite:

   ```plaintext
   /model
   ```

2. Explore os modelos disponíveis e as informações de uso.
3. Mantenha o modelo atual, selecione outro ou pressione <kbd>Esc</kbd> para fechar a lista.

## Compartilhe uma sessão

Trabalhar em equipe e compartilhar aprendizados ajuda todo mundo a aprimorar o uso de ferramentas de IA. O comando `/share` pode exportar uma sessão para um arquivo Markdown ou HTML, criar um link compartilhável ou publicar uma GitHub gist.

1. Digite `/help` e consulte as opções de `/share` na versão instalada.
2. Se quiser compartilhar esta sessão, escolha o destino mais adequado, como `/share file` para exportar um arquivo Markdown local.
3. Revise o conteúdo exportado antes de enviá-lo a alguém ou publicá-lo. As exportações podem incluir prompts, respostas e detalhes do projeto.

Publicar um link ou uma gist é opcional. Não publique conteúdo do repositório ou da conversa que sua equipe não pretenda compartilhar.

## Opcional: amplie o trabalho ou delegue

O fluxo de trabalho principal do workshop está concluído. O Copilot CLI também inclui comandos para tarefas maiores:

- `/fleet` pode dividir subtarefas independentes entre subagentes e executá-las em paralelo.
- `/worktree` pode criar uma worktree Git isolada para outra tarefa.
- `/delegate` pode enviar uma tarefa ao agente de nuvem do Copilot, que trabalha de forma assíncrona e talvez abra um pull request.

Esses comandos são opcionais porque podem criar outras worktrees ou executar trabalho remoto. Antes de experimentá-los, comece uma tarefa nova e bem delimitada e revise os resultados pelo fluxo de trabalho habitual. Para aprender mais sobre trabalho assíncrono com agentes, continue no [workshop do agente de nuvem][cloud-workshop].

## Resumo e próximos passos

Os comandos de barra do Copilot CLI permitem configurar a ferramenta, compartilhar sessões e consultar informações de bastidores. Nesta lição, você:

- usou `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa.
- usou `/model` para explorar os modelos disponíveis.
- aprendeu como `/share` pode exportar ou compartilhar uma sessão.
- explorou comandos opcionais para trabalho em paralelo, worktrees e delegação para o agente de nuvem.

Há muitos outros comandos de barra disponíveis e ainda mais recursos para explorar no Copilot CLI. Vamos encerrar nossa jornada [revisando o que aprendemos][next-lesson] e conhecendo próximos passos para continuar aprendendo.

## Recursos

- [Referência de comandos do Copilot CLI][cli-reference]
- [Gerenciamento de contexto no Copilot CLI][context-management]
- [Sobre o agente de nuvem do Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
