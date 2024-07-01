# SPL Network

Este documento visa descrever o funcionamento da rede do SPL, da comunicação com Game Controller e futuramente entre robôs.

## Game Controller

Atualmente, toda a comunicação do SPL ocorre por IPv4, mas a maior parte também é compatível com IPv6. 

Todo tutorial de instalação e utilização do GameController está no [repositório](https://github.com/RoboCup-SPL/GameController3), este documento explica o funcionamento e especificações da comunicação.

A função deste programa é agir como árbitro da partida. Registrando eventos, gols, penalidades, etc. Ele também é responsável por enviar mensagens de controle para os robôs, como o início e fim de jogo, mudanças de estados (mais detalhes em [Estados do Robô](./estados_do_robo.md)), além de receber mensagens de status dos jogadores.
O Game Controller comunica com os jogadores por três canais:

- Manda messagens de controle na frequência de 2Hz (transmissão [UDP](https://www.alura.com.br/artigos/quais-as-diferencas-entre-o-tcp-e-o-udp) pela porta 3838, num formato especificado pela struct `RoboCupGameControlData` no código fonte do GC). Essas mensagens de controle nem sempre representam o estado atual do jogo, especialmente depois de um gol ou a transição para o estado `playing`. Depois destes eventos eles continuam mantendo o estado anterior por até 15 segundos, ou até outro evento que não poderia ocorrer neste estado "falso".
- Recebe mensagens de status dos jogadores no intervalo de frequência de 0.5Hz até 2Hz (UDP [Unicast](https://www.eletronet.com/blog/entenda-a-diferenca-entre-unicast-multicast-e-broadcast/) na porta 3939, formato especificado pela struct `RoboCupGameControlReturnData`).
- Recebe mensagens de time dos robôs (UDP Broadcast porta 10000 + número do time, até 128 bytes de payload com formato arbitrário).

## Comunicação Geral

Esse tópico é baseado no livro de [regras do SPL 2024](https://spl.robocup.org/wp-content/uploads/SPL-Rules-2024.pdf). Membros devem se manter informados para possíveis mudanças.

Como citado anteriormente, a comunicação entre os robôs é feita por IPv4 e IPv6. A estrutura do IP segue o padrão `10.<interface>.<time>.<número>` com a interface sendo 0 para WiFi e 1 para Ethernet. O número do time é 47 e o número do robô. Então, por exemplo, o robô 13 da Rinobot comunicando via WiFi teria o IP `10.0.47.13`. O número do robô é configurado na geração da imagem do Robô, no arquivo `meta-hulks/recipes-hulks/network-config/network-config/id_map.json`.

Há um limite de 1200 pacotes UDP que podem ser transmitidos durante o jogo. Este limite é aumentado em 60 para cada minuto de acréscimo do jogo. Há penalidades para o time que exceder o limite.

## Comunicação entre Robôs
**TODO**