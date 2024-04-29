# Documentação
Este documento visa descrever como documentar corretamente o projeto e o workflow de desenvolvimento. Assim como um resumo de produção com Markdown e configurações do projeto e geração do site.

Qualquer dúvida, problema na configuração, correção ou sugestão de melhoria, entre em contato com o gerente da categoria ou abra uma issue no repositório.

## Markdown
Markdown é uma linguagem de marcação que permite a escrita de textos com formatação simples e rápida. É amplamente utilizada em documentações de projetos, READMEs, wikis, entre outros. A linguagem é simples e fácil de aprender, e permite a escrita de textos com formatação básica, como títulos, listas, links, imagens, tabelas, entre outros:

```markdown
# Titulo 1
## Titulo 2
### Titulo 3

negrito: **negrito**
itálico: *itálico*

linha de código: `código`
bloco de código:
    ```rust
    fn main() {
        println!("Hello, tamboerijn!");
    }
    ```
É possível escolher a linguagem para melhor formatação do bloco de código.

Lista não ordenada:
- item 1
- item 2
- item 3

Lista ordenada:
1. item 1
2. item 2
3. item 3

Link: [Colinha de markdown](https://www.markdownguide.org/cheat-sheet/)
```
Algumas convenções de formatação de texto são:    

- Utilizar somente um `# Titulo` por arquivo.  
- Adicionar dois espaços no final de uma linha para quebra de linha.  
- Utilizar `---` para separar seções, ou assuntos diferentes.  
- Comandos ou estruturas de diretórios devem ser escritos como `código`.  
- Separar texto de tópicos com uma linha em branco.  


Informações mais detalhadas sobre a linguagem Markdown podem ser encontradas no [Cheat-Sheet](https://www.markdownguide.org/cheat-sheet/).  

---

## Configuração

### Git
É ideal que os repositórios sejam clonados por SSH, já tendo a chave SSH configurada no GitHub, como [feito anteriomente](config-git.md). Isso facilita o processo de clonagem e push de arquivos.

```bash
    git clone git@github.com:rinobot-team/SPL-Docs.git
```
Caso haja algum problema de permissão entre em contato com o gerente da categoria ou diretor de projetos.

O repositório é dividido em 3 branchs principais:
- `main`: branch principal, onde o código estável é mantido.  
- `dev`: branch de desenvolvimento, onde as novas documentações são escritas e revisadas.  
- `gh-pages`: branch de produção, onde a documentação é publicada.  

O ideal é que cada desenvolvedor crie uma branch local para desenvolver suas documentações, e depois faça um pull request para a branch `dev`. Assim, as documentações podem ser revisadas e corrigidas antes de serem publicadas.

```bash
    git branch sua-branch
    # Depois de fazer as alterações e commits
    git switch dev
    git merge sua-branch
    git push
```
*Cuidado para não dar push na sua branch local*

Caso seu documento seja revisado e aprovado, ele será movido para a branch `main` e `gh-pages` para ser publicado.

```bash
    git switch main
    git merge dev
    git push
```
Merge entre dev e main é idealmente feita pelo gerente, mas pode ser feita por qualquer membro da equipe mediante aprovação.

```bash
    mkdocs gh-deploy
```

O comando acima é utilizado para publicar a documentação no GitHub Pages. Ele gera uma página estática com a documentação e a publica na branch `gh-pages` automaticamente. A página pode ser acessada em `https://rinobot-team.github.io/SPL-Docs/`.

### MkDocs
O MkDocs é uma ferramenta que permite a criação de documentações estáticas a partir de arquivos Markdown. Ele é utilizado para gerar a documentação do projeto e publicá-la no GitHub Pages. Como esse projeto é baseado em python, é necessário criar um [ambiente virtual](https://docs.python.org/pt-br/3/library/venv.html) para instalar as dependências.

```bash
    python3 -m venv venv
    source venv/bin/activate
    pip install mkdocs mkdocs-material
```

Caso o método anterior não funcione (se o tema não for encontrado ao utilizar `mkdocs serve`), você pode instalar o MkDocs e o MkDocs Material usando o `pip` diretamente, mas não é recomendado:

```bash
    pip install --break-system-packages mkdocs mkdocs-material
```
Com isso você já deve ter tudo necessário para documentar.

## Workflow 

### Desenvolvimento
Para gerar o site de forma concisa e customizada há um arquivo `mkdocs.yml` que contém as configurações do site. Nele é possível adicionar temas, plugins, e customizar a aparência do site. A sintaxe é simples e intuitiva, e a [documentação oficial do MkDocs](https://www.mkdocs.org/) pode te ajudar muito.

Além disso, utilizamos o tema [MkDocs Material](https://squidfunk.github.io/mkdocs-material/), que é um tema moderno e responsivo, com suporte a dark mode e muitas outras funcionalidades e plugins.

Para testar o site localmente, basta rodar o comando:

```bash
    mkdocs serve
```
e ele irá gerar um servidor local com o site, que pode ser acessado em `http://127.0.0.1:8000/`.

Erros de sintaxe ou localização de arquivo são mostrados no terminal, e o site é atualizado automaticamente a cada alteração feita nos arquivos Markdown ou estrutura do projeto.

É comum que ocorram erros em localização de imagens ou outros arquivos de asset, por isso optamos por mante-los no mesmo diretório que o arquivo Markdown que os utiliza.

**Além disso é importante adicionar novos arquivos na seção `nav` do arquivo `mkdocs.yml`, para que eles sejam exibidos no mapa do site.**

### Deploy
Para publicar a documentação no GitHub Pages, basta rodar o comando:

```bash
    mkdocs gh-deploy
```
É de boa prática que esse deploy seja feito após a revisão e aprovação da documentação e somente na branch `main`, para evitar erros e problemas de visualização.