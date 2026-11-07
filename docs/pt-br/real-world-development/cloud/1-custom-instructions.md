---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica — ou se houver informações de contexto que o Copilot deva conhecer —, você deve disponibilizar essas informações. Os [arquivos de instruções][instruction-files] orientam o Copilot não apenas sobre *o que* fazer, mas também sobre *como* fazer.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de código e os padrões de documentação chegam ao Copilot por meio das instruções do repositório e dos arquivos específicos por caminho.
- adicionar um novo padrão válido para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos percursos de VS Code e CLI, você não executará um prompt antes e depois da alteração. O agente de nuvem trabalha de forma assíncrona em issues do GitHub, então é difícil demonstrar o efeito lado a lado. Você verá a influência das instruções ao revisar os pull requests gerados mais adiante.

## Arquivos de instruções

### Cenário

A Tailspin Toys tem diretrizes de desenvolvimento, incluindo testes unitários para a camada de dados, modo escuro e visual moderno para a interface, documentação com comentários TSDoc e um bloco de comentários no início de cada arquivo que explique sua função. Os arquivos de instruções garantem que o Copilot receba essas orientações.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot. Você pode indicar convenções de código, bibliotecas e tipos de comentários, além de criar instruções para todo o repositório ou para arquivos e tarefas específicos.

- `.github/copilot-instructions.md` é enviado ao Copilot em **todas** as solicitações do repositório e deve conter contexto do projeto, stack, práticas e orientações globais.
- `.github/instructions/*.instructions.md` pode conter instruções para linguagens, tipos de arquivo ou tarefas específicas, como TypeScript, Astro, componentes de interface ou testes unitários.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para autocompletar ou sugerir a próxima edição. O Copilot Chat, o Copilot CLI e o agente de nuvem usam as instruções do repositório e os arquivos `*.instructions.md` com `applyTo`. Também há suporte a outros padrões, como `AGENTS.md` e `CLAUDE.md`; consulte a [referência de suporte a instruções personalizadas][custom-instructions-support].

### Boas práticas para gerenciar arquivos de instruções

- Mantenha `copilot-instructions.md` focado nas informações do projeto e nos padrões globais.
- Use arquivos `*.instructions.md` para tipos de arquivo, como testes unitários, componentes Astro e camada de dados, ou para tarefas específicas.
- Use linguagem natural e orientações claras, com exemplos do que o código deve ou não deve fazer.

Não existe uma única forma de criar instruções ou usar IA. Experimente para descobrir o que funciona no projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de instruções. Este repositório inclui orientações para [atualizações de interface][ui-instructions] e [Astro][astro-instructions]. O Copilot também pode gerar instruções, por exemplo com **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. Para modelos, explore o [awesome-copilot][awesome-copilot].

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explore os arquivos de instruções do projeto

Leia os arquivos incluídos no repositório pela interface web do GitHub ou em um codespace.

1. Abra `.github/copilot-instructions.md`.
2. Observe a descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**, incluindo a orientação de **GitHub Actions Workflows** dentro de **Code standards**.
3. Explore `.github/instructions` e observe os arquivos para Astro, Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md` e observe o frontmatter `applyTo`, que define um glob relativo à raiz do repositório. Por exemplo, ele corresponde a arquivos `**/*.test.ts`.
5. Leia as orientações específicas para criar testes unitários.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções e arquivos do projeto. Esses links permitem decompor instruções maiores em arquivos menores e reutilizáveis e indicar exemplos. Os caminhos são relativos ao próprio arquivo de instruções.

> [!NOTE]
> A seção **Code formatting requirements** descreve os padrões de código do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentário nos arquivos.

## Adicione um padrão ao repositório

Esta será a única edição feita neste exercício: uma regra global para adicionar documentação no código usando comentários TSDoc e um cabeçalho no nível do arquivo. O agente de nuvem usará a regra em issues atribuídas mais adiante.

Crie uma branch para as alterações no terminal do codespace:

```bash
git checkout -b custom-instructions-cloud
```

O agente de nuvem lerá as instruções da branch de destino da issue. Mesmo assim, você fará commit das alterações para manter o histórico organizado.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize **Code formatting requirements**, perto da linha 27. Observe que a seção ainda não inclui documentação no código, e é por isso que o helper gerado não tinha comentários.
3. Logo abaixo dos padrões existentes, adicione estas linhas:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Salve `copilot-instructions.md`.

> [!TIP]
> As instruções globais ficam em `.github/copilot-instructions.md`; as instruções para linguagens, tipos de arquivo ou tarefas específicas ficam em arquivos `*.instructions.md`. O arquivo do repositório é adequado para padrões globais, como a regra de comentários de documentação.

## Faça commit, envie e integre a atualização

O agente de nuvem lê as instruções da branch de destino. Na próxima lição, ele criará branches a partir de `main`; portanto, as instruções precisam estar em `main` para serem aplicadas.

1. Prepare e faça commit da atualização:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request de `custom-instructions-cloud` para `main` e faça o merge.

> [!TIP]
> Se preferir trabalhar diretamente em `main` neste workshop, você pode ignorar as etapas da branch e fazer commit direto em `main`. A branch demonstra como o trabalho é feito em um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções e adicionou um padrão ao arquivo `.github/copilot-instructions.md`. Você verá o padrão aplicado aos pull requests criados pelo agente de nuvem durante o restante do percurso.

Em seguida, você [atribuirá sua primeira issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Boas práticas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para criar instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
