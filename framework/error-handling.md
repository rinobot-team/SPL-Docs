# Tratamento de Erros
*Documentação detalhada ainda não fornecida pela HULKs*

## **TODO**: Elaborar
- Tratamento de Erros
    - 3 maneirs de tratar erros
        - Configurar uma saída principal para `none`: Acontece quando o nó não consegue gerar essa saída (por exemplo, quando as entradas não estão disponíveis ou houve um erro temporário dentro do nó)
            - Recuperável, esperado que seja resolvido no próximo ciclo
        - Retorno `Err(...)` de `cycle()`
            - Inrecuperável, mas a framework pode desligar graciosamente, esperado que não melhore nos próximos ciclos/no futuro
        - Panic no `panic!()` ou durante `unwrap()`
            - Inrecuperável, desligamento imediato, o kernel derrubará todo o processo, não há maneira de desligar graciosamente