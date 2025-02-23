# Serialização e Deserialização de Caminhos

A crate `path_serde` introduz interfaces para serialização e deserialização de partes específicas de tipos através de caminhos para seus campos internos. Essa funcionalidade é usada principalmente pela [Comunicação](./communication.md) para serializar dados e fornecê-los para nossas ferramentas de debugging.

## Traits

A crate fornece 3 traits diferentes: `PathSerialize`, `PathDeserialize` e `PathIntrospect`.

### `PathSerialize`

Esse trait permite a serialização de partes de tipos através da confirmação de caminhos para o dado interno desejado. Isso é útil quando somente uma parte do tipo precisa ser serializada.

```rust
trait PathSerialize {
    fn serialize_path<S>(&self, path: &str, serializer: S) -> Result<S::Ok, Error<S::Error>>
    where
        S: Serializer;
}
```

Por exemplo, um usuário que está interessado somente na posição angular da flexão de uma junta de tornozelo pode utilizar o  caminho `Control.main.sensor_data.positions.ankle_pitch` para acessar somente este valor serializado.

### `PathDeserialize`

De forma inversa, o trait `PathDeserialize` facilida a deserialização de dados de um caminho definido.

```rust
trait PathDeserialize {
    fn deserialize_path<'de, D>(
        &mut self,
        path: &str,
        deserializer: D,
    ) -> Result<(), Error<D::Error>>
    where
        D: Deserializer<'de>;
}
```

Esse funcionalidade é utilizada quando mudamos somente partes de um parâmetro.

### `PathInstrospect`

Esse trait permite instrospecção de tipos, ou seja, permite o usuário gerar um conjunto de caminhos possíveis para os campos de um tipo. Essa funcionalidade é importante para explorar dinamicamente a estrutura de tipos de dados. Por exemplo, o ferremental as vezes utiliza para autocompletar caminhos quando se inscrevendo nos dados do robô.

## Macro

A `path_serde` também provê macros de `derive`, gerando a implementação destes traits automaticamente. O código fonte de um tipo anotado é analizado e a implementação é gerada para cada campo, delegando a chamada para os subtipos.

### Atributos

Tipos e campos podem ser anotados mais a fundo para modificar a geração de código. Cada atributo é prefixado com `#[path_serde(<...>)]` para identificá-los.
São definidos os seguintes atributos:

#### Container: `bound`

Is atribulo é atrelado ao tipo do container e define limites (bounds) genéricos para a implementação.

```rust
#[derive(Serialize, PathSerialize)]
#[path_serde(bound = T: PathSerialize + Serialize)]
struct MyStruct<T> {
    foo: T,
}
```

#### Container: `add_leaf`

Esse atributo adiciona uma folha adicional para os filhos de um tipo especificando o nome e o tipo da folha. Este tipo é necessário para implementar o método `TryFrom<Self>` que gera dados para o campo quando requisitado.

> Claramente, `container` é representado como uma árvore, por isso a presença de folhas.

```rust
#[derive(Serialize, PathSerialize)]
#[path_serde(add_leaf(bar: MyIntoType)]
struct MyStruct {
    foo: i32,
}
```

#### Campo: `leaf`

Este atributo se trata da folha de uma árvore. Isto é, não se espera que ela tenha filhos, então a delegação de um path acaba nele.

```rust
#[derive(Serialize, PathSerialize)]
pub struct MultivariateNormalDistribution {
    pub mean: f32,
    #[path_serde(leaf)]
    pub covariance: FancyType,
}
```

#### Campo: `skip`

Este atributo especifica que o campo marcado deve ser pulado na implementação. Ele não sera incluido na (de)serialização ou na instrospecção.

```rust
#[derive(Serialize, PathSerialize)]
pub struct MultivariateNormalDistribution {
    pub mean: f32,
    #[path_serde(skip)]
    pub covariance: FancyType,
}
```

## Exemplo de Uso

```rust
#[derive(PathSerialize, PathDeserialize, PathIntrospect)]
struct ExampleStruct {
    foo: u32,
    bar: String,
}

fn main() {
    let example = ExampleStruct {
        foo: 42,
        bar: String::from("example"),
    };

    let serialized_data = example.serialize_path("foo", /* serializer */); // (1)

    let deserialized_data = example.deserialize_path("bar", /* deserializer */); // (2)

    let available_paths = ExampleStruct::get_fields(); // (3)
}
```

1. Serializa os dados usando path.
2. Deserializar os dados a partir de um path especifico.
3. Gera um conjunto com todos os paths disponiveis dentro de `ExampleStruct`.
