# Pepsi
Pepsi é uma multiferramenta utilizada pra tudo que está no contexto do código para o NAO. Ela pode ser usada para compilar, modificar parâmetros, fazer deploy no robô, abrir um shell remoto com os jogadores, entre outras coisas.

Essa página é um resumo das funcionalidades do `pepsi`. Para instruções detalhadas utilize o comando `pepsi --help` ou `pepsi <subcomando> --help`.

## Webots workflow
É bem tranquilo. Abra o Webots, carregue o arquivo de mundo `webots/worlds/penalized_extern.wbt` e execute:
    
```bash
    ./pepsi run
```
Ele irá compilar (se necessário) e repois rodar o binário do webots. A simulação é pausada automaticamente até que o binário inicie.

## NAO workflow
```bash
    ./pepsi uplodad <número / IP>
```
Esse comando faz o seguinte:
- Checa se a toolchain está instalado, faz download e instala se for necessário
- Compila o código para o target NAO (`pepsi build --target nao`)
- Faz upload do binário, parâmetros de configuração, arquivo de motion, redes neurais, etc. para o robô (deploy)
- Reinicia o serviço HULKs nos NAOs

## Interação com os NAOs
NAOs podem ser identificados tanto por IP quanto por número, números são convertidos por IP assim:
- `{número}` -> `10.1.24.{número}`
- `{número}w` -> `10.0.24.{número}`
Muitos subcomandos podem agir em múltiplos NAOs concorrentemente.

`upload` compila o código e faz o deploy de todos os arquivos binários e de configuração para um ou mais robôs.

`wireless`, `reboot`, `poweroff` e `hulk` interagem diretamente com o robô. Enquanto `communication` e `playernumber` só alteram parâmetros de configuração locais.

`pregame` combina desativação das comunicações (para evitar a emissão de mensagens ilegais), atribuição dos números dos jogadores, configuração das redes de wifi, upload e reinicialização do *HULK service*.

`logs` e/ou `postgame` podem ser usados no pós-jogo para coletar logs e reiniciar o serviço HULK.

`gammaray` é utilizado para fazer o flash da imagem do HULKs-OS para um ou mais robôs.

## Opções de build
Para subcomandos que compilam binários, é possível especificar o alvo e perfil de build. Incluindo `build`, `run`, `check` e `clippy`.

## Aliveness
Utilizando o subcomando `aliveness`, a pepsi pode recolher informações dos NAOs conectados via ethernet. Por padrão, somente informações irregulares como sistemas inativos, versões desatualizadas do HULKs-OS e nivel de carga de bateria abaixo de 95% são informados. Usando as flags `-v` / `--verbose` ou `-j` / `--json` é possível obter informações mais detalhadas em formato legível para humanos ou computadores.

Também é possível configurar timeouts via `t` / `--timeout` (padrão de 200ms) e especificar endereços (número ou IPs) para verificar robõs específicos.

Para mais informações, utilize leia esse [documento](./aliveness.md).

## Shell Completion
Shell completions são configurações do seu ambiente de shell para que ele complete comandos automaticamente. Para habilitar o shell completion para o `pepsi`, execute o seguinte comando:

Exemplo para zshell
```bash
    ./pepsi completions zsh > _pepsi 
```
Consulte a documentação do seu ambiente de shell caso use outro (fish, tmux, etc).

Para incluir sugestões dinânmicas é necessário adicionar ao *PATH*, para isso utilize este comando:   
```bash
    cargo install --path tools/pepsi
``` 
e adicionar `~/.cargo/bin` ao *PATH*.