# Databases e Tipos
Os módulos podem produzir tipos não padrões. Esses tipos especificos são definidos no diretório `crates/types`.

Uma database contém todas as saídas dos nós dentro de um ciclo. Para cara cycler existe uma database onde ele armazena suas saídas. Elas são representadas por uma struct de Rust. Se um nó precisa de uma entrada, a referência para o campo que esse dado ocupará na database é passada para o nó.   

Esses campos podem conter o tipo `Option` do Rust, regularmente é utilizado `Option::Some` para representar que o campo foi gerado e pode ser usado. Também há o `Option::None` que pode ser interpretado como um erro leve em que o nó de origem não conseguiu gerar o output neste ciclo. Isso pode acontercer por exemplo quando a projeção da câmera não é valida neste ciclo. Porém, databases também podem conter tipos padrões de Rust, por exemplo, se o nó produz um `f32` como saída. Mais informações sobre os tipos codificados em [Option](https://doc.rust-lang.org/std/option/) podem ser encontrados em [Tratamento de Erros](./error-handling.md) e [Macros](./macros.md).

**TODO**: Elaborar mais sobre.  
**TODO**: Explicar serialização e deserialização dos tipos.