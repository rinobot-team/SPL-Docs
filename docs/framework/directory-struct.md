# Estrutura de diretórios
*Importante: diretório = pasta*

O repostitório do programa é classificado como [monorepo](https://en.wikipedia.org/wiki/Monorepo), ou seja, todos os códigos fontes estão em um único repositório. A estrutura de diretórios é organizada de forma a facilitar a navegação e a compreensão do código. Aqui está uma visão geral da estrutura de diretórios:  

- `crates/`: Contém diverentes crates (módulos) que compõem o programa, além de outras crates para robótica e ferramental.  
    - `code_generation/`: Assim que o código fonte é analizado, essa crate vai gerar o código necessário para a execução dos cyclers e nós.  
    - `communication/`: O servidor de comunicação (para a framework) e client (para o ferramental de debug).   
    - `context_attribute/`: Contém o proc-macro `#[context]` usado nos nós para prepará-los para a execução.  
    - `framework/`: Blocos de construção para a framework de execução e alguns tipos necessários para o programa.  
    - `parameters/`: Funcionalidades para os parâmetros do programa (serialização e deserialização).  
    - `serialize_hierarchy/`: Traits necessários para todos os tipos da Comunicação.  
    - `serialize_hierarchy_derive/`: Macro de derivação para o trait `SerializeHierarchy`.  
- `etc/`: Todos os arquivos adicionais para o deploy do programa para o NAO.  
    - `parameters/`: Arquivos de parâmetros enviados para o NAO e lidos durante a execução.  
- `tools/`: Ferramentas para auxiliar no desenvolvimento e debug do programa.  
    - `pepsi/`: Ferramenta para compilar e comunicar com o NAO.  
    - `twix/`: Ferramenta para debugar o programa.  