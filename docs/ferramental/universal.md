# Universal  
Universal é um código em python que converte valores que estão em graus para radianos.  

## Uso  
Uma das propostas de melhoria da Rinobot-Jaguar para a Roboup 2024 é diminuir o tempo que os robôs da HULKS gastam para se levantar após uma queda. Uma das formas pensadas para essa melhoria, foi utilizar as mesmas sequência de valores usados no projeto Mari de 2018 da Rinobot. 

Porém, um dos problemas encontrados no processo dessa troca, foi o fato dos valores da Mari estarem em graus, e os valores da HULKS estarem em radianos.  

Portanto, foi desenvolvido este conversor para facilitar essa tarefa.  

## Funcionamento
O código é simples. 

1. Primeiro ele define o caminho para leitura e escrita dos arquivos.  
2. Depois ele  faz a leitura do arquivo linha por linha, selecionando e encontrando os valores em graus e convertendo-os para radianos.  
3. Ao final de cada linha, ele escreve os valores no arquivo de saída e após a leitura de todas as linhas ele finaliza o código.  

### Observação
O arquivo de coreografia possui algumas linhas de comentários, as quais são começadas por "#". O código reconhece essas linhas e apenas as-imprime sem nenhuma alteação.

*Este conversor está configurado para funcionar corretamente com os arquivos da Mari ou de arquivos com estrutura igual. Não existe nenhuma garantia de que ele funcionará corretamente em quaisquer outro tipo de arquivo.*


