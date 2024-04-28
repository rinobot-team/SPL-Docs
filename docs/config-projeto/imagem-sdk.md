# Compilando a Imagem pro NAO e adiquirindo a SDK
**Por enquanto esse tutorial só é valido para o repositório HULKs, estamos modificando para que corresponda ao nosso contexto**  
**Essa seção ainda não está totalmente testada, erros podem ocorrer**  

Atualmente utilizamos o [Projeto Yocto](https://www.yoctoproject.org/) para criar uma imagem OPN flashável para o NAO, além de um SDK compatível para desenvolvimento direcionado ao robô. Esse SDK contém uma toolchain completa para cross-compilação e pode ser facilmente instalada nos computadores de desenvolvimento.

## Tamboerijn e Yocto SDK já existentes
Se você já possui o SDK do Yocto instalado, e nosso [reposiório do Tamboerijn](https://github.com/rinobot-team/Tamboerijn) clonado, é possível utilizar o `pepsi` de forma imediata:
    
```bash
    ./pepsi uploar <número/IP>
```
O [`pepsi`](../ferramental/pepsi.md) vai automaticamente baixar o SDK do GitHub e perguntar para você para instalá-lo durante a compilação.

## Criação da Imagem e SDK
O Projeto Yocto utiliza o [Bitbake](https://www.yoctoproject.org/docs/3.1/bitbake-user-manual/bitbake-user-manual.html) como engine de execução de tasks e oferece uma camada de abstração para a criação e modificação de imagens já existentes. Combinado com o [OpenEmbedded](https://www.openembedded.org/wiki/Main_Page), toda a árvore de criação é estruturada em camadas. Nos baseando no processo utilizado pela HULKs, temos as camadas meta-nao e meta-hulks.

### Configuração do Ambiente de Trabalho
Para a criação da imagem de SDK, certifique-se que você tem pelo menos 100 GB de espaço livre em seu disco rígido, não será ocupado permanentemente, mas durante o processo existem diversos arquivos temporários que podem ocupar esse espaço antes de serem excluídos. Comece clonando o repositório do Tamboerijn (se já não tiver feito) e criando um ambiente de trabalho para o Yocto:
```bash
    mkdir yocto
    cd yocto
```
Este diretório vai possui as camadas [meta-nao](https://github.com/HULKs/meta-nao) e meta-hulks, um script que roda comandos do BitBake e o repositório da HULKs [^1]. **Esse diretório não pode estar contido em um diretório que é repositório git**

```bash
    # Dentro do diretório yocto
    mv HULKsCodeRelease hulk
    cp -r nao/yocto/meta-hulks meta-hulks
    git clone git@github.com:HULKs/meta-nao
```