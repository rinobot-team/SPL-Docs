# Como instalar OpenCV 4.7.0 no seu computador
O OpenCV é uma biblioteca de visão computacional usada eventualmente por algumas equipes da SPL.
### Alguns usos da biblioteca na liga:
1. SPQRTeam 🇮🇹 (2016):
  - [Github](https://github.com/SPQRTeam/SPQRBallPerceptor/tree/master?tab=readme-ov-file)
  - [Tutorial de detecção de bola usando Haar Cascade](https://profs.scienze.univr.it/~bloisi/tutorial/balldetection.html)
2. Dutch Nao Team 🇳🇱 (2021):
  - [YOLO v3 (artigo)](https://www.researchgate.net/publication/355153667_Project_AI_Real-time_object_detection_and_avoidance_for_autonomous_Nao_Robots_performing_in_the_Standard_Platform_League)

## Como instalar
A maneira "padrão" de instalação do OpenCV é via compilação do código fonte. Por ser um processo relativamente complicado, um querido fez um script que faz esse processo.
1. Crie uma pasta em qualquer lugar (e.g opencv_installation)
2. Baixe [**esse** Makefile](https://github.com/hybridgroup/gocv/blob/release/Makefile)
3. Coloque o Makefile na pasta que você criou
4. dentro da pasta: `make install`
### Detalhes
1. Caso não tenha o `make`: `sudo apt install build-essential`
2. Vai dar um erro no final do script, algo como:
```bash
    go clean --cache
    rm -rf /tmp/opencv
    /bin/sh: 1: go: not found
    go run ./cmd/version/main.go
    make: go: No such file or directory
    make: *** [Makefile:315: verify] Error 127
```
Não se preocupe.

## Como testar a instalação
`TODO`
