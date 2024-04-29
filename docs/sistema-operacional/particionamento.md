# Particionamento
O Nao usa um único dispositivo de armazenamento flash para fins de armazenamento principal. Depois de fazer o flash corretamente com o HULKsOS, esse dispositivo fica reconhecido como `/dev/mmcblk1`. O dispositivo conta com 4 partições diferentes:
```bash
NOME           TAMANHO     PONTO DE MONTAGEM
mmcblk1p1       29.1G
|- mmcblk1p1    128M      /media/internal
|- mmcblk1p2    64M       
|- mmcblk1p3    3.8G      /
|- mmcblk1p4    25.1G     /data
```

## `mmcblk1p1`: Softbank Partition
A primeira partição é chamada de 'internal' e é usada pelos binários da Softbank durante o flash. A partição é montada por padrão em `/media/internal` e é necessária para interface com o LoLA ou HAL. Durante utilização normal, essa partição não é acessada pelos binários do Tamboerijn.

A Softbank utiliza essa partição para armazenar informações gerais, como IDs. Por exemplo, o script `/opt/aldebaran/head_id` usa o arquivo `/media/internal/DeviceHeadInternalGeode.xml` para recolher o ID da cabeça.

## `mmcblk1p2`: EFI Partition
A segunda partição é a de [EFI boot](https://canaltech.com.br/hardware/o-que-e-uefi/), e não é montada por padrão. Para acessar ela, e os arquivos de EFI, monte a partição:
```bash
    # No shell do NAO
    sudo su
    mount /dev/mmblk1p2 /mnt/
    # Arquivos de EFI estão em /mnt/
```

## `mmcblk1p3`: Root Partition
A terceira partição é o root. Ela é criada e administrada pela configuração do Yocto e não deve ser alterada em runtime. Para modificar a partição, é necessário fazer as alterações necessárias durante o processo de criação da imagem e depois reflashar o sistema.

## `mmcblk1p4`: Data Partition
A quarta partição é para armazenagem e dados do runtime. Ela é montada em `/data` por padrão.

Quando iniciando o sistema pela primeira vez, as duas unidades de sistema `data-format` e `data-skeleton` são responsáveis por configurar a partição e estrutura de diretórios. A unidade `data-format` roda uma vez antes de montar a partição para criar a árvore de diretórios, e depois se desabilita. A unidade `data-skeleton` roda toda vez que o sistema é iniciado e garante o funcionamento da estrutura de diretórios para outras montagens.