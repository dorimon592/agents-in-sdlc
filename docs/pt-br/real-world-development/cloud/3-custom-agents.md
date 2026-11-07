---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados em tarefas ou domínios do fluxo de desenvolvimento. Arquivos Markdown na pasta `.github/agents` fornecem instruções direcionadas, boas práticas, padrões de código e conhecimento de domínio. As equipes podem registrar sua experiência em agentes reutilizáveis, como agentes de acessibilidade que seguem a [WCAG][wcag], de segurança ou de testes.

Os agentes podem ser definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialização e as instruções.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] são definidos principalmente em arquivos Markdown, mas desempenham funções diferentes: o **agente personalizado** é quem executa o trabalho; as **skills** são ferramentas.

Agentes personalizados têm sua própria janela de contexto e podem orquestrar skills, e até outros agentes. Neste laboratório, o agente de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade e poderá usar skills de fluxo de pull request ou de testes.

> [!NOTE]
> Não existe uma única forma "correta" de escrever um agente personalizado. Teste e itere para descobrir o que funciona em seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer tornar sua plataforma de financiamento coletivo acessível a todas as pessoas, independentemente da capacidade visual ou das preferências de cada uma. Comentários de usuários indicaram dificuldade para ler o tema escuro por causa do baixo contraste entre texto e fundo. A equipe de design solicitou um modo de alto contraste com uma opção para ativá-lo e desativá-lo.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- atribuir uma tarefa ao agente de nuvem do Copilot usando o agente de acessibilidade.

## Revise o agente personalizado de acessibilidade

Já existe um agente personalizado de acessibilidade. Volte ao codespace e revise o arquivo:

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

3. Examine as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para sites acessíveis.
   - boas práticas de acessibilidade.
   - exemplos de código em HTML, CSS e JavaScript.
   - armadilhas e erros comuns.

## Crie uma issue e atribua-a

Mission Control é o local central para gerenciar agentes. Você pode atribuir tarefas ao agente de nuvem do Copilot, acompanhar as tarefas e fornecer orientações. Vamos atribuir ao Copilot a tarefa de criar o modo de alto contraste.

1. Acesse seu repositório.
2. Selecione **Issues** e, em seguida, **New issue**.
3. Selecione **Blank issue**.
4. No campo **Title**, digite `Add high contrast mode to website`.
5. Em **Description**, digite:

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

6. Selecione **Create** para criar a issue.
7. No painel à direita, selecione **Assign to Copilot**.
8. Na lista de agentes personalizados, selecione **Accessibility agent**.

   ![Captura de tela da atribuição do agente de nuvem, com o agente personalizado de acessibilidade selecionado](../../../_images/ex5-select-custom-agent.png)

9. Selecione **Assign**. O Copilot começará a trabalhar em segundo plano.

## Resumo e próximos passos

Este exercício apresentou [agentes personalizados][custom-agents] do GitHub Copilot, assistentes especializados em tarefas e domínios. Eles permitem registrar a experiência e os padrões da equipe em agentes reutilizáveis.

Você explorou:

- como os agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot implementa o modo de alto contraste, você pode [acompanhar e orientar a sessão do agente][next-lesson] pelo Mission Control.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar o uso de agentes personalizados na organização][org-custom-agents]
- [Preparar o uso de agentes personalizados na empresa][enterprise-custom-agents]

---

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
