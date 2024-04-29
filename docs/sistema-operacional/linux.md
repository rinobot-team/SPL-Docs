# Linux
O NAO utiliza o kernel do Linux versão 5.4 para [processadores Intel](https://github.com/intel/linux-intel-lts/tree/5.4/preempt-rt).

A maior parte da configuração do kernel é feitar pela camada `meta-intel` do Yocto, modificações específicas para o NAO são feitas pela camada `meta-nao` e consistem de atualizações e módulos para o kernel para comunicação com a placa do peito.