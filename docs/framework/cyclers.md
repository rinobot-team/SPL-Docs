# Cyclers
Um cycler no sistema de controle robótico Tamboerijn é um sobcomponente que cicla os nós. O nome "cycler" vem da sua característica principal, ele contém um loop que itera sobre os dados que estão entrando e produz algum dado de saída em cada iteração. Os cyclers chamam seu método interno `cycle()` em cada iteração, esse método consiste de trẽs etapas:   
1. **Setup**: Espera por dados novos e prepara o ciclo.   
2. **Processamento**: Roda os nós necessários nos dados recebidos.   
3. **Término**: Manda comando para os atuadores e guarda os dados anteriores antes de chamar o próximo ciclo.

Existem múltiplos cyclers em todo o software, e uma das principais tarefas da framework é permitir que eles comuniquem entre si. Por exemplo, na etapa de *setup*, dados de outros cyclers e da comunicação são coletado. Ou, na etapa de *término*, os dados produzidos durante o *processamento* são enviados para outros cyclers, caso necessário.

Cyclers são separados em duas categorias:  
1. **Cyclers de tempo real**: Usados para controle do robô, como em atuadores.  
2. **Cyclers de percepção**: Usados para processar dados de sensores, como o de visão por exemplo.  

## Cycler de tempo real
Um cycler de tempo real existe externamente ao ambiente, integra dados oriúndos dos cyclers de perceção, e produz alguma saída no fim do ciclo. Um exemplo é o cycler que roda em tempo real sincronizado com o intervalo do LoLA (83 hz), ele recebe dados de sensores do HULA/LoLA através da interface de hardware e cria uma saída para os atuadores que é mandada de volta para o HULA/LoLA. O cycler de controle integra dados de todos os outros cyclers de percepão (áudio, SPL network e visão) em sua pipeline de filtragem. Mais informações sobre essa pipeline podem ser encontradas em [Filtragem](./filtering.md). O cycler de controle comtém todo código de robótica que precisa ser avaliado em cada ciclo de tempo real. Em outras palavras, ele possui todos os nós necessários para gerar novos outputs. Qualquer nó que seja muito custoso computacionalmente, como a visão, é executada em seu próprio cycler de percepção.

## Cycler de percepção
Além dos cyclers de tempo real centrais, existem múltiplos cyclers de percepção que percebem dados do mundo exterior e pré-processam eles. A saída de cada cycler de percepção é integrada aos cyclers de tempo real para respeitar suas saídas de tempo real. Já que cyclers de percepção rodam em paralelo aos de tempo real - e são capazes de reter dados históricos - eles podem rodar em intervalos de ciclo distintos. Esses cyclers normalmente esperam por um evente engatilhado externamente, como nova imagem da camera ou mensagem da rede por exemplo. Com isso, o início do processamento é anunciado aos cyclers de tempo real na etapa de *setup*, além de adiquirir dados requisitados dos cyclers de tempo real. A saída de dado dos cyclers de percepção é então enviada para os cyclers de tempo real na etapa de *término*. Mais informações sobre os dados que transitam entre cyclers pode ser encontrada em [Filtragem](./filtering.md). Os cyclers de percepção existentes são:   
1. **audio**: Recebe dados de áudio da [Interface de Hardware](./hardware-interface.md), basicamente dados dos microfones do NAO, principalemente para reconhecimento de apito.   
2. **spl_network**: Espera por mensagens vindas da rede ou requests de envio de mensagem vindo de outros cycler. Cada ciclo ele, ou pré-processa mensagens recebidas via parsing (análise sintática), ou envia mensagens para o exterior, a rede.   
3. **vision_top**: Recebe dados da camera superioro do NAO via [Interface de Hardware](./hardware-interface.md), e processa a imagem para retirar e identificar diferentes features.   
4. **vision_bottom**: Similar ao `vision_top`, mas para a camera inferior do NAO.