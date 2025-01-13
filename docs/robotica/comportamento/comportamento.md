# Comportamento

O comportamento do robô é controlado pelo nó `node`. Nele é decidido quais ações serão tomadas. Essa organização é feita em dois passos:

## Coleta de Ações

Nesse passo, o estado do `world` e outros inputs são utilizados para criar uma lista ordenada por prioridade de ações que o robô **gostaria de tomar**. Exemplos:  

```rust
let mut actions = vec![
            Action::Unstiff,
            Action::SitDown,
            Action::Penalize,
            Action::Initial,
            Action::FallSafely,
            Action::StandUp,
        ];
```

Dependendo da situação atual, diferentes ações podem ser adicionadas à lista caso necessário:

*Se a role atual é de Goleiro e uma cobrança de pênalte está acontecendo, as ações `Jump` e `PrepareJump` são adicionadas à lista*  

```rust
match world_state.robot.role {
        Role::Keeper => match world_state.filtered_game_controller_state {
            Some(FilteredGameControllerState {
                game_phase: GamePhase::PenaltyShootout { .. },
                ..
            }) => {
                actions.push(Action::Jump);
                actions.push(Action::PrepareJump);
            }
            _ => actions.push(Action::DefendGoal),
        },
```

## Seleção de Ação

Agora, a lista é iterada até que uma ação, que é executável no momento, é encontrada. Essa ação retorna um `motion_command`, que é enviado para o `motion_selection` no Cycler de [Motion](../movimentacao/README.md).  
