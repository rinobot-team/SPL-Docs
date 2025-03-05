# Roles

Este documento descreve as **Roles**, que são os papéis que um robô pode assumir em um jogo, como elas são definidas e como modificar o comportamento delas.  
Elas são aplicadas somente em tempo de execução, diferente da [Mari](https://github.com/rinobot-team/SPL-Mari) em que são definidas em tempo de compilação num arquivo JSON.  

> É importante ressaltar que as Roles são dinâmicas e mudam de acordo com o estado do jogo, posição dos robôs, proximidade da bola, etc.

## Enum `Roles`

A enumeração `Roles` é um conjunto de constantes que representam os papéis que um robô pode assumir. Pode ser encontrada na crate `/crates/types/src/roles.rs`.  
O robô pode assumir um destes papéis:

```rust
pub enum Role {
    DefenderLeft,
    DefenderRight,
    Keeper,
    Loser,
    MidfielderLeft,
    MidfielderRight,
    ReplacementKeeper,
    Searcher,
    #[default]
    Striker,
    StrikerSupporter,
}
```

## Funções das Roles

- Striker (Atacante): Assume a posse efetiva da bola, conduzindo chutes e jogadas ofensivas. O robô se torna **Striker** se detectar/possuir a bola e não houver outro **Striker** oficial, ou caso `forced_role` imponha isso.
- Loser (Perdedor): Entra nesse estado quando o **Striker** perde a bola: por exemplo, o robô era **Striker**, mas percebeu que não tem mais a bola ou recebeu mensagem informando perda de posse.
- Searcher (Buscador): Usa estratégias de busca para procurar a bola no campo. O robô assume **Searcher** quando não há posse clara da bola, e nenhuma outra função de ataque ou posse foi definida. Informa a posição da bola para os outros robôs caso encontre.
- Keeper (Goleiro): Protege o gol, principalmente em jogadas de ataque do adversário. É definido prioritariamente quando a partida indica que o time adversário está chutando ou se `forced_role` exigir.
- ReplacementKeeper (Goleiro Reserva): Substitui o **Keeper** quando esse está penalizado ou sofre alguma imposição. Ativo apenas se a lógica de distribuições de Roles não colocar outro robô como **Keeper** principal.
- DefenderLeft (Defensor Esquerdo) / DefenderRight (Defensor Direito): Defendem zonas específicas (lateral esquerda ou direita). São atribuídos quando o time está em postura defensiva e não há necessidade de **Keeper** adicional.
- MidfielderLeft (Meio-Campo Esquerdo) / MidfielderRight (Meio-Campo Direito): Ocupam funções intermediárias, auxiliando tanto na defesa quanto no ataque, de acordo com as necessidades do time (por exemplo, suprindo ausência de **Striker** ou **Loser**).
- StrikerSupporter (Apoio ao Atacante): Ajuda o **Striker** principal, mas não disputa diretamente a bola. Geralmente escolhido quando há mais de um jogador disponível para auxiliar na jogada ofensiva.

## Definindo as Roles

Há uma `struct` chamada `RoleAssignment` que é responsável por definir as roles dos robôs, controlar o estado atual do Role do jogador e outras informações (por exemplo, quando foi a última transmissão de mensagem).  
O método cycle decide se o Role deve mudar, checando se é preciso inicializar a função, se há forçamento de Role ou se é hora de enviar mensagens de rede.

Dentro de cycle, chama-se `process_role_state_machine`, que contém a lógica principal para troca de papéis, verificando:

1. Se a bola foi perdida (e o jogador é **Striker**), podendo virar **Loser**.
2. Se outro jogador já reivindicou **Striker**.
3. Se o jogador conseguiu a bola, mudando de **Loser** para **Striker** (ou de **Searcher** para **Striker**).
4. Se há algum Role forçado via forced_role.
5. Outras verificações específicas para cada Role.

## Definindo Roles Forçadas

Se `forced_role` estiver configurado, o jogador assume essa função imediatamente, sem depender das lógicas internas de detecção ou do estado do jogo:

```rust
// ...existing code...
if let Some(forced_role) = context.forced_role {
    self.role = *forced_role;
} else {
    self.role = role;
}
// ...existing code...
```
