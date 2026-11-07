---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revise o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em tarefas para melhorar a experiência e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse um recurso de jogos relacionados para a equipe de design avaliar e implementasse recursos de acessibilidade, incluindo seletores de modo de alto contraste e modo claro. Vamos revisar as alterações e, se necessário, orientar o Copilot para aprimorá-las antes de permitir que o restante da equipe revise os PRs.

### Cenário

Os fundamentos do design de software e do DevOps não mudam com a IA generativa. Sempre devemos revisar o código gerado e seguir o processo normal de DevOps. Vamos revisar as sugestões do GitHub Copilot para a documentação, os jogos relacionados e os recursos de acessibilidade.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem trabalha de forma assíncrona e sem supervisão, há restrições para manter o ambiente seguro:

- O Copilot tem acesso de leitura ao repositório e acesso de escrita **somente** à branch que usará para o trabalho.
- O agente de nuvem é executado no GitHub Actions em um ambiente separado e efêmero.
- Um ser humano precisa aprovar todos os workflows do GitHub Actions antes da execução.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo servidores MCP.

## Revise a documentação gerada

Vamos começar pelo primeiro pull request (PR) criado pelo agente de nuvem do Copilot: a documentação do código. Use a interface padrão de PR do GitHub.com.

> [!NOTE]
> Talvez o PR mostre um aviso de que o Copilot foi bloqueado por um firewall. Isso é **esperado**, pois o acesso do Copilot a recursos externos, incluindo servidores MCP, é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. Abra seu repositório no github.com.
2. Selecione **Pull Requests** para ver a lista de PRs.
3. Abra o PR com um título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Aguarde a conclusão, que pode levar alguns minutos.

4. Quando o PR estiver pronto, selecione a guia **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, que inclui comentários TSDoc e outra documentação. As alterações exatas variam. Procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário, que vêm dos arquivos de instruções personalizados vistos no início do Exercício 2.
6. Quando tiver revisado as alterações, volte à guia **Conversation** e role para baixo.
7. Procure um indicador de que os workflows aguardam aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows serão adicionados à seção de verificações do PR. Se tudo der certo, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.

## Peça alterações ao GitHub Copilot

Você também pode mencionar o Copilot em comentários do PR ou comentários em linha no código. Ele verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados são não determinísticos, não há um texto específico a recomendar. Você pode pedir, por exemplo, para:

- adicionar um cabeçalho de comentário no início de cada arquivo com uma breve descrição.
- adicionar comentários TSDoc a arquivos TypeScript e Astro.
- criar um README com uma descrição da estrutura do aplicativo Astro.

1. Publique um comentário pedindo uma alteração na documentação gerada e mencione **@copilot**. Use uma das ideias acima ou outra sugestão.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao PR.

   ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, um novo commit aparecerá no PR.
5. Selecione **Files changed** e revise as alterações.

Continue iterando até ficar satisfeito. Depois, você poderá converter o PR de rascunho em pronto para revisão e integrá-lo à branch principal.

![Converter o PR em pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revise o recurso de jogos relacionados

Agora, vamos revisar o PR criado pelo Copilot para mostrar jogos relacionados na página de detalhes de cada jogo.

1. Volte ao repositório no GitHub.com e selecione a guia **Pull Requests**.
2. Abra o PR com um título semelhante a **Show related games on the game details page**.
3. Selecione **Files changed** e revise o código gerado.
4. Quando tiver revisado as alterações, volte à guia **Conversation** e role para baixo.
5. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

6. Aguarde as verificações do projeto para o aplicativo Astro. Elas podem levar alguns minutos.
7. **Opcional:** faça um teste manual no codespace. Abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot (por exemplo, `copilot/fix-8`):

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot criou o recurso de jogos relacionados. Você pode continuar iterando por meio de comentários na guia **Conversation**. Por exemplo, peça ao Copilot para alterar a quantidade de jogos relacionados ou adicionar cabeçalhos de comentário e comentários TSDoc. Observe que esse trabalho foi atribuído **antes** da atualização das instruções. Assim como no PR anterior, o Copilot verá o comentário e iniciará uma nova sessão.

## Revise os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados com o agente personalizado de acessibilidade. O PR deve incluir o modo de alto contraste atribuído no Exercício 3 e o modo claro solicitado no Mission Control no Exercício 4.

1. Volte ao repositório no GitHub.com e selecione a guia **Pull Requests**.
2. Abra o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Aguarde a conclusão, que pode levar alguns minutos.

3. Selecione **Files changed** e revise o código gerado.
4. Examine a implementação, principalmente:
   - os seletores da interface para alternar entre os modos.
   - o armazenamento local usado para guardar as preferências.
   - as alterações de CSS ou de estilo para os modos de alto contraste e claro.
   - os atributos de acessibilidade (rótulos ARIA, navegação por teclado etc.).
   - o JavaScript ou TypeScript responsável pela alternância de modos.
5. Quando tiver revisado as alterações, volte à guia **Conversation** e role para baixo.
6. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

7. Aguarde as verificações do projeto para o aplicativo Astro. Elas podem levar alguns minutos.
8. **Opcional:** teste manualmente os recursos de acessibilidade no codespace. Abra o terminal e execute os comandos abaixo, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   Inicie o aplicativo e teste os seletores de alto contraste e modo claro no navegador. Confirme se funcionam e se as preferências persistem após recarregar a página.

O agente personalizado de acessibilidade ajudou a orientar a implementação segundo boas práticas. Se encontrar problemas ou melhorias, mencione **@copilot** em um comentário para solicitar alterações, como nos outros PRs.

## Exercício opcional: continue delegando

O agente de nuvem é mais útil quando recebe itens reais do backlog para que você possa se dedicar a outras tarefas. Para praticar, crie mais algumas issues no repositório e atribua-as ao Copilot. Por exemplo:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- implementar paginação na lista de jogos.
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Nestas lições, você:

- **inspecionou os arquivos de instruções personalizados** incluídos no repositório e observou como influenciam o trabalho do agente.
- **atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **criou e usou um agente personalizado** de acessibilidade para adicionar modos de alto contraste e claro.
- **usou a página de agentes do Copilot como Mission Control** para acompanhar e orientar a sessão de acessibilidade.
- **revisou e ajustou os pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Se quiser continuar aprendendo, os outros percursos complementam o que praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP no IDE.
- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas no terminal usando Plan e Autopilot, skills de agente, um agente de QA personalizado, MCP do Playwright, Agent Merge e a integração opcional `/delegate` com o agente de nuvem.

No seu repositório, experimente estas ideias:

- atribuir ao agente de nuvem uma issue de refatoração ou de cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre os agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
