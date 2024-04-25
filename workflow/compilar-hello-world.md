
# Quero compilar um código qualquer em Rust pro NAO</p>
***Exemplo: quero rodar um "Hello World" no robô, como fazer?***

# Introdução
Quando um código é compilado, usando, por exemplo, <strong><code>cargo build</code></strong>, é basicamente gerado um ** arquivo binário** específico para a arquitetura do seu computador. Ou seja, caso você compile no seu computador muito provavelmente o binário <strong>não </strong>vai rodar no robô. Da mesma forma, caso você compile no seu computador muito provavelmente não vai rodar num computador com arquitetura x86[^1].

# O que fazer então?
**~~Compila o código no robô mesmo~~**
**Cross Compiling.** Você compila o código em uma máquina para ser executado em outra (outro tipo de máquina/arquitetura)!  

## Mas como fazer isso?
No caso do sistema da HULKs, (o texto a seguir parte de muito achismo e interpretação do que os membros da equipe alemã explicaram em reunião com a gente)  
Ao usar o Yocto[^2] para gerar o sistema da HULKs (e posteriormente o nosso) é também gerado alguns binários para _“cross” _compilar o código para o NAO. Esses binários fazem parte da (cross)toolchain da HULKs.
Dessa forma, podemos simplesmente usar esses binários para compilar um código arbitrário pro robô!!
## Entendi, mas como fazer isso?
Como eu falei na toolchain da HULKs tem ferramentas para compilar o código específico pro robô (ao invés de compilar para arquitetura do seu próprio PC). 
(esse meio do caminho eu entendi direito ainda, mas..)
Basta usar o “cargo” (ferramenta de compilação do rust) que tem na toolchain!! 
Agora eu tenho dois _cargo_s no meu computador, um que compila Rust normalmente, outro que compila ESPECIFICAMENTE pro NAO. Então só usar esse segundo ao invés do primeiro quando eu for compilar pro robô!!
Para isso eles (HULKs) desenvolveram um **script** de _bash_ que configura o ambiente para usar essa versão do cargo (e demais coisinhas necessárias para funcionar) quando você digitar **`cargo build`**.
### Localização do Script:

    ~/.naosdk/7.1.0/environment-setup-corei7-64-aldebaran-linux
    # note que "~" é o mesmo que "/home/seu_nome"
    # note também que talvez o caminho mude, esse funciona para a versão 7.1.0, mas  muito provavelmente NÃO é seu caso 

#### **Não** achei a pasta `.naosdk` no meu computador
Instala o código da HULKs (usando o **pepsi**) no seu computador primeiro depois volta aqui.  
Obs: pra instalar basta dar um `./pepsi` na pasta do repositório da HULKs.

### Execução do Script:
Antes de executar, uma ressalva, esse script altera várias variáveis de ambiente para fazer funcionar o código. Então vale entrar em outra sessão[^3] de **bash** antes para não _"contaminar"_ a atual.
Para rodar o script execute:

    bash
    source ~/.naosdk/7.1.0/environment-setup-corei7-64-aldebaran-linux 
    # troque o argumento ~/.naosdk/7.1.0/environment-setup-corei7-64-aldebaran-linux  pelo caminho desse arquivo em questão no seu computador!!
# Como verificar se funcionou
O comando `type` mostra como um *comando* seria interpretado pelo shell se fosse chamado. De forma parecida o comando `which` procura no **PATH** do sistema por que programa é chamado ao digitar tal *comando*.
## Portanto para verificar se o script rodou, podemos:

    type cargo
    # ou which cargo
    # ou type rustc
# Como rodar um código então
Agora como o `cargo` (e demais programas) apontam para essa versão expecífica para compilar pro NAO, podemos simplesmente criar e compilar um projeto em Rust:

## Para compilar
    cargo new teste &&
    cd teste &&
    cargo build

## Para passar e rodar no robô
### Introdução
Existe varias maneiras de acessar outro computador remotamente. Uma delas é o **ssh** que nos permite de forma segura abrir um terminal como um usuário numa outra máquina. 
### Usando o ssh
Para usar o **ssh** de forma simples basta passar o usuário e o endereço de IP da máquina que você quer acessar (como se fosse aquele usuário). Exemplo:

    ssh nao@192.18.0.100
    # usuário: nao
    # endereço IP do robô: 192.168.0.100

Pronto você está acessando o NAO. Você pode fazer basicamente TUDO no robô a partir daí. Talvez desse menos trabalho se tivesse uma interface gráfica, mas poxa, muito chique, né?

### Como enviar/*receber* arquivos via ssh
O comando **scp** é a mistura de **ssh** com **cp**<sub>(copiar)</sub>. Ou seja, copia via ssh.

#### Para usar esse comando:
> scp usuário<b>@</b>máquina<b>:</b>local/de/destino

#### Exemplo:
    scp nome_do_arquivo nao@192.168.0.100:/home/nao
    # arquivo: nome_do_arquivo
    # usuário no robô: nao
    # IP do robô: 192.186.0.100
    # local de destino no robô: /home/nao (representado pelo "~" dentro do robô)
### Para passar o código compilado pro robô então:

    scp ~/teste/target/x86_64-aldebaran-linux-gnu/debug/teste
sendo "teste" o nome da pasta/projeto e "teste" por padrão então o nome do executável
note que os binários ficam na pasta target quando compilados via 'cargo'

---

## TODO:
- [X] E se eu não tiver nem essa pasta .naosdk? Instala o codigo da HULKs ./pepsi
- [X] Para checar se funcionou
- [X] Como compilar então?
- [ ] E compilar biblioteca?


<!-- Footnotes themselves at the bottom. -->
## Notes
[^1]:
     Veja mais [aqui](https://pt.wikipedia.org/wiki/X86)
[^2]:
     Yocto é um projeto que permite criar distribuições linux customizadas independente do hardware. Pensada justamente para sistemas embarcados com os mais diferentes hardwares. Mais informações [aqui](https://www.yoctoproject.org/)
[^3]:
      não é uma sessão, é mais uma instância
