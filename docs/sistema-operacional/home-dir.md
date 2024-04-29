# Diretório Home
O diretório Home (`/home/nao`) é sobreposto com uma montagem na partição `/data`.

Quando flashando para o robô e atualizando o binário do HULK, a estrutura do diretório home tem esta aparência:

```bash
.
|-- hulk
|   |-- bin
|   |   `-- hulk
|   |-- etc
|   |   |-- parameters
|   |   |   `-- *.json
|   |   |-- motions
|   |   |   `-- *.motion2
|   |   |-- neural_networks
|   |   |   `-- *.hdf5
|   |   |-- poses
|   |   |   `-- *.pose
|   |   `-- sounds
|   |       `-- *.ogg
|   `-- logs
|       |-- hulk-1667906932.err
|       |-- hulk-1667906932.out
|       |-- hulk.err -> /home/nao/hulk/logs/hulk-1667906932.err
|       `-- hulk.out -> /home/nao/hulk/logs/hulk-1667906932.out
`-- robocup.conf
```

O arquivo `./robocup.conf` é necessário para iniciar o serviço do LoLA no modo Robocupper. Todos os arquivos relacionados com o serviço e binários HULK são armazenados no subdiretório `hulk` e espelha a estrutura de [arquivos de desenvolvimento](../framework/directory-struct.md).