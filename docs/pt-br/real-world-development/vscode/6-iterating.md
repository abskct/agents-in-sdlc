---
title: "Exercício 6 - Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← Lição anterior: Monitorar e gerenciar agentes][previous-lesson] |
| :-- |

## Revise o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas voltadas a melhorar a experiência do usuário. Usou o modo de agente para adicionar filtragem no cliente e no servidor, o servidor MCP do Playwright para testar manualmente esse trabalho no navegador e, em seguida, um agente personalizado para implementar recursos de acessibilidade — controles dos modos de alto contraste e claro — e orientar a sessão em andamento para ampliar o trabalho. Agora, é hora de publicar esse trabalho local e revisá-lo da mesma forma que sua equipe faria.

### Cenário

Os fundamentos do design de software e do DevOps não mudam com a inclusão de IA generativa. Você ainda precisa de um ciclo de revisão real para tudo o que o Copilot produz. Com isso em mente, vamos enviar as alterações de acessibilidade do Codespace, abrir um pull request e examinar o diff antes de envolver o restante da equipe.

## Publique os recursos de acessibilidade

Os controles dos modos de alto contraste e claro implementados pelo agente personalizado de acessibilidade nos [Exercícios 4][exercise-4] e [5][exercise-5] estão no Codespace como alterações registradas em commits. Vamos enviá-las para uma branch e abrir um pull request para que o restante da equipe possa revisar.

1. Volte ao Codespace.
2. Abra a exibição **Source Control** no VS Code.
3. Confirme se as alterações de acessibilidade foram registradas em commits. Se houver alterações do Exercício 5 que ainda não foram registradas, prepare-as e faça commit agora com uma mensagem descritiva, como `Add high-contrast and light-mode toggles`.
4. Publique a branch selecionando **Publish Branch** (ou use o menu **...** → **Push**).
5. O VS Code oferecerá a opção de abrir a nova branch no github.com. Aceite a solicitação ou acesse o repositório manualmente e selecione **Compare & pull request** no banner da branch.
6. Defina um título claro (por exemplo, `Add high-contrast and light-mode toggles`) e uma descrição curta que resuma o que foi feito e por quê.
7. Selecione **Create pull request**.
8. Quando o PR estiver aberto, selecione a guia **Files changed** para revisar o trabalho de ponta a ponta. Preste atenção especial a:
   - componentes da interface para alternar entre os modos.
   - uso do armazenamento local para manter as preferências do usuário.
   - alterações de CSS ou de estilo para os modos de alto contraste e claro.
   - atributos de acessibilidade (rótulos ARIA, navegação pelo teclado etc.).
   - código JavaScript/TypeScript que gerencia a alternância entre os modos.

9. Volte à guia **Conversation**.
10. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

11. Aguarde a conclusão dos workflows. Se tudo correr bem, eles deverão ser aprovados.

> [!TIP]
> Quer uma segunda opinião sobre o trabalho de acessibilidade? Mencione `@copilot` em um comentário no PR com uma solicitação, como "revise este PR em busca de outros problemas de WCAG" ou "sugira melhorias para a navegação pelo teclado". O Copilot iniciará uma nova sessão para responder ao comentário.

## Exercício opcional — continue explorando localmente

Trabalhar de forma iterativa com um agente no IDE é uma habilidade que só se desenvolve com a prática. Algumas ideias para sessões complementares no VS Code:

- Adicionar um formulário para manifestar interesse em apoiar o jogo na página de detalhes.
- Implementar paginação na lista de jogos.
- Adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados em `src/lib/`.
- Ampliar o escopo do agente de acessibilidade — por exemplo, auditar a ordem de foco do teclado em todo o site.

## Resumo

Parabéns — você concluiu o percurso do VS Code. Ao longo deste laboratório, você:

- **Usou o Playwright MCP para testar manualmente o recurso.** Você adicionou o servidor MCP do Playwright e deixou o Copilot controlar um navegador para verificar a filtragem antes de abrir um pull request.
- **Orientou o modo de agente a fazer alterações coordenadas em toda a pilha.** Você adicionou um recurso de filtragem que envolveu o cliente, o servidor e os testes em uma única sessão.
- **Usou um agente personalizado.** Você selecionou o agente personalizado de acessibilidade no seletor de agentes e acompanhou a implementação do modo de alto contraste no repositório.
- **Gerenciou e orientou uma sessão de agente.** Você revisou as alterações propostas diretamente na conversa, aceitou o que queria e ampliou a sessão com um pedido complementar para adicionar o modo claro.
- **Concluiu o ciclo com um pull request.** Você publicou o trabalho local e o revisou de ponta a ponta da mesma forma que sua equipe faria.

## Revisão e próximos passos

Isso encerra o percurso obrigatório do VS Code. Você pode parar aqui com o workshop concluído.

Se quiser ampliar sua perspectiva sobre os recursos de agente do Copilot, os outros percursos abordam cenários relacionados em diferentes interfaces:

- 💻 **[Percurso do CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills, um agente de QA personalizado, Playwright MCP, Agent Merge e transições de contexto.
- ☁️ **[Percurso do agente de nuvem](../../cloud/)** — concentre-se em atribuir issues ao agente de nuvem, monitorar sessões pela página de agentes e iterar de forma assíncrona em pull requests.

Você também pode continuar desenvolvendo o trabalho iniciado. O [awesome-copilot][awesome-copilot] é uma ótima fonte de arquivos de instruções, agentes personalizados e skills que você pode adaptar aos seus projetos.

Como extensão opcional, [Opcional: Incorporar o Foundry][exercise-7] usa o VS Code e o Microsoft Foundry Toolkit para preparar um modelo, implantar um Backer Concierge e conectá-lo ao site.

## Recursos

- [GitHub Copilot][github-copilot]
- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
