# Parâmetros
O software de controle robótico tem algums parâmetros de configuração que afetam cálculos e a execução do código.

## Carregando e Modificações
Os parâmetros são carregados pelo sistema de arquivos. Esses arquivos estão localizados em `etc/parameters/`. Este diretório também é mandado por deploy para o NAO. A [Comunicação](./communication.md) é capaz de modificar valores dos parâmetros em tempo de execução (mas não consegue aplicar essa modificações no arquivo propriamente dito).

## Sobrescrevendo
O diretório dos parâmetros permite sobrescrever individualmente campos de configurações na hierarquia dos objetos. Os NAOs tem as câmeras e a placa principal na cabeça, e a placa do peito e atuadores espalhados pelo corpo. Cada cabeça e corpo do NAO possui um ID único que permite carregar parâmetros específicos. Além disso, robôs podem precisar de diferentes parâmetros dependendo da localização, essa localização é selecionada no diretório `etc/parameters/` e aponta para um diretório em que os arquivos de parâmetros sobrescritos são colocados. Para criar um objeto de parâmetros completo é necessário utilizar este procedimento:  

1. Ler e processar (parsing) `etc/configuration/default.json`  
2. Se existir, ler e processar...  
    - Para o NAO: `etc/nao_location/default.json`  
    - Para Webots: `etc/webots_location/default.json`  
    - Para o simulador de comportamento: `etc/simulated_location/default.json`  
3. Se existir, ler e processar `etc/body.{body_id}.json`  
4. Se existir, ler e processar `etc/head.{body_id}.json`  
5. Se existir, ler e processar...  
    - Para o NAO: `etc/nao_location/body.{body_id}.json`  
    - Para Webots: `etc/webots_location/body.{body_id}.json`  
    - Para o simulador de comportamento: `etc/simulated_location/body.{body_id}.json`  
6. Se existir, ler e processar...  
    - Para o NAO: `etc/nao_location/head.{body_id}.json`  
    - Para Webots: `etc/webots_location/head.{body_id}.json`  
    - Para o simulador de comportamento: `etc/simulated_location/head.{body_id}.json`  

Os locais dos diretórios geralmente são [symlinks](https://www.howtogeek.com/16226/complete-guide-to-symbolic-links-symlinks-on-windows-or-linux/) para os diretórios verdadeiros. Isso permite mudar locais facilmente redirecionando os symlinks. 