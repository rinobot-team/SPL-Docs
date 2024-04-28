# Recording and Replay
A framework suporta gravação de dados e o replay deles depois de uma partida para melhor análise. Para cada instância do cycler, somente os inputs e estados dos nós no inicio do ciclo são gravados. Durante o replay, esses estados e inputs são usados para recalcular todos os outputs. Um servidor de comunicação durante o replay pode ser usado para investigar os dados gravados via [Twix](./twix.md).

## Gravação
- Upload manual para o robô
    - Use `./pepsi recording ...` para habilitar a gravação em diferentes taxas
        - Os parâmetros padrão são encontrados em `etc/parameters/framework.json`
    - Use `./pepsi upload ...` para fazer o upload normalmente
- Pregame
    - Use `./pepsi pregame --recording-intervals ... ...` para permitir a gravação em diferentes taxas em um passo
        - Isso vai setar os parâmetros de gravação para os padrões do `etc/parameters/framework.json`

Seja cauteloso habilitando os cyclers de visão, pois isso resulta em muitos dados sendo gravados. Cyclers de Top e Bottom podem ocupar todo o disco em aproximadamente 10 minutos.

Dados são gravados somente durante `PrimaryState::Ready`, `PrimaryState::Set` e `PrimaryState::Play`.

## Replayer
Assumindo que você já gravou alguns dados em um robô, é possivel utilizar o *replayer* para revisitar dados gravados.

- Faça download dos logs em um diretório `logs` dentro do repositório via `./pepsi postgame ... meu_replay_gamer ...`
- O diretório `meu_replay_gamer` agora contém diretórios com logs de cada robô. Cada diretório de robô contém um diretório com dados de replay de uma execução do binário `hulk`. Todos os arquivos de instâncias de cyclers precisam estar presentes, independente de se estão ativos ou não durante a gravação (neste caso estarão vazios)
- Inicie a ferramenta de replay apontando pro diretório do log que você quer revisitar: `./pepsi run --target replayer --meu_replay_gamer/robot1/10.1.24.42/123321`
- Conecte seu Twix ao `localhost` e abra os painéis
- Mova o slider para fazer os dados disponíveis para o Twix *Dica: Clique na caixa de texto e use as setas do teclado para "animar"*
- Tá pronto o sorvetinho