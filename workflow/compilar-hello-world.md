
# Quero compilar um código qualquer em Rust pro NAO</p>
## Exemplo: quero rodar um _Hello World_ no robô, como fazer? 

# Introdução
Quando um código é compilado, usando, por exemplo, <strong><code>cargo build</code></strong>, é basicamente gerado um **  arquivo binário** específico para a arquitetura do seu computador. Ou seja, caso você compile no seu computador muito provavelmente o binário <strong>não </strong>vai rodar no robô. Da mesma forma, caso você compile no seu computador muito provavelmente não vai rodar num computador com arquitetura x86[^1].

## O que fazer então?
**~~Compila o código no robô mesmo~~**
**Cross Compiling.** Você compila o código em uma máquina para ser executado em outra (outro tipo de máquina/arquitetura)!  

### Mas como fazer isso?
No caso do sistema da HULKs, (o texto a seguir parte de muito achismo e interpretação do que os membros da equipe alemã explicaram em reunião com a gente)  
Ao usar o Yocto[^2] para gerar o sistema da HULKs (e posteriormente o nosso) é também gerado alguns binários para _“cross” _compilar o código para o NAO. Esses binários fazem parte da (cross)toolchain da HULKs.
Dessa forma, podemos simplesmente usar esses binários para compilar um código arbitrário pro robô!!
### Entendi, mas como fazer isso?
Como eu falei na toolchain da HULKs tem ferramentas para compilar o código específico pro robô (ao invés de compilar para arquitetura do seu próprio PC). 
(esse meio do caminho eu entendi direito ainda, mas..)
Basta usar o “cargo” (ferramenta de compilação do rust) que tem na toolchain!! 
Agora eu tenho dois _cargo_s no meu computador, um que compila Rust normalmente, outro que compila ESPECIFICAMENTE pro NAO. Então só usar esse segundo ao invés do primeiro quando eu for compilar pro robô!!
Para isso eles (HULKs) desenvolveram um **script** de _bash _que configura o ambiente para usar essa versão do cargo (e demais coisinhas necessárias para funcionar) quando você digitar **<code>cargo build</code>**.
#### Localização do Script:
    ~/.naosdk/7.1.0/environment-setup-corei7-64-aldebaran-linux
    # note que "~" é o mesmo que "/home/seu_nome"
    # note também que talvez o caminho mude, esse funciona para a versão 7.1.0, mas  muito provavelmente NÃO é seu caso 
Mas tem um porém, esse script altera várias variáveis de ambiente para fazer funcionar o código. Então vale entrar em outra sessão[^3] de **bash** antes para não _contaminar_ a atual.
#### Execução do Script:
Para rodar o script execute:
    bash
    source ~/.naosdk/7.1.0/environment-setup-corei7-64-aldebaran-linux 
    # troque o argumento ~/.naosdk/7.1.0/environment-setup-corei7-64-aldebaran-linux  pelo caminho desse arquivo em questão no seu computador!!

## TODO:
- [ ] E se eu não tiver nem essa pasta .naosdk? Instala o codigo da HULKs ./pepsi
- [ ] Para checar se funcionou 
- [ ] E compilar biblioteca?


<!-- Footnotes themselves at the bottom. -->
## Notes
[^1]:
     Veja mais [aqui](https://pt.wikipedia.org/wiki/X86)
[^2]:
     Yocto é um projeto que permite criar distribuições linux customizadas independente do hardware. Pensada justamente para sistemas embarcados com os mais diferentes hardwares. Mais informações [aqui](https://www.yoctoproject.org/)
[^3]:
      não é uma sessão, é mais uma instância
