---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados em tarefas ou domínios específicos do fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você fornece instruções direcionadas, boas práticas, padrões de código e conhecimento de domínio para orientar o Copilot. As equipes podem transformar sua experiência em agentes reutilizáveis, como um agente de acessibilidade que aplica as diretrizes da [WCAG][wcag], um agente de segurança que segue práticas de código seguro ou um agente de testes que mantém padrões consistentes.

Agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialização e as instruções do agente.

### Agentes personalizados e skills de agente

Há alguma sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept], pois ambos costumam usar arquivos Markdown para explicar à IA como executar tarefas. A distinção mais simples é: o **agente personalizado** é quem executa o trabalho; as **skills** são ferramentas.

Agentes personalizados têm uma janela de contexto própria e podem orquestrar skills, e até outros agentes. Neste laboratório, o agente personalizado de acessibilidade revisará e atualizará o site de acordo com diretrizes de acessibilidade. Como parte do trabalho, poderia chamar skills de fluxo de pull request ou de execução e gerenciamento de testes.

> [!NOTE]
> Não existe uma única forma "correta" de escrever um agente personalizado. Como em qualquer trabalho com IA, teste e itere para descobrir o que funciona em seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer garantir que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente da capacidade visual ou das preferências de cada uma. Comentários recentes indicaram que o tema escuro atual é difícil de ler devido ao baixo contraste entre as cores do texto e do fundo. A equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é importante, você usará um agente personalizado para implementar a funcionalidade.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar o modo de alto contraste.

## Revise o agente personalizado de acessibilidade

Já existe um agente personalizado de acessibilidade no projeto. Vamos revisar o arquivo para entender como ele orientará o Copilot.

Volte ao codespace e crie uma branch a partir de `main` para o trabalho de acessibilidade, mantendo o PR de filtragem do Exercício 3 separado:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

3. Examine as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para sites acessíveis.
   - boas práticas de acessibilidade.
   - exemplos de código em HTML, CSS e JavaScript.
   - armadilhas e erros comuns.

## Use o agente personalizado no Copilot Chat

O VS Code mostra todos os agentes personalizados definidos em `.github/agents` no seletor de agentes na parte inferior da visualização Copilot Chat. Você pode selecionar um agente para limitar a sessão às instruções e ferramentas dele.

> [!TIP]
> **Abra o Copilot Chat**
>
> Antes de iniciar os exercícios abaixo, volte ao codespace, abra o painel Copilot Chat e selecione **New Chat** para começar uma conversa limpa. O modo e o modelo variam conforme o exercício; as etapas indicarão quando for relevante.

1. Se ainda não estiver selecionado, escolha **Agent** no seletor de agentes da visualização Chat.

   ![Captura de tela do seletor de agente na visualização Chat.](../../../_images/shared-chat-mode-selector.png)

2. Selecione o seletor de agentes na parte inferior da visualização Chat. Ele mostra o agente ativo, que por padrão é **default**.
3. Selecione **Accessibility agent** na lista de agentes.
4. Envie este prompt ao agente de acessibilidade:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. O Copilot Chat explorará a base de código, proporá alterações e editará arquivos do projeto. Cada edição aparecerá na conversa com o caminho do arquivo e um diff para revisão.
6. O indicador **Files changed** será atualizado conforme o agente modificar arquivos.

> [!NOTE]
> O processo pode levar alguns minutos. O Copilot fará alterações reais no repositório, como nos componentes Astro, no CSS e em testes relacionados.

Você revisará e orientará esse trabalho em andamento no próximo exercício.

## Resumo e próximos passos

Este exercício apresentou [agentes personalizados][custom-agents], assistentes de IA especializados em tarefas e domínios. Eles permitem registrar a experiência e os padrões da equipe em agentes reutilizáveis que orientam o Copilot.

Você explorou:

- como os agentes personalizados são definidos.
- como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, você [acompanhará e orientará o trabalho do agente][next-lesson], revisando as alterações à medida que são feitas e adicionando um seletor de modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar seu recurso com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
