# Aliveness
É um sistema para buscar informações de status dos NAOs na rede. Ele consiste de duas partes: O service rodando no NAO e o client rodando no computador.

## Informações Disponíveis via Aliveness
As seguintes informações estão disponíveis via Aliveness:

- Hostname
- Versão atual do HULKs-OS
- Estados dos serviços do sistema: HAL (Hardware Abstraction Layer), HuLA, HULK e LoLA
- Estado da carga da bateria e corrente
- ID da cabeça
- ID do corpo
- Nome na rede Wireless
- Temperatura de juntas
- Nome da interface da qual o beacon é recebido (atualmente sempre *enp4s0*)

## Serviço de Aliveness
O serviço de Aliveness é compilado junto com a imagem do HULKs-OS e é incluso nele. É iniciado com a primeira conexão com a rede via Ethernete escuta todas as mensagens enviadas ao endereço de multicast `224.0.0.42` assim como o próprio endereço de IP.

Quando recebe um pacote UDP com o conteúdo `BEACON`, ele responde com as informações a cima. O pacote de resposta é um JSON com as informações.

## Client de Aliveness
O [Pepsi](./pepsi.md) possui um client de aliveness completo com diferentes níveis de verbosidade e opções de exportação.

Exemplo de uso:
```bash
    ./pepsi aliveness
    ./pepsi aliveness 27 32
    ./pepsi aliveness --json
    ./pepsi aliveness --timeout 500 -v
```

Quando executando qualquer um dos subcomandos no pepsi, ele irá mandar a mensagem de beacon citada anteriomente para o endereço de multicast ou aos endereços de IP dos NAOs. Então ele coleta todos as respostas dentro do timeout e filtros de acordo com o nível de verbosidade escolhido.

## Possíveis problemas com Firewall
Quando não há endereço do NAO especificado, o beacon é mandado via multicast e as respostas são recebidas via unicast. Como as respostas são de diferentes IPs, o firewall pode bloquear as respostas (confunde com um ataque de DDOS).

Nesse caso, é possível abrir uma permissão no firewall, como exemplo temos o ufw, que é o firewall mais comum em sistemas unix:
```bash
    ufw allow proto udp from 10.1.24.0/24
```