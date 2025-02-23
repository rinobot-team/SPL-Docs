# Nodes (Nós)

Os Nós normalmente contém código de robótica propriamente dito e são intercambiáveis entre cyclers. Cada nó é caracterizado po uma função `cycle()` que é chamada em cada ciclo. Essa função pega os inputs do nó como parâmetros e retorna os outputs deste node. Além disso, nós possuem um estado que é preservado entre ciclos.

![Diagrama de um nó](node.drawio.png)

Nós são structs normais do Rust em que os campos da struct representam o estado, e um método chamado `cycle()` no `impl` do nó que representa a função de ciclo. Esse conceito permite escrever os nós na forma mais otimizada para a linguagem Rust. Um nó pode ter multiplos inputs de diferentes tipos que podem ser adicionados ao struct. Aqui está um exemplo de nó, mais informação pode ser encontrada em [Macros](./macros.md):  

```rust
use std::{collections::VecDeque, time::SystemTime};

use color_eyre::Result;
use context_attribute::context;
use framework::{MainOutput, PerceptionInput};
use serde::{Deserialize, Serialize};
use types::{cycle_time::CycleTime, filtered_whistle::FilteredWhistle, whistle::Whistle};

#[derive(Deserialize, Serialize)]
pub struct WhistleFilter { // (1)
    detection_buffer: VecDeque<bool>,
    was_detected_last_cycle: bool,
    last_detection: Option<SystemTime>,
}

#[context]
pub struct CreationContext {} // (2)

#[context]
pub struct CycleContext { // (3)
    buffer_length: Parameter<usize, "whistle_filter.buffer_length">, // (4)
    minimum_detections: Parameter<usize, "whistle_filter.minimum_detections">,
    cycle_time: Input<CycleTime, "cycle_time">, // (5)
    detected_whistle: PerceptionInput<Whistle, "Audio", "detected_whistle">, // (6)
}

#[context]
#[derive(Default)]
pub struct MainOutputs {
    pub filtered_whistle: MainOutput<FilteredWhistle>,
}

impl WhistleFilter {
    pub fn new(_context: CreationContext) -> Result<Self> { // (7)
        Ok(Self {
            detection_buffer: Default::default(),
            was_detected_last_cycle: false,
            last_detection: None,
        })
    }

    pub fn cycle(&mut self, context: CycleContext) -> Result<MainOutputs> { // (8)
        let cycle_start_time = context.cycle_time.start_time;

        for &is_detected in context
            .detected_whistle
            .persistent
            .values()
            .flatten()
            .flat_map(|whistle| &whistle.is_detected)
        {
            self.detection_buffer.push_front(is_detected);
        }
        self.detection_buffer.truncate(*context.buffer_length); // (9)

        let number_of_detections = self
            .detection_buffer
            .iter()
            .filter(|&&was_detected| was_detected)
            .count();
        let is_detected = number_of_detections > *context.minimum_detections;
        let started_this_cycle = is_detected && !self.was_detected_last_cycle;
        if started_this_cycle {
            self.last_detection = Some(cycle_start_time);
        }
        self.was_detected_last_cycle = is_detected;

        Ok(MainOutputs {
            filtered_whistle: FilteredWhistle {
                is_detected,
                last_detection: self.last_detection,
                started_this_cycle,
            }
            .into(),
        }) //(10)
    }
}
```

1. Estados do nó.
2. Contexto de criação do nó. Mais informações em [Macros](./macros.md).
3. Contexto de ciclo do nó. Mais informações em [Macros](./macros.md).
4. Parâmetros extraídos de `default.json`. Pode ser mudado em tempo de execução utilizando o [Twix](../ferramental/twix.md).
5. Input de outro nó, do tipo `CycleTime`.
6. Input de outro nó, mas com dados persistentes e transientes.
7. Será chamado na criação do nó. Basicamente, é o construtor do nó.
8. Será chamado a cada ciclo. É a função principal do nó.
9. Parâmetro declarado pelo usuário. Então tem de ser uma referência. Aqui o parâmetro é desreferenciado para ser utilizado.
10. Retorna os outputs do nó. O `into()` é utilizado para converter o tipo `FilteredWhistle` para `MainOutput<FilteredWhistle>`.
