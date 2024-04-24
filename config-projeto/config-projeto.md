# Configurações do Projeto e Compilação para o Simulador Webots

Esse tópico vai guiá-los na instalação de dependências e configurações necessárias para compilar o código do projeto para o simulador Webots. A Rinobot recomenda utilizar o sistema operacional Ubuntu 20.04 LTS para o desenvolvimento do projeto, porém Arch Linux e derivados também são suportados.

## Instalando dependências

Alguns pacotes são necessários para compilar o código do projeto. Para instalá-los, execute o comando abaixo:

=== "Ubuntu 20.04 LTS"  
1. Instalando Dependências
```bash  
    sudo apt update  
    sudo apt install git git-lfs build-essential libssl-dev pkg-config libclang-dev rsync cmake libhdf5-dev libopusfile-dev python3 libasound2-dev libluajit-5.1-dev libudev-dev
```
2. Instalando Webots
Baixe o Webots [aqui](https://cyberbotics.com/download) e a partir do arquivo XXX.deb instale com
```bash
    sudo dpkg -i XXX.deb
```

3. Instale a toolchain do Rust
```bash
    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

=== "Arch, Manjaro e derivados"
1. Instalando Dependências
```bash
    yay -S git git-lfs base-devel rustup rsync cmake clang hdf5 opusfile python webots
```
Para o Arch recomendamos usar o AUR Helper `yay`, caso não tenha, instale com `sudo pacman -S yay`.

2. Instale a toolchain do Rust
```bash
    rustup default stable
```

## Adiquirindo o código

Clone o [repositório](https://github.com/rinobot-team/Tamboerijn) do projeto e entre na pasta do projeto:
```bash
    git clone git@github.com:rinobot-team/Tamboerijn.git
    cd Tamboerijn
```
Recomendamos clonar o repositório com SSH, caso não tenha a chave SSH configurada, clone com HTTPS:
```bash
    git clone https://github.com/rinobot-team/Tamboerijn.git
    cd Tamboerijn
```
## Webots
Na Rinobot ainda não aderimos 100% ao Webots, porém planejamos migrar para ele em breve para simulação do comportamento, estratégia e resposta ao GameController. Mas, seguindo a documentação original da HULKs temos os seguintes passos:

### Compilando para o Webots
Na raíz do nosso repositório há um programa chamado `pepsi`, ele é uma multiferramenta que usamos para compilação e upload para o robô. Para compilar o projeto para o Webots, execute o comando abaixo:
```bash
    ./pepsi build
```
Caso seja a primeira vez que esteja utilizando o comando, ele primeiro irá compilar a ferramenta e depois compilar o projeto. Isso pode demorar alguns minutos.

### Rodando o simulaor
Depois de compilado, abre o Webots e abra o arquivo `webots/worlds/penalized.wbt` do repositório.

### Rodando no modo externo
Para não ser obrigado a reabrir o programa toda que recompilar o controlador você pode rodar o `webots/worlds/penalized_extern.wbt` e iniciar o controlador com o seguinte comando:
```bash
    ./pepsi run
```