# Compilando a Imagem pro NAO e adiquirindo a SDK
**Essa seção ainda está incompleta e não está totalmente testada, erros podem ocorrer**  

Atualmente utilizamos o [Projeto Yocto](https://www.yoctoproject.org/) para criar uma imagem OPN flashável para o NAO, além de um SDK compatível para desenvolvimento direcionado ao robô. Esse SDK contém uma toolchain completa para cross-compilação e pode ser facilmente instalada nos computadores de desenvolvimento.

A Rinobot-Jaguar atualmente utiliza o HULKsOS fornecido pela equipe HULKs para os NAOs, no futuro temos intenção de fazer nossa propria imagem.

## Tamboerijn e Yocto SDK já existentes
Se você já possui o SDK do Yocto instalado, e nosso [reposiório do Tamboerijn](https://github.com/rinobot-team/Tamboerijn) clonado, é possível utilizar o `pepsi` de forma imediata:
    
```bash
    ./pepsi upload <número/IP>
```
O [`pepsi`](../ferramental/pepsi.md) vai automaticamente baixar o SDK do GitHub e perguntar para você para instalá-lo durante a compilação.

## Criação da Imagem e SDK
O Projeto Yocto utiliza o [BitBake](https://www.yoctoproject.org/docs/3.1/bitbake-user-manual/bitbake-user-manual.html) como engine de execução de tasks e oferece uma camada de abstração para a criação e modificação de imagens já existentes. Combinado com o [OpenEmbedded](https://www.openembedded.org/wiki/Main_Page), toda a árvore de criação é estruturada em camadas. Nos baseando no processo utilizado pela HULKs, temos as camadas meta-nao e meta-hulks.

### Configuração

Primeiro crie um diretório para trabalhar, tenha pelo menos 100 GB livres de espaço interno para evitar erros, esse diretório não pode fazer parte de um repositório git e de preferência deve ser feito na sua pasta `home` caso esteja no Linux, por convenção nomearemos esse diretório de `worktree/`.

```bash
    cd ~ # Acessa o diretório home
    mkdir worktree
    cd worktree
```

Para configuração do projeto é utilizada a framework [siemens/kas](https://github.com/siemens/kas). Para configurar a estrutura do projeto usaremos a versão em container com o script `kas-container`, ele pode ser encontrado no repositório [SPL-assets](https://github.com/rinobot-team/SPL-assets). Para evitar erros recomendamos modificar as permissões do script para que ele possa ser executado:

```bash
    chmod +x kas-container
```

Baixe e instale o [Docker](https://docs.docker.com/desktop/install/linux-install/). Utilize este comando para testar se o Docker foi instalado corretamente:

```bash
    docker --version
```

Caso ocorra um erro de permissão parecido com este: `docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock` acesse [esta questão do StackOverflow](https://stackoverflow.com/questions/48957195/how-to-fix-docker-got-permission-denied-issue)


Clone o repositório `meta-nao` da HULKs para dentro da pasta `worktree/`:

```bash
    git clone git@github.com:hulks/meta-nao
```

### Criação da Imagem

O NAOv6 utiliza o LoLA e HAL para comunicação com a placa do peito. Esses programas são fornecidos pela SoftBank já compilados na imagem OPN de Robocupper. Para conseguí-los é preciso extrair seus binários da imagem OPN, para isso é utilizado o script `meta-nao/meta/recipes-support/aldebaran/extract_binaries.sh`. Para gerar o arquivo contendo os binários citados siga esses passos:

```bash
    cd meta-nao/meta/recipes-support/aldebaran
    mkdir -p aldebaran-binaries
    ./extract_binaries.sh -o aldebaran-binaries/aldebaran_binaries.tar.gz <imagem-robocupper.opn>
```

Esta imagem pode ser adquirida entrando em contato com a RoboCup SPL TC ou, para membros da Rinobot, baixando-a do Google Drive (Em `SPL>Utilitários>Imagens Do Sistema>nao-2.8.5.11_ROBOCUP_ONLY_with_root.opn`).  
**TODO:** Colocar imagem no repositório do SPL-assets.

Caso ocorra algum erro referente a `libguestfs` e `supermin` execute o comando como `sudo`, em caso de falta do pacote `guestmount` ou `patchelf` instale-os com o comando:

```bash
    sudo apt install guestmount patchelf
```

Execute o `kas` dentro da pasta `worktree/` referenciando o arquivo e configuração do projeto `kas-project.yml`:

```bash
    ./kas-container -d shell meta-nao/kas-project.yml
```

O `kas` consegue todas as fontes necessárias para o build e as configura em seus respectivos diretórios dentro do `worktree/`. O script deve demorar alguns minutos para rodar e ao fim seu diretório de trabalho deve estar parecido com isso:

```bash
    worktree/
    ├── build
    ├── meta-clang
    ├── meta-congatec-x86
    ├── meta-intel
    ├── meta-nao
    ├── meta-openembedded
    └── poky
```

Se o shell do seu container não inicializar você talvez tenha que setar a variável de ambiente `TERM` corretamente:

```bash
    export TERM=xterm
```

Para buildar a imagem OPN, execute este comando dentro do terminal do container:

```bash
    bitbake nao-image
```

Isso gera e executa tudo necessário para construir o arquivo `.opn`. Este processo pode levar algumas horas dependendo da capacidade do seu computador e necessita de conexão com a internet. O BitBake possui caching, então pequenas alterações não levarão tanto tempo, mas na primeira vez prepare uma xícara de café e uns pães de queijo.

Ao final do processo, o arquivo `.opn` estará disponível em `worktree/build/tmp/deploy/images/nao-v6/nao-image-HULKsOS-<versão>.ext3.gz.opn`. A partir daqui, o arquivo pode ser gravado em um pendrive e utilizado para flashar o NAO.

## Migrando da HULKs para Rinobot-Jaguar

*Esta parte é bem imprecisa e ainda não foi testada*

É preciso modificar alguns arquivos com informações hardcoded tanto nos arquivos de criação de imagem quanto no Tamborijn. Lembrese que o número do nosso time na Robocup é **47**. Os IDs de cabeça e corpo todos os robôs estão no [Drive da Rino](https://docs.google.com/document/d/16nKMPdGDzVwUSrvvkynVEvNF-w7cSi8O5gUpb4GqNBw/edit).

### Modificando a Imagem (meta-hulks)

Em `worktree/meta-hulks/recipes-hulks/network-config/configure-network` é preciso mudar os IPs para o nosso código, onde tem o 24 da HULKs coloque 47 (Ex: `10.1.24.x` para `10.1.47.x`).

Em `worktree/meta-hulks/recipes-hulks/network-config/network-config/id_map.json` é preciso colocar os IDs de cabeça e corpo de todos os robôs. O conteúdo do arquivo do drive pode ser só copiado e colado no lugar.

As configurações de conexão do wifi podem ser mudadas, o padrão já funciona corretamente, mas se for necessário modificar isso basta ir nos arquivos `worktree/meta-nao/recipes-conf/nao-wifi-conf/nao-wifi-conf/*.psk`.

Para mudar o nome da Distro vá em `worktree/meta-hulks/conf/distro/HULKsOS.conf`. É necessário mudar a variável `DISTRO` e renomear o arquivo com grafia idêntica, além de mudar o nome no `meta-hulks/kas-project.yml`.

### Modificando o Tamboerijn

Mudar o número do time no arquivo `crates/spl_network/src/lib.rs`.  

Em alguns lugares do Pepsi também é necessário mudar o número do time `tools/pepsi/src/parsers.rs`.  

No Twix também é necessário mudar o número do time `tools/twix/src/completion_edit.rs`.

E também é necessário importar os IDs de cabeça e corpo de todos os robôs no arquivo `etc/configuration/hardware_ids.json`.