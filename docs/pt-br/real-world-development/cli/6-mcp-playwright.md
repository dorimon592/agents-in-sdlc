---
title: "Lição 6 - Validar a funcionalidade com o MCP do Playwright"
description: "Inspecione ou configure o MCP do Playwright no Copilot CLI e use-o para explorar o recurso de filtragem em um navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como já destacamos, escrever código envolve mais do que apenas programar. É preciso trabalhar com dados, serviços externos e automações adicionais disponíveis ao Copilot. É aí que entram os servidores MCP: eles permitem que o Copilot vá além dos recursos incluídos na CLI e acesse outras ferramentas e serviços.

Nesta lição, você vai:

- entender o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza.
- adicionar o servidor MCP do Playwright se ele ainda não estiver disponível.
- pedir ao agente que controle um navegador e explore o recurso de filtragem.

## Cenário

Testes unitários e de ponta a ponta são importantes, mas validar alterações na interface exige interagir com ela. Você quer permitir que o Copilot use o site como uma pessoa usuária faria, automatizando a validação e aumentando a confiança de que as alterações funcionam como esperado.

## O que é o Model Context Protocol (MCP)?

O [Model Context Protocol (MCP)][mcp-blog-post] permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real. Assim, eles podem acessar informações atualizadas e executar ações em seu nome.

As ferramentas e os recursos são acessados por um servidor MCP, que atua como ponte entre o agente de IA e as ferramentas e serviços externos. Cada servidor MCP oferece um conjunto diferente de ferramentas e recursos.

Alguns servidores MCP populares:

- **[Servidor MCP do GitHub][github-mcp]**: fornece acesso às APIs para gerenciar repositórios, issues e pull requests do GitHub.
- **[Servidor MCP do Playwright][playwright-mcp-server]**: fornece recursos de automação de navegador usando o Playwright.

Há muitos outros servidores MCP. O GitHub mantém um [registro de MCP][mcp-registry] para facilitar a descoberta de servidores e as contribuições ao ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise o código-fonte, verifique o publicador e considere as implicações de segurança.

## Adicione o servidor MCP do Playwright

Se o servidor MCP do Playwright ainda não estiver disponível, adicione-o para que o Copilot possa interagir com o site como uma pessoa usuária faria.

> [!NOTE]
> Se o MCP do Playwright já estiver configurado no Copilot CLI, ignore estas etapas e continue na próxima seção.

1. Volte ao Codespace.
2. No Copilot CLI, abra a caixa de diálogo para adicionar um servidor MCP com o comando:

   ```plaintext
   /mcp add
   ```

3. No campo de nome, digite `playwright` e pressione <kbd>Tab</kbd>.
4. Confirme o tipo de servidor **STDIO** pressionando <kbd>Enter</kbd> e, depois, pressione <kbd>Tab</kbd>.
5. Cole o seguinte na caixa de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pressione <kbd>Control</kbd>+<kbd>S</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para salvar o novo servidor MCP.
7. Pressione <kbd>Esc</kbd> para sair da caixa de diálogo de MCP.

## Peça ao Copilot que explore o recurso com o Playwright

Anteriormente, você confirmou manualmente que a funcionalidade funcionava. Agora, vamos usar o servidor MCP do Playwright disponível no Copilot para fazer o mesmo.

1. Use este prompt para pedir ao Copilot que valide a funcionalidade com o servidor MCP do Playwright:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Não é necessário instruir o Copilot a usar o servidor MCP: ele normalmente o identifica por conta própria. Mesmo assim, indicar a ferramenta pode ajudar a obter resultados mais consistentes e economizar tokens.

2. Observe as etapas executadas pelo Copilot no navegador para confirmar se a funcionalidade funciona.
3. Leia o relatório e confirme que tudo está funcionando como esperado.

O Copilot iniciará o servidor, usará o Playwright para interagir com o site, parará o servidor e apresentará um relatório.

## Resumo e próximos passos

Parabéns, você usou o servidor MCP do Playwright para explorar seu recurso em um navegador real por meio do Copilot CLI. Nesta lição, você:

- aprendeu o que é o Model Context Protocol (MCP) e como o Copilot CLI o utiliza.
- adicionou o servidor MCP do Playwright, se ele ainda não estivesse disponível.
- pediu ao agente que controlasse um navegador e explorasse o recurso de filtragem.

Em seguida, você [criará um agente personalizado de QA][next-lesson] que combina a skill e as ferramentas do navegador.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Adicionar servidores MCP ao Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
