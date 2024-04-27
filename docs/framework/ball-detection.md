# Detecção de Bola
A Detecção de Bola é uma parte de extrema importância durante toda a extensão do jogo, orientando o robô e o própio código em si. Para isso, utiliza a captação e e interpretação das imagens das câmeras superior e infeirior presentes no NAO, coleta e processa as imagens, obtendo as informações necessárias para as outras estruturas de decisão, como a Movimentação e o Comportamento.

Em um alto nível, a detecção de bola segue as seguintes etapas:  
- Detecção das imagens pelas cameras (`local`) e interpretação das informações nos formatos adequados
- Interpretação e avaliação dos dados das imagens pelas redes neurais (`local`)
- Obtenção da presença da bola e a sua localização

A seguir, cada um dos pontos do processo de detecção será analisada:

# Estruturas

## NeuralNetworks
A estrutura **NeuralNetworks** contém três redes neurais compiladas: `preclassifier`, `classifier` e `positioner`. Estas redes neurais são provavelmente usadas para classificar se uma determinada região da imagem contém uma bola (preclassifier e classifier) e para determinar a posição da bola na imagem (positioner).

## BallCluster
A estrutura **BallCluster** é usada para agrupar várias CandidateEvaluation que são consideradas parte da mesma bola. Ela contém um Circle que representa a localização e o tamanho da bola, e um vetor de referências para as CandidateEvaluation que são membros do cluster.

## Estados de ciclo
As estruturas **CreationContext**, **CycleContext** e **MainOutputs** são usadas para gerenciar o estado e os parâmetros do ciclo de detecção de bolas. **CreationContext** contém a interface de hardware e os parâmetros de detecção de bolas. **CycleContext** contém várias entradas e parâmetros necessários para um ciclo de detecção, incluindo os candidatos a bola, a matriz da câmera, os candidatos da grade de perspectiva, a imagem e o raio da bola. **MainOutputs** contém a saída principal do ciclo de detecção, que é um vetor opcional de bolas detectadas.

## BallDetection
A estrutura **BallDetection** é onde o processo da detecção de bola ocorre, com todas as partes praticas do procedimento. Ela é definida com um campo neural_networks do tipo NeuralNetworks. Este campo é marcado com o atributo  `#[serde(skip, default = "deserialize_not_implemented")]`, o que indica que ele deve ser ignorado durante a serialização e deserialização.

A seguir, será analisada cada uma das funções presentes dentro da estrutura **BallDetection**:

## Funções dentro da Estrutura BallDetection

Possui dois métodos principais: `new` e `cycle`.

### new
O método `new` é usado para criar uma nova instância de **BallDetection**. Ele compila três redes neurais: um pré-classificador, um classificador e um posicionador, que são armazenados na estrutura **NeuralNetworks**. O método do cycle é a principal função que processa uma imagem para detectar bolas. É necessário um **CycleContext** como entrada, que contém a imagem e outros parâmetros necessários.

### cycle
A função de `cycle` é a principal função que orquestra o processo de detecção de bola. Começa avaliando os candidatos para detecção de bola usando a função `evaluate_candidates`(colocar os endereços para as funcoes mais abaixo). Os candidatos são então filtrados com base no fato de terem um círculo corrigido. Para cada bola detectada, um peso de mesclagem é calculado usando a função `calcule_ball_merge_factor`(colocar os endereços para as funcoes mais abaixo). As bolas são então agrupadas usando a função `cluster_balls`(colocar os endereços para as funcoes mais abaixo) e projetadas no solo usando a função `project_balls_to_ground`(colocar os endereços para as funcoes mais abaixo). A função retorna as bolas detectadas.

A seguir, se encontram as funções executadas em repetiçãoo pela prórpia funcao `cycle`:

### evaluate_candidates
A função `evaluate_candidates` obtém uma lista de círculos candidatos (bolas potenciais), uma imagem e um conjunto de redes neurais. Ele amplia cada círculo candidato, amostra a imagem dentro do círculo ampliado e, em seguida, usa a rede neural pré-classificadora `preclassifier` para avaliar a amostra. Se a confiança do pré-classificador estiver acima de um determinado limite, a rede neural do classificador `classifier` será usada para avaliar melhor a amostra. Se a confiança do classificador também estiver acima de um determinado limite, a rede neural do posicionador `positioner` é usada para corrigir a posição e o tamanho do círculo. A função retorna uma lista de estruturas **CandidateEvaluation**, cada uma contendo um círculo candidato, as confianças do pré-classificador e do classificador e o círculo corrigido (se houver).

### calculate_ball_merge_factor
A função `calculate_ball_merge_factor` calcula um fator de mesclagem para uma bola, que é usado para determinar se várias bolas devem ser mescladas em uma única bola. O fator de mesclagem é uma combinação da confiança do classificador, da proximidade do círculo corrigido ao círculo candidato e de quanto do círculo corrigido está contido na imagem.

