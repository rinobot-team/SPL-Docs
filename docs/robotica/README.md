# Indice
Esta seção visa explicar como o código funciona no NAO de maneira mais abstrata e geral, já tendo explicado os módulos fornecidos pela framework.

Atualmente existem cinco [threads](https://en.wikipedia.org/wiki/Thread_(computing)) rodando no NAO, em jargão de Tamboerijn (e HULKs) são chamados de `cyclers`:

- `Control`
- `VisionTop`
- `VisionBottom`
- `Audio`
- `SPLNetwork`

**TODO:** Elaborar

## Podemos dividir a robótica em três áreas principais:
Na Rino estruturamos de uma forma um pouco diferente da HULKs, então existem disparidades entre este documento e o oficial da equipe alemã.

### Sensoriamento
Como o robô percebe o mundo ao seu redor. No caso do NAO, temos câmeras, microfones, sensores de toque, ultrassom,giroscópios, acelerômetros, etc. Essa área é responsável por:

- [Filtragem](./sensoriamento/filtragem.md)
- [Visão](./sensoriamento/visao.md)
- [Áudio](./sensoriamento/audio.md)

## Comportamento e Estratégia
Como o robô decide o que fazer, comunicação com GameController (SPLNetwork) e time, e `roles` dos robôs em campo.  

- [Comportamento](./comportamento/comportamento.md)
- [SPL Network](./comportamento/spl-network.md)

## Movimentação
Toda a parte física do robô, como modelo matemático, planejamento de passos, caminhar, chutar, etc.

- [Planejamento de Passos](./movimentacao/step-planning.md)
- [Caminhar](./movimentacao/caminhar.md)
- [Chutar](./movimentacao/chutar.md)
- [Motion Files](./movimentacao/motion-files.md)