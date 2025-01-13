# Indice

Esta seção visa explicar como o código funciona no NAO de maneira mais abstrata e geral, já tendo explicado os módulos fornecidos pela framework.

## Cyclers

Atualmente existem seis [threads](https://en.wikipedia.org/wiki/Thread_(computing)) rodando no NAO, em jargão de Tamboerijn (e HULKs) são chamados de `cyclers`:

- `Control`
- `VisionTop`
- `VisionBottom`
- `Audio`
- `SPLNetwork`
- `ObjectDetectionTop`

De forma resumida elas são responsáveis por:

### Controle

Este Cycler é responsável pelo controle alto-nível do robô, isso inclui tasks relacionadas à comportamento, seleção de ações para execução, relacionadas à movimentação como execução de ações selecionadas como driblar, chutar, ficar em pé, entre outras.  
A thread de controle roda com maior prioridade que as outras com uma frequência maior de 83 Hz, ou seja, a cada 12 ms.  
*Se estiver mais curioso da uma fuçada no código, nos diretórios de `behavior` na crate `control`*

### VisionTop e VisionBottom

Esses dois Cyclers processam as imagens das câmeras do NAO, a `VisionTop` processa a imagem da câmera superior e a `VisionBottom` a inferior. Isso inclui segmentação de imagem, detecção de bola, de linhas, entre outros.  
Ambos rodam a 30 Hz (a frequência das câmeras), ou seja, a cada 33 ms.

### Audio

Esse Cycler é responsável por processar o áudio captado pelos microfones do NAO, ou seja, a detecção de apito.

### SPLNetwork

Essa thread lida com todas mensagens recebidas e enviadas pela rede SPL, então, é responsável pela comunicação com o GameController e com os outros robôs do time.

### ObjectDetectionTop

Esse Cycler ainda não é presente no Tamboerijn, mas é presente no código da HULKs. Ele é responsável por detectar gestos do árbitro. Nossa liga ainda não utiliza essa funcionalidade, mas futuramente pode ser necessário implementar.

## Podemos dividir a robótica em três áreas principais

Na Rino estruturamos de uma forma um pouco diferente da HULKs, então existem disparidades entre este documento e o oficial da equipe alemã.

### Sensoriamento

Como o robô percebe o mundo ao seu redor. No caso do NAO, temos câmeras, microfones, sensores de toque, ultrassom,giroscópios, acelerômetros, etc. Essa área é responsável por:

- [Filtragem](./sensoriamento/filtragem.md)
- [Visão](./sensoriamento/visao.md)
- [Áudio](./sensoriamento/audio.md)

### Comportamento e Estratégia

Como o robô decide o que fazer, comunicação com GameController (SPLNetwork) e time, e `roles` (papéis) dos robôs em campo.  

- [Comportamento](./comportamento/comportamento.md)
- [SPL Network](./comportamento/spl-network.md)

### Movimentação

Toda a parte física do robô, como modelo matemático, planejamento de passos, caminhar, chutar, etc.

- [Planejamento de Passos](./movimentacao/step-planning.md)
- [Caminhar](./movimentacao/caminhar.md)
- [Chutar](./movimentacao/chutar.md)
- [Motion Files](./movimentacao/motion-files.md)
