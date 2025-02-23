# Resumo

Neste arquivo apresentamos de forma simplificada como o Tamboerijn lida com movimentação. Esse esquema de 3 passos não ocorre na prática, no código é tudo de uma vez, mas é uma forma de entender como o robô se movimenta.  

## Seleção da Movimentação

A movimentação começa no `motion_selector` encadeado a partir da resposta do [comportamento](../comportamento/comportamento.md), o `motion_command`. Aqui a movimentação atual é escolhida baseada na movimentação anterior, se ela já acabou ou precisa ser abortada.  

## Execução da Movimentação

No próximo passo, todos os nós de todas as motions são executados. Os nós, que as motions não foram selecionadas, são retirados previamente.  

## Envio de Comandos

A movimentação finaliza coletando todos os comandos dos motores na struct `motor_commands_collector`, envia eles para a `motor_commands_optimizer` e depois os escreve na interface de hardware no `command_sender`.