### cluster_balls
A função `cluster_balls` recebe uma lista de bolas e um fator de raio de mesclagem. Ele agrupa as bolas com base na proximidade umas das outras. Se uma bola estiver dentro do raio de fusão de um cluster, ela será adicionada a esse cluster. Caso contrário, um novo cluster será criado para aquela bola. A função retorna uma lista de estruturas BallCluster, cada uma contendo um círculo representando a localização e o tamanho do cluster, e uma lista das bolas no cluster.

### project_balls_to_ground
A função `project_balls_to_ground` obtém uma lista de clusters de bolas, uma matriz de câmera e um raio de bola. Ele projeta cada cluster do plano da imagem para o plano terrestre usando a matriz da câmera e o raio da bola. A função retorna uma lista de estruturas **Ball**, cada uma contendo a posição da bola no solo e a localização da bola na imagem.

### preclassify_sample, classify_sample e position_sample
As funções `preclassify_sample`, `classify_sample` e `position_sample` usam uma referência mutável a uma *CompiledNN* (rede neural compilada) e uma referência a uma amostra como argumentos. Uma amostra aqui provavelmente é uma pequena seção da imagem que está sendo processada.

Em todas as três funções, a amostra é alimentada na rede neural. Isso é feito iterando os pixels da amostra e colocando-os na camada de entrada da rede neural. A função `network.apply()` é então chamada para executar a rede neural na entrada.

As funções `preclassify_sample` e `classify_sample` retornam a primeira saída da rede neural como um número de ponto flutuante. Esta saída é provavelmente uma pontuação de confiança que indica a probabilidade de a amostra conter uma bola.

### position_sample
A função `position_sample`, por outro lado, retorna um **Circle** que representa a posição e o tamanho da bola na amostra. O centro e o raio do círculo são determinados pelas três primeiras saídas da rede neural.

### sample_grayscale
A função `sample_grayscale` usa uma referência a [YCbCr422Image](https://github.com/leandromoreira/digital_video_introduction/issues/93) e a **Circle** como argumentos. O **Circle** representa uma bola candidata dentro da imagem. A função cria uma amostra em tons de cinza da imagem dentro do círculo candidato. Isso é feito iterando os pixels dentro do círculo, convertendo cada pixel em escala de cinza e colocando o valor da escala de cinza na amostra. A amostra é então devolvida.

### evaluate_candidates
A função `evaluate_candidates` pega uma lista de possíveis candidatos à bola (representados como círculos), uma imagem e um conjunto de redes neurais. Ele avalia cada candidato ampliando o círculo do candidato, amostrando os valores da escala de cinza da imagem dentro desse círculo e, em seguida, alimentando essa amostra em duas redes neurais: um `preclassifier` e um `classifier`. O `preclassifier` é usado para eliminar rapidamente candidatos improváveis, enquanto o `classifier` é usado para uma avaliação mais detalhada. Se a confiança do `classifier` estiver acima de um determinado limite, o círculo do candidato é corrigido utilizando uma terceira rede neural, o posicionador.

### image_container
A função ``bounding_box_patch_intersection`` calcula a área de intersecção entre as caixas delimitadoras de dois círculos (o círculo candidato e um círculo candidato de patch). Isso é usado para determinar quanta sobreposição existe entre os dois círculos.

### calcula_ball_merge_factor
A função ``image_containment`` calcula quanto de um círculo está contido na imagem. Isto é feito criando um retângulo que representa a imagem, calculando a área de interseção entre a caixa delimitadora do círculo e o retângulo da imagem e, em seguida, dividindo-a pela área da caixa delimitadora do círculo.

### merge_balls
A função ``calcula_ball_merge_factor`` calcula um fator de mesclagem para uma bola. Isso é feito aumentando a confiança do classificador, a proximidade da correção (quão próximo o círculo corrigido está do círculo candidato) e a contenção da imagem para certas potências e depois multiplicando-as.

### merge_balls
A função ``merge_balls`` mescla uma lista de bolas em uma única bola. Isto é feito calculando uma média ponderada dos centros e raios das bolas, onde os pesos são os pesos mesclados das bolas.

### cluster_balls
A função ``cluster_balls`` agrupa uma lista de bolas com base na proximidade umas das outras. Isso é feito iterando sobre as bolas e, para cada bola, encontrando um cluster cujo centro esteja a uma certa distância da bola. Se tal agrupamento for encontrado, a bola é adicionada ao agrupamento e o círculo do agrupamento é atualizado fundindo-o com o círculo da bola. Se tal cluster não for encontrado, um novo cluster é criado tendo a bola como único membro.

### project_balls_to_ground
Finalmente, a função ``project_balls_to_ground`` projeta uma lista de grupos de bolas no chão. Isso é feito convertendo as coordenadas de pixel do centro do cluster em coordenadas terrestres usando uma matriz de câmera. Se esta conversão for bem-sucedida, uma nova bola é criada com as coordenadas do solo como posição e o círculo do cluster como localização da imagem. A função retorna uma lista dessas bolas.