---
title: "Exercício 1 - Instruções personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Modo de agente →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica — ou se houver informações de contexto que o Copilot deva conhecer —, você precisa garantir que elas estejam disponíveis. Uma das principais formas de fornecer orientações sobre a geração de código são os [arquivos de instruções][instruction-files].

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio das instruções personalizadas do repositório e de arquivos de instruções específicos por caminho.
- gerar a primeira parte dos dados da filtragem (um helper de editoras) com as instruções *atuais*.
- adicionar um novo padrão válido para todo o repositório em `.github/copilot-instructions.md`.
- executar o mesmo prompt novamente e observar o código gerado adotar o novo padrão.
- fazer commit das instruções e da parte de filtragem em `main` para que o Copilot possa usar as orientações atualizadas no próximo exercício.

> [!CAUTION]
> O código gerado pode não seguir todos os padrões definidos. O Copilot é não determinístico. O objetivo é observar a *tendência* de comportamento depois de atualizar as instruções, não obter uma saída idêntica caractere por caractere.

## Arquivos de instruções

### Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para suas práticas. Eles incluem:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar o modo escuro e ter uma aparência moderna.
- O código deve ser documentado com comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de código e seus requisitos. Elas ajudam a orientar sugestões e trechos de código. Você pode especificar convenções de código, bibliotecas e tipos de comentários desejados, além de criar instruções para todo o repositório ou para tipos específicos de arquivo.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações sobre o projeto relevantes para a maioria das solicitações feitas pelo chat ou pela CLI, como a stack, uma visão geral do que está sendo criado, boas práticas e outras orientações globais.
- `.github/instructions/*.instructions.md`, arquivos que fornecem orientações para tarefas ou tipos específicos de arquivo, como TypeScript, Astro, um componente de interface ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para autocompletar ou sugerir a próxima edição.
>
> Ao gerar código, o Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam tanto as instruções do repositório quanto os arquivos `*.instructions.md` com frontmatter `applyTo`.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], como `AGENTS.md` e `CLAUDE.md`.

### Boas práticas para gerenciar arquivos de instruções

Uma discussão completa sobre a criação de arquivos de instruções está fora do escopo deste workshop, mas os exemplos do projeto mostram uma abordagem representativa:

- Mantenha `copilot-instructions.md` focado nas orientações do projeto, como o que está sendo desenvolvido, a estrutura do projeto e os padrões globais de código.
- Use arquivos `*.instructions.md` para tipos específicos de arquivo (testes unitários, componentes Astro, camada de dados) ou tarefas.
- Use linguagem natural e orientações claras. Inclua exemplos do que o código deve ou não deve fazer.

Não existe uma única forma de criar arquivos de instruções, assim como não existe uma única forma de usar IA. Experimente para descobrir o que funciona no seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter uma coleção robusta de arquivos de instruções. Ao explorar os arquivos deste projeto, você verá orientações para diversas tarefas, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface apresenta esse recurso de forma diferente, como **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. A lição do ambiente em que você está indicará quando for relevante.
>
> Para encontrar modelos ou um ponto de partida, explore o [awesome-copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções personalizadas deste projeto

Leia os arquivos de instruções fornecidos com o repositório: um `copilot-instructions.md` principal e vários arquivos `*.instructions.md` para diferentes tarefas. Abra-os no editor ou na interface web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Observe a descrição breve do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas de **GitHub Actions Workflows**. Elas se aplicam a qualquer interação com o Copilot.
3. Explore a pasta `.github/instructions` e observe as instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md` e observe `applyTo`, um padrão glob relativo à raiz do repositório. Neste caso, ele corresponde a arquivos de teste TypeScript, como `**/*.test.ts`.
5. Observe as instruções específicas para criar testes unitários.
6. Abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Isso permite dividir instruções maiores em arquivos menores e reutilizáveis e indicar exemplos ao Copilot. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` descreve os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Crie uma branch para as alterações

Vamos seguir boas práticas e criar uma branch para as alterações.

1. Volte ao codespace do exercício anterior.
2. Abra um terminal com <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crie uma branch e mude para ela:

   ```bash
   git checkout -b custom-instructions
   ```

## Use o Copilot Chat *antes* de atualizar as instruções

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de iniciar os exercícios abaixo, volte ao codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; as etapas indicarão quando for relevante.

Para observar o impacto das instruções personalizadas, envie um prompt com o arquivo de instruções atual. Depois, atualize-o e envie o mesmo prompt novamente para comparar.

1. Feche as guias de editor abertas nos exercícios anteriores para que o Copilot use somente o contexto desejado.
2. Abra `src/lib/publishers.ts` para indicar onde o helper deve ficar.
3. No seletor de agentes da visualização Chat, selecione **Agent** para que o Copilot possa alterar arquivos.

   ![Captura de tela do seletor de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

4. Envie o prompt:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. O Copilot explorará o projeto e aplicará alterações, normalmente no helper e em seus testes.
6. Observe que o helper proposto é uma função tipada que recebe o cliente `db` como primeiro argumento e retorna uma matriz tipada de editoras. Isso segue as convenções da camada de dados em `.github/instructions/drizzle.instructions.md`, aplicáveis a `src/lib/*.ts`.
7. Observe que o código proposto **não inclui** comentários TSDoc nem um cabeçalho de comentário no arquivo.

> [!CAUTION]
> Como o Copilot é probabilístico, talvez ele adicione comentários mesmo sem essa orientação. Tudo bem: o objetivo continua sendo observar o ganho de *consistência* depois da atualização das instruções.

## Adicione um padrão ao repositório

O arquivo `.github/copilot-instructions.md` fornece informações do projeto ao Copilot. Vamos documentar os padrões de código para melhorar as sugestões.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting requirements**, próxima da linha 27. Ela documenta os padrões, mas ainda não exige documentação no código; por isso, o helper não tinha comentários.
3. Logo abaixo dos padrões existentes, adicione estas linhas para solicitar cabeçalhos de arquivo e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu na lição anterior, as instruções globais podem ficar em `.github/copilot-instructions.md`; instruções para linguagens, tipos de arquivo ou tarefas específicas podem ficar em arquivos `*.instructions.md`. O arquivo do repositório é adequado para padrões válidos em todo o projeto, como a regra de comentários de documentação.

## Execute o prompt novamente e observe a alteração

1. No Copilot Chat, selecione **New Chat** para limpar o contexto.
2. Volte a `src/lib/publishers.ts` para indicar o arquivo correto ao Copilot.
3. Envie o **mesmo prompt**:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observe que o arquivo proposto começa com um bloco de comentários semelhante a este:

   ```typescript
   /**
    * Helpers de acesso a dados de editoras para a plataforma de financiamento coletivo Tailspin Toys.
    * Fornece funções para recuperar informações de editoras do banco de dados.
    */
   ```

5. Observe que a função agora inclui um comentário TSDoc semelhante a este:

   ```typescript
   /**
    * Retorna uma lista de todas as editoras com id e nome.
    *
    * @param db - O cliente de banco de dados Drizzle.
    * @returns Uma promessa que resolve para uma matriz de objetos de editora.
    */
   ```

Você orientou o Copilot a seguir um novo padrão do projeto e aplicá-lo ao código real que será usado no próximo exercício.

## Faça commit das instruções e envie a branch

Os arquivos de instruções, como qualquer outro recurso do repositório, são controlados pelo mesmo fluxo de código-fonte.

1. Abra um terminal com <kbd>Control</kbd>+<kbd>`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Confirme se as instruções e o helper foram alterados:

   ```bash
   git status
   ```

3. Adicione e faça commit da atualização das instruções e de todos os arquivos alterados pelo Copilot para criar o helper:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Se git status mostrar outras alterações relacionadas (por exemplo, testes), adicione esses arquivos também.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envie a branch ao repositório:

   ```bash
   git push -u origin custom-instructions
   ```

## Crie e integre um pull request

Com a branch enviada, vamos abrir um pull request associado à issue de padrões de documentação no backlog. O Copilot pode encontrar a issue, criar um PR que a encerra e integrá-lo.

> [!NOTE]
> Este é o primeiro exercício em que o Copilot atua no GitHub por você. O modelo do projeto já configura as ferramentas do GitHub, portanto não é necessário configurar nada. Na primeira vez que o Copilot usar uma ferramenta, o VS Code talvez peça que você entre no GitHub; siga as solicitações para autorizar. Você aprenderá como essa conexão funciona (Model Context Protocol) em outro exercício.

1. Abra o Copilot Chat no codespace.
2. Pressione <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir a visualização Copilot Chat e confirme que **Agent** está selecionado.
3. Peça ao Copilot para encontrar a issue sobre a atualização das instruções e criar um PR da branch atual, descrevendo tanto as instruções quanto o helper de editoras:

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Aguarde o Copilot encontrar a issue e criar o PR.
5. Quando solicitado que o Copilot execute ações no GitHub em seu nome, revise o comando e selecione **Allow** se estiver de acordo.
6. Depois da criação do PR, peça ao Copilot para integrá-lo e retornar à branch `main`:

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Quando solicitado que o Copilot execute ações no GitHub e comandos de shell, revise-os e selecione **Allow** se estiver de acordo.

Você criou e integrou um pull request com a ajuda do GitHub Copilot.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções e usou o Copilot Chat no VS Code para:

- enviar um prompt de geração de código e observar o resultado com as instruções *existentes*.
- adicionar um padrão válido para todo o repositório em `.github/copilot-instructions.md`.
- enviar o mesmo prompt novamente e observar o código adotar o novo padrão.
- fazer commit das instruções e da base do helper em `main` para que o próximo exercício possa usá-los.

Em seguida, você colocará as instruções em prática no [modo de agente][next-lesson] para adicionar um recurso à base de código.

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Boas práticas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para criar instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Instruções pessoais personalizadas para o GitHub Copilot][personal-instructions]
- [Awesome Copilot: coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
