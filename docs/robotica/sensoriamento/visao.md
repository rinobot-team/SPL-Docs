# Visão
**Esta seção está em desenvolvimento**

A visão da Rinobot-Jaguar atualmente utiliza uma Rede Neural Convolucional (CNN) para identificar a bola, e no futuro, robôs adversários. A CNN utilizada é a [Tiny YOLOv3](https://opencv-tutorial.readthedocs.io/en/latest/yolo/yolo.html), uma versão reduzida do YOLO (You Only Look Once) destinada a dispositivos embarcados.

O treinamento é feito em computador para melhor utilizar recursos de processamento, como GPU, e o modelo é compilado em uma [matriz de pesos](https://python-course.eu/machine-learning/neural-networks-structure-weights-and-matrices.php) 

## Treinamento
Para o treinamento do modelo, utilizamos o [Darknet](https://github.com/hank-ai/darknet), uma framework que direciona a YOLO para detecção de objetos. Como fonte de dados, utilizamos o [ImageTagger](https://imagetagger.bit-bots.de/users/team/28/), um banco de imagens retiradas de jogos da Robocup e [German Open](https://robocup.de/german-open/?lang=en) e disponibilizado gratuitamente. Mais precisamente utilizamos o dataset da NaoDevils, de Dortmund.

## Detecção de Marcas
*EM DESENVOLVIMENTO*

## Detecção de Robôs
*EM DESENVOLVIMENTO*

## Haar Cascade
*NO FUTURO PLANEJAMOS UTILIZAR O HAAR CASCADE NOVAMENTE, PARA SELEÇÃO DE CANDIDATOS*

**TODO: Adicionar mais conteúdo**