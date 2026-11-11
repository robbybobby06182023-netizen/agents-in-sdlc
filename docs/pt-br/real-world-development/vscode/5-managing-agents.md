---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando você usa o GitHub Copilot no modo de agente, ele trabalha de forma autônoma — explora a base de código, propõe mudanças, edita arquivos e executa comandos. Como esse trabalho acontece na sua máquina em tempo real, o Copilot Chat no VS Code mostra todas as chamadas de ferramentas e edições de arquivos à medida que ocorrem. Você pode revisar cada diferença em linha, aceitar ou rejeitar mudanças individuais e direcionar a conversa com prompts complementares para aprimorar ou ampliar o trabalho sem sair do editor.

Neste exercício, você vai:

- monitorar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições de arquivos propostas.
- direcionar o agente durante a sessão para adicionar um modo claro junto com o modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design apresentou outra solicitação: também quer uma opção de modo claro para usuários que consideram o tema padrão escuro demais. Em vez de iniciar uma conversa nova, você direcionará a sessão existente para ampliar o trabalho.

## Monitore o progresso do Copilot

A exibição do Copilot Chat mostra cada etapa executada pelo agente — os arquivos explorados, os comandos executados e as edições propostas. Cada edição aparece em linha no chat, com as diferenças e os controles **Keep**/**Undo**.

1. Retorne à exibição do Copilot Chat usada no Exercício 4.
2. Enquanto o agente trabalha, role pelo chat para acompanhar cada chamada de ferramenta. Você verá leituras e edições de arquivos e os comandos de terminal cuja execução foi solicitada.
3. Quando o agente propuser uma edição de arquivo, as diferenças aparecerão em linha:
   - Selecione **Keep** para aceitar a mudança.
   - Selecione **Undo** para reverter a mudança e permitir outra tentativa do agente.
4. Você também pode abrir o indicador **Files changed** na parte superior do chat para ver todos os arquivos modificados pelo agente durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta pedir permissão (por exemplo, para executar um comando de terminal), o agente fará uma pausa e exibirá uma solicitação **Allow**. Aprove apenas os comandos que considerar seguros.

5. Aguarde o agente concluir a implementação do modo de alto contraste. Você deverá ver uma mensagem final que resume as mudanças.

## Oriente o agente para adicionar o modo claro

Agora que o modo de alto contraste está implementado, você ampliará a mesma conversa para adicionar uma opção de modo claro. Como continuará na mesma sessão de chat, o agente manterá todo o contexto do trabalho anterior — incluindo os componentes que alterou, a abordagem de estilo usada e o padrão de armazenamento local configurado.

1. No campo de entrada do Copilot Chat, envie o seguinte prompt complementar:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. O agente continuará o trabalho, explorará as mudanças já feitas e proporá adições para o modo claro.
3. Revise cada nova edição à medida que for aplicada, como fez com o modo de alto contraste. Use **Keep** ou **Undo** em cada diferença.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar a interface das opções de alternância de uma maneira de que você não goste —, pare-o com o botão **Stop** ao lado do campo de entrada do chat e envie uma mensagem complementar esclarecendo o pedido.

> [!TIP]
> Direcionar uma sessão em andamento é mais rápido do que começar de novo. Use prompts complementares para solicitar ajustes ("simplifique o CSS — use variáveis CSS em vez de duplicar seletores"), corrigir o rumo ("não adicione uma terceira opção de alternância; integre o modo claro ao painel de configurações existente") ou ampliar o escopo ("adicione também atalhos de teclado para as duas opções").

## Revise o conjunto de mudanças

Antes de fazer commit do trabalho, revise rapidamente tudo o que o agente alterou.

1. Abra a exibição **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações nos componentes Astro, nos estilos e em testes relacionados, se houver.
3. Abra alguns arquivos alterados e examine as diferenças. Confirme que os padrões de acessibilidade do agente personalizado foram aplicados — atributos ARIA, navegação pelo teclado, HTML semântico e persistência com armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das mudanças pelo painel Source Control. Você as publicará [na próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para monitorar e direcionar uma sessão em andamento — revisando cada edição de arquivo, aceitando ou rejeitando mudanças e enviando prompts complementares para ampliar o trabalho sem perder o contexto.

Nesta lição, você explorou:

- como monitorar o trabalho do agente em tempo real.
- como aceitar e rejeitar edições individuais de arquivos.
- como direcionar uma sessão em andamento com prompts complementares.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade como um pull request e revisará as mudanças produzidas pelo agente personalizado local durante o laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar no trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
