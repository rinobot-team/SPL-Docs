# Configurações do NAO
Essa seção assume que você já tenha seguido todos os passos anteriores. Caso não tenha, siga a documentação na ordem correta.

## Flashando o firmware
Você pode fazer o flash do firmware tanto usando o `pepsi` quanto manualmente utilizando um pendrive.

Para fazer com o `pepsi` é necessário utilizar o subcomando `gammaray`, isto é descrito na seção de ferramental. 

### Preparando o pendrive
Primeiro, a imagem do firmware tem que ser copiada para o pendrive. Use o comando `lsblk` para verificar o nome do dispositivo.

Depois, utilize o seguinte comando para copiar a imagem para o pendrive, substitua `/path-to-image.opn` pelo caminho da imagem e `sdX` pelo dispositivo do pendrive:
```bash
    dd if=/path-to-image.opn of=/dev/sdX status=progress
```

Finalmente, rode o comando `sync` para garantir que todos os dados foram escritos no pendrive antes de removê-lo.
```bash
    sync
```

### Flashando a imagem no NAO
    - Certifique-se de que o NAO está desligado e ligado na tomada para prevenir que a bateria acabe durante o processo. Se a bateria acabar, o NAO pode se tornar um peso de papel de 70 mil reais.
    - Insira o pendrive no NAO na porta USB localizada na nuca do robô.
    - Segure o botão de ligar por 5 segundos até que a luz acenda, e solta imediatamente. Os LEDs do botão devem piscar rapidamente.
    - Espere o processo terminar. O NAO irá reiniciar automaticamente quando terminar.
    - As luzes das orelhas do NAO irão acender indicando uma barra de progresso.
    - Pode ser necessário reiniciar o NAO manualmente após o processo terminar. Caso ocorra, retire o pendrive antes de ligar o NAO.

## Compilando para o NAO
O comando para compilar o código para o NAO é o mesmo que para o simulador, apenas com a flag `--target nao` adicionada. O comando é:
```bash
    ./pepsi build --target nao
```
A compilação utiliza o perfil `incremental` do cargo por padrão.

## Upload para o robô
O upload pode ser feito com o número ou IPs do robô. O comando é:
```bash
    ./pepsi upload <numero/IP>
```
Ele irá compilar o código e enviar para o robô especificado automaticamente.

## Acesso remoto
É possível conectar ao shell do NAO via SSH utilizando o comando:
```bash
    ./pepsi shell <numero/IP>
```