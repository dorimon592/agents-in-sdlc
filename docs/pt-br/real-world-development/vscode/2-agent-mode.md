---
title: "Exercício 2 - Adicionar funcionalidade com o Agent Mode do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até uma alteração simples costuma envolver vários arquivos e tarefas, como executar testes. Em geral, uma pessoa desenvolvedora localiza os arquivos necessários, faz as alterações, executa testes, depura problemas, descobre quais arquivos faltaram e repete o processo.

O Agent Mode do Copilot foi criado para trabalhar com mais autonomia no IDE. Ele explora a estrutura do projeto, faz as alterações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos usá-lo para adicionar funcionalidade ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente executa tarefas no IDE e permite ciclos de feedback rápidos. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o Agent Mode do Copilot pode explorar o projeto, identificar os arquivos relevantes e coordenar alterações.
- o Agent Mode do GitHub Copilot pode implementar recursos na interface e na camada de dados.
- revisar as alterações e os testes gerados pelo Agent Mode antes de integrá-los à base de código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem os jogos por categoria e editora. Você já adicionou um helper de editoras no exercício anterior; agora, concluirá o trabalho restante na camada de dados, na interface e nos testes com o Agent Mode do Copilot.

## Execute o site da Tailspin Toys

Antes de alterar o projeto, explore o site para entender como ele funciona. A Tailspin Toys é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. O aplicativo Astro gera páginas como HTML estático durante a compilação. As páginas consultam diretamente um banco de dados SQLite local por meio do Drizzle ORM; não há uma API de backend separada nem um framework de interface no cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade usa pequenos elementos `<script>` do Astro com APIs padrão do DOM.

### Inicie o site

Há uma tarefa `npm run dev` que inicia o servidor de desenvolvimento do Astro.

1. Volte ao codespace. Você deverá estar em `main` depois do exercício anterior.
2. Abra um terminal com <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, você verá uma mensagem semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo pedir para abrir `http://localhost:4321` em uma janela do navegador, feche-a selecionando **x**.

5. Abra o site mantendo <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) pressionada e selecionando `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, selecionar o link `localhost` no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`, um túnel privado para o servidor do codespace.

### Explore o site

Na página inicial, a lista apresenta jogos de tabuleiro, títulos, imagens e descrições. Ao selecionar um jogo, você abre a página de detalhes, que apresenta outras informações, como título, descrição, editora e categoria.

## Explore o backlog com o Copilot

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de iniciar os exercícios abaixo, volte ao codespace, abra o painel Copilot Chat e selecione **New Chat** para começar uma conversa limpa. O modo e o modelo variam conforme o exercício; as etapas indicam quando isso importa.

O site funciona, mas queremos aprimorá-lo. Peça ao GitHub Copilot que mostre o backlog de issues criado automaticamente ao configurar o projeto a partir do modelo.

1. No seletor de agentes da visualização Chat, selecione **Agent**. O agente planeja e implementa alterações em vários arquivos, executa comandos de terminal e chama ferramentas.

   ![Captura de tela do seletor de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Na lista de modelos disponíveis, selecione **Claude Sonnet 4.5**.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo específico. Usamos Claude Sonnet 4.5 ao criá-lo para manter as sugestões relativamente consistentes. Como os LLMs são probabilísticos, suas sugestões podem ser diferentes das mostradas aqui; isso é normal.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente e ainda concluir a tarefa.

3. Envie ao Copilot este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para permitir que o comando liste todas as issues.
5. Revise a lista e a priorização geradas pelo Copilot.

## Revise os arquivos de instruções

Antes de pedir ao agente que gere código, revise o arquivo de instruções que fornecerá contexto. Você usará o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como abordar alterações no site.

1. No codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações sobre a arquitetura, os princípios de design de componentes, testabilidade e acessibilidade, além dos links para instruções sobre Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem referenciar outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao arquivo de instruções, permitindo reutilizar instruções menores e exemplos.

## Implemente a filtragem

Para concluir a filtragem, serão necessárias pelo menos três alterações:

- adicionar ou aprimorar a lógica de filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento de filtragem.
- atualizar a página de listagem de jogos com a interface de filtragem.

Os testes também precisam passar antes de integrar as alterações. O Agent Mode pode executar essas tarefas.

1. Continue a conversa atual ou selecione **New Chat**.
2. Selecione **Add Context**, **Instructions** e **ui** para adicionar o arquivo de instruções.

   ![Captura de tela da seleção do arquivo de instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado.

   ![Captura de tela do seletor de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado.
5. Envie este prompt para implementar a funcionalidade solicitada na issue do backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorar o projeto e localizar os arquivos da camada de dados, da interface e dos testes. Em seguida, ele alterará arquivos e executará testes.

   ![Captura de tela do Copilot explorando arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot explorará o projeto, alterará arquivos e executará testes. Esse processo pode levar alguns minutos, dependendo da tarefa e da base de código. Durante o trabalho, talvez apareçam os controles **Keep** e **Undo** no editor. Quando terminar, haverá um desses controles para cada alteração; não é necessário selecionar nenhum enquanto o trabalho estiver em andamento.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Captura de tela de uma caixa de diálogo do Copilot Chat pedindo confirmação para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Talvez haja pausas e falhas em alguns testes durante o processo. O Copilot alterna entre a geração de código e os testes até concluir a tarefa e não detectar erros.

   ![Captura de tela de uma sessão completa do Copilot Agent Mode](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado para identificar possíveis problemas.

> [!CAUTION]
> É sempre importante revisar o código gerado pelo Copilot ou por qualquer ferramenta de IA.

10. Volte ao navegador com o site em execução e explore o recurso.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Neste exercício, você usou o Agent Mode do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Você aprendeu como:

- o Agent Mode pode implementar recursos na interface e na camada de dados.
- o Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- revisar alterações e testes antes de integrá-los à base de código.

Agora, vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício extra: implementar paginação

À medida que a lista de jogos cresce, será necessário adicionar paginação. Use as habilidades deste exercício e peça ao Copilot que implemente a paginação. Considere:

- seguir as práticas existentes e usar os arquivos de instruções.
- decidir se a pessoa usuária poderá escolher o tamanho da página ou se ele será fixo.
- fornecer ao Copilot as orientações necessárias no prompt.
- iterar com o GitHub Copilot, pedindo alterações e fornecendo contexto. Esse processo é normal ao trabalhar com o Copilot.

## Recursos

- [Copilot: modos ask, edit e agent — o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
