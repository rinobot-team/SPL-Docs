# Filtragem
*Documentação detalhada ainda não fornecida pela HULKs*

## **TODO**: Elaborar

- FutureQueue/Filtragem
    - Resumo: Diagrama de tempo
    - Motivação: Filtros precisam de atualizações monótonas
        - O que o nó de filtragem tem de fazer em cada ciclo?
            - Reverter medições temporárias do último ciclo
            - Aplicar medições persistentes
            - Aplicar medições temporárias
    - FutureQueue (cada Cycler de Percepção tem um para comunicar com Controle)
        - Produtor
            - anunciar
            - finalizar
        - Consumidor
            - consumir
    - PersistentDatabases consomem de múltiplas FutureQueues e reorganizam dados
        - persistente vs. temporário
    - PersistentInputs (Interface para os nós de filtro)
        - persistente vs. temporário