# WiFi
O WiFi é fornecido por um chip Qualcomm Atheros AR9462, que é um chip de rede sem fio de banda dupla 802.11ac. O WiFi é configurado via [iNet Wireless Daemon (iwd)](https://iwd.wiki.kernel.org/).

## Configuração `iwd`
O serviço iwd pode ser configurado manualmente usando a ferramentos interface de comando `iwctl`. Para mudanças persistentes, o iwd guarda arquvivos `*psk` para cada SSID conhecido em `/var/lib/iwd/`. A distribuição Yocto  instala esses arquivos `*psk` para as redes de SSIDs de SPL_A a SPL_F.

```toml
    [Security]
    Passphrase=Nao1237

    [Settings]
    AutoConnect=false
```

Conexão automatica está desativada para prevenir que o NAO conecte à rede do SPL sem necessidade, o que pode causar problemas durante partidas de outras equipes. Se o iwd foi designado para conectar na rede uma vez, ele tenta reconectar para o mesmo SSID até que o daemon seja instruído a desconectar.

O iwd também pode ser usado para configurar o IP e rodar DHCP. Esta opção é chamada `Network Configuration` e desabilitada via `/etc/iwd/main.conf`. Configuração de IP é feita pelo [systemd-networkd](https://www.freedesktop.org/software/systemd/man/systemd.network.html).

```toml
    [General]
    EnableNetworkConfiguration=false
```

## Configuração de IP
O IP do NAO é derivado do número de ID dos robôs como setado em `../config-projeto/imagem-sdk.md`. Para IPv6 ele segue o padrão `10.{interface}.{time}.{nao}`. Por exemplo, para o robô 13 da equipe Rinobot-Jaguar, o IPv6 para a interface wireless é `10.0.47.13` e para cabeada `10.1.47.13`.   