---
slug: pt-br/real-world-development/cli
title: "CLI do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O **[GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)** leva o GitHub Copilot ao terminal como assistente de programação baseado em agentes. Ele explora bases de código, gera código, executa comandos e se conecta a ferramentas externas pela linha de comando.

Nestas lições, você instalará e autenticará o Copilot CLI e fornecerá contexto sobre o projeto usando instruções personalizadas. Depois, usará o modo Plan para planejar um recurso, implementá-lo com o Autopilot e verificar se funciona. Você conectará o servidor MCP do Playwright para testar o recurso em um navegador, usará a skill `quality-checks` e criará um agente de QA. Por fim, revisará e integrará o pull request com o Agent Merge e explorará comandos de barra para contexto, modelos, compartilhamento e delegação opcional para o agente de nuvem. Você também pode seguir a [série opcional de três módulos sobre GitHub Copilot CLI e Foundry][foundry] para preparar um modelo, criar e implantar um agente hospedado e integrá-lo ao site.

## Lições

| Lição | Tópico | Descrição |
|----------|-------|-------------|
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e seu codespace |
| [1. Instalar o Copilot CLI][ex1] | Instalação | Instale e autentique o Copilot CLI |
| [2. Adicionar avaliações por estrelas: uma melhoria rápida][ex2] | Primeira alteração | Exiba as avaliações existentes e a alternativa para `null`, revise a alteração e integre o primeiro pull request |
| [3. Modos de agente: Plan e Autopilot][ex3] | Modos de agente | Planeje o recurso a partir de uma issue, desenvolva com o Autopilot e revise no modo Interactive |
| [4. Orientar o Copilot com instruções personalizadas][ex4] | Contexto | Explore e atualize as instruções e aplique-as à filtragem |
| [5. Personalizar e usar uma skill de verificações de qualidade][ex5] | Verificações repetíveis | Explore a skill existente, altere o formato do relatório e execute-a |
| [6. Validar a funcionalidade com o MCP do Playwright][ex6] | Observação no navegador | Configure o MCP e use-o para examinar a filtragem |
| [7. Criar e usar um agente de QA][ex7] | Requisitos e cobertura | Crie e selecione um perfil especializado e reúna evidências da verificação final |
| [8. Criar e integrar o pull request do recurso][ex8] | Revisão e merge | Revise as alterações e use o Agent Merge no segundo pull request |
| [9. Explorar comandos de barra][ex9] | Recursos da CLI | Explore o contexto, os modelos, o compartilhamento e a delegação opcional para o agente de nuvem |
| [10. Revisão e próximos passos][ex10] | Resumo | Revise o fluxo de trabalho e continue aprendendo |
| [Incorporar o Foundry (opcional)][foundry] | Agentes hospedados | Prepare um modelo, crie e implante um Backer Concierge baseado no catálogo e conecte-o ao site |

## Pré-requisitos

Antes de participar deste workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano ativo **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] Familiaridade básica com operações de terminal e linha de comando
- [ ] O Git instalado e configurado

> [!TIP]
> Não tem um plano pago? Estudantes verificados podem obter o GitHub Copilot gratuitamente por meio do [GitHub Education][callout-student-plan-education]. O plano **Copilot Student** inclui os recursos de agente, MCP, revisão de código e Copilot CLI usados neste workshop, para que você possa concluir todos os percursos com ele.

> [!NOTE]
> Se você usa o Copilot Business ou Enterprise, confirme com o administrador se a política do Copilot CLI está habilitada.

## Começar

[**Comece pela Lição 0: Pré-requisitos →**][ex0]

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
[callout-student-plan-education]: https://github.com/education/students
