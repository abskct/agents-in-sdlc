---
title: "Lição 2 - Adicionar avaliações por estrelas: uma melhoria rápida"
description: "Inicie sua primeira sessão de agente no aplicativo GitHub Copilot, faça uma pequena alteração nos cartões dos jogos e integre-a como seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Agora que você conheceu o espaço de trabalho e usou um chat rápido, está na hora de iniciar uma **sessão de agente** e fazer sua primeira alteração no projeto. A mudança será pequena: os dados dos jogos já têm uma avaliação por estrelas, mas os cartões da página inicial ainda não a exibem. Você pedirá ao agente que mostre a avaliação, revisará a alteração e fará o merge no primeiro pull request.

Nesta lição, você vai:

- iniciar uma sessão de agente e aprender como ela é estruturada.
- pedir ao agente que faça uma alteração pequena e focada no projeto.
- revisar a alteração na visualização de diff do espaço de trabalho.
- executar o aplicativo localmente para confirmar a alteração no navegador.
- abrir e fazer o merge do primeiro pull request.

## Cenário

Cada jogo na Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes do jogo. Porém, os cartões de jogos na página inicial exibem somente o título, a categoria, a editora e a descrição. Como aquecimento, peça ao agente para mostrar a avaliação existente em cada cartão: uma alteração pequena e independente, ideal para sua primeira sessão.

## Anatomia de uma sessão

Uma **sessão** é uma conversa com um agente executada em seu próprio espaço de trabalho isolado. Cada sessão recebe um **git worktree e uma branch dedicados**, o que permite executar várias sessões ao mesmo tempo — uma para adicionar um recurso e outra para corrigir um bug — sem que as alterações entrem em conflito. Suas sessões aparecem na barra lateral, agrupadas por repositório. Selecione uma delas para acessá-la.

Em uma sessão, você verá três elementos: a **conversa** com o agente, a **atividade das ferramentas** usadas pelo agente para explorar e editar arquivos e a lista de **arquivos alterados** com os respectivos diffs.

## Inicie uma sessão e solicite a alteração

Vamos iniciar uma nova sessão para explorar o projeto e implementar o recurso. Durante a [configuração do aplicativo][prior-lesson], você adicionou o projeto pelo repositório do GitHub. Vamos criar uma nova sessão para esse repositório e solicitar a alteração.

1. Volte ao aplicativo GitHub Copilot ou abra-o.
2. Na parte superior da barra lateral, selecione **+** para iniciar uma sessão.
3. Selecione `tailspin-toys` como repositório.
4. Escolha **new working tree** e o modo **Interactive** abaixo da caixa de prompt. Use este prompt para solicitar a alteração:

    ```plaintext
    Exiba a avaliação starRating de cada jogo em uma escala de cinco estrelas nos cartões da lista de jogos. Se a avaliação for null, mostre "No rating yet". Mantenha o layout atual dos cartões, adicione testes e execute as verificações relevantes.
    ```

5. Pressione <kbd>Enter</kbd> para enviar o prompt ao Copilot.

O aplicativo Copilot começa criando um novo worktree, uma cópia isolada do projeto. Em seguida, explora o projeto, localiza os arquivos que precisam ser atualizados para adicionar o recurso e cria o código necessário. Você acabou de adicionar um recurso com o aplicativo Copilot.

## Revise o diff

Todas as alterações geradas por IA devem ser revisadas antes do merge, mesmo as pequenas. Vamos explorar as alterações diretamente no aplicativo Copilot.

1. No canto superior direito do aplicativo, selecione **Toggle review panel**. No painel que abrir, selecione **Changes** para ver os arquivos alterados e seus diffs.

    ![Barra de ferramentas superior do aplicativo GitHub Copilot com uma seta apontando para o botão Toggle review panel à direita de Create PR](../../../_images/app-2-review-panel.png)

2. Você verá código adicionado a `GameCard.astro`, o arquivo principal usado para exibir os detalhes do jogo. Ele deverá ser semelhante ao exemplo a seguir: um pequeno bloco que renderiza a avaliação quando ela está disponível e usa "No rating yet" quando `starRating` é `null`:

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Como o Copilot, assim como todas as ferramentas de IA generativa, é probabilístico e não determinístico, o código exato pode variar. No entanto, ele deve ser relativamente semelhante.

## Verifique as alterações

Revise os resultados das verificações automatizadas do agente antes de abrir um navegador. Confirme se os testes cobrem um valor numérico de `starRating` e a alternativa para `null`. Um pré-requisito ausente ou uma verificação ignorada não conta como aprovação; revise qualquer solicitação de instalação antes de aprová-la.

Não basta ler o código e presumir que funciona. Vamos pedir ao Copilot para abrir o site e examinar a interface atualizada. Para isso, podemos pedir que inicie o site e o abra em um canvas de navegador.

> [!TIP]
> Um canvas é um widget interativo disponível dentro do próprio aplicativo Copilot. Você explorará canvases personalizados e até criará um mais adiante, mas, por enquanto, usaremos o canvas de navegador integrado.

1. Use o prompt a seguir para pedir ao Copilot que inicie o aplicativo e abra a página no canvas de navegador:

    ```plaintext
    Inicie o aplicativo e abra-o no canvas de navegador.
    ```

2. Em alguns instantes, o aplicativo será iniciado e uma janela do navegador será aberta dentro do aplicativo Copilot.
3. Confirme se os cartões dos jogos avaliados exibem o valor em uma escala de cinco estrelas.
4. Quando terminar, peça ao Copilot para parar o servidor de desenvolvimento iniciado para esta sessão:

    ```plaintext
    Pare o servidor de desenvolvimento e feche o canvas de navegador.
    ```

## Abra e faça o merge do primeiro pull request

Você criou o recurso. Agora é hora de criar um pull request (PR) para mesclar o novo código à base de código existente.

1. Selecione **Create PR** no canto superior direito.
2. Se solicitado, selecione **Sign in with your browser** e siga as instruções para se autenticar.
3. Se o Copilot explicar que a política do repositório exige uma issue correspondente antes da criação do PR, confirme que ele deve seguir esse processo.
4. O Copilot começará a criar o PR.
5. Selecione o indicador **PR** logo acima do chat para abrir o PR no painel de revisão. Você pode revisá-lo nesse painel.
6. Quando estiver tudo pronto, selecione **Ready to merge**.
7. Na nova janela de diálogo, selecione **Merge pull request** para fazer o merge do PR.

## Resumo e próximos passos

Parabéns! Você entregou sua primeira alteração com o aplicativo GitHub Copilot. Você:

- iniciou uma sessão de agente e aprendeu como as sessões são estruturadas.
- orientou o agente a fazer uma alteração pequena e focada nos cartões dos jogos.
- revisou a alteração na visualização de diff do espaço de trabalho.
- executou o aplicativo localmente para confirmar a avaliação por estrelas no navegador.
- abriu o PR 1, revisou as verificações e fez o merge explicitamente.

Em seguida, você [começará pela issue de filtragem e usará os modos Plan e Autopilot][next-lesson] para criar um recurso maior.

## Recursos

- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#instale-e-configure-o-aplicativo-github-copilot
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
