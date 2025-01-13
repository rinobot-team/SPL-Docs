# Audio

O NAO v6 possui 4 microfones, que estão localizados na cabeça. Eles suportam frequências de 100 até 10.000 Hz. Se você for nerd o suficiente, pode dar uma olhada na [documentação oficial](http://doc.aldebaran.com/2-8/family/nao_technical/microphone_naov6.html) da Aldebaran.  

O Cycler de audio só possui 2 nós, para gravar áudio e detectar apitos.  

## Gravação de áudio

Lê amostras de áudio do microfone de `MicrophoneInterface` e as armazena na [database](../../framework/databases-types.md) do Cycler de áudio.  

## Detecção de apitos

Como diz o nome, detecta apitos. Similar ao futebol humano, os árbitros usam apitos para sinalizar o início e o fim de uma partida. Mais detalhes estão no [livro de regras do SPL](https://spl.robocup.org/wp-content/uploads/SPL-Rules-master.pdf) (sempre procure o mais atual).  

De forma simplificada, o detector de apitos funciona comparando amostras de áudio pré gravadas com as amostras atuais. Se a diferença entre as amostras for menor que um limiar, o apito é detectado. Ela utiliza [FFT](https://en.wikipedia.org/wiki/Fast_Fourier_transform).  

Um beijo para a [NAO Devils](https://naodevils.de/) que desenvolveu o detector de apitos e publicou um [artigo](https://data.tu-dortmund.de/dataset.xhtml?persistentId=doi:10.17877/tudodata-2024-m0f6bmi1).
