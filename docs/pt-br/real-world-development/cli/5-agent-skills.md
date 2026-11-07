---
title: "Lição 5 - Personalizar e usar uma skill de verificações de qualidade"
description: "Explore a skill quality-checks existente, personalize o formato do relatório e use-a para validar a filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escrever código envolve mais do que apenas programar. Você já validou manualmente que o código funciona e usou arquivos de instruções para garantir que ele siga os padrões. E os testes, o lint e as outras partes da integração contínua (CI)?

Para esse tipo de tarefa, **skills de agente** são ideais. Elas ajudam o Copilot a entender como executar operações repetíveis.

Nesta lição, você vai:

- explorar a skill `quality-checks` existente.
- personalizar o formato dos resultados.
- recarregar e executar a skill.

## Cenário

A Tailspin Toys usa a skill `quality-checks` para testes unitários, lint e verificações de tipos. A equipe quer aprimorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

Skills de agente reúnem instruções reutilizáveis, scripts executáveis e recursos de apoio que um agente carrega quando necessário. Em geral, uma skill é uma pasta com um arquivo Markdown chamado `SKILL.md`. Esse arquivo tem frontmatter com nome e descrição, uma visão geral da função da skill e orientações sobre quando usá-la. A pasta também pode conter subpastas com scripts e outros recursos.

> [!NOTE]
> Arquivos e pastas adicionais não são obrigatórios. A skill `quality-checks` da Tailspin Toys contém apenas `SKILL.md` porque usa os comandos existentes do projeto.

As skills podem ficar na pasta `.github/skills` do projeto para serem compartilhadas e reutilizadas pela equipe ou na pasta pessoal `~/.copilot/skills`.

## Explore a skill

Vamos explorar a skill `quality-checks` da equipe Tailspin Toys, que executa testes unitários, lint e verificações de tipos.

1. Volte ao Codespace e abra `.github/skills/quality-checks/SKILL.md` no editor do Codespaces.
2. Leia `name` e `description` no início do arquivo. A descrição ajuda o Copilot a decidir quando usar a skill.
3. Leia as instruções e observe como orientam o Copilot na execução dos testes e do lint.
4. Observe que a skill ainda não inclui uma seção **Results output formatting**.

## Execute a skill antes da alteração

Você pode chamar skills diretamente pelo Copilot CLI ou usar linguagem natural. Vamos pedir ao Copilot que execute as três verificações da skill.

1. Volte à conversa de filtragem no modo Interactive.
2. Envie este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório apresentado no fim.

## Personalize o relatório

Vamos aprimorar o relatório para que indique o que foi executado, se houve sucesso e o que as ferramentas informaram.

1. Volte a `.github/skills/quality-checks/SKILL.md`.
2. Adicione a seção abaixo ao final do arquivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. O arquivo será salvo automaticamente.

## Execute a skill atualizada

O Copilot CLI pode recarregar skills editadas sem reiniciar a conversa.

1. Digite:

   ```plaintext
   /skills reload
   ```

2. Envie exatamente o mesmo prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório e compare-o com o primeiro.

## Resumo e próximos passos

Você personalizou e usou uma skill existente. Nesta lição, você:

- explorou a skill `quality-checks` para testes unitários, lint e verificações de tipos.
- personalizou o formato dos resultados.
- recarregou e executou a skill.

Essa alteração acompanhará a filtragem no PR do recurso. Em seguida, você permitirá que o Copilot interaja diretamente com o site [usando o MCP do Playwright][next-lesson].

## Mais exemplos de skills

Estes exemplos da comunidade são referências, não tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e script de exportação incluído: `drawio`][drawio-example]
- [Teste de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
