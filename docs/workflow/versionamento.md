# Versionamento

Como o próprio nome sugere, o versionamento é o controle de versões de um software. Para isso, utilizamos git e github para hospedar.

## Git
Considerando que você já tenha feito o setup do git, criado uma conta no github e configurado o ssh, vamos ao que interessa.

### Convenções de versionamento do SPL:

- **Branches**:
    - `main`: branch principal, onde o código estável é mantido.
    - `dev`: branch de desenvolvimento, onde o código em desenvolvimento é mantido. É ideal que nela tenha código que pelo menos compile.
    - `dev-local`: branch de desenvolvimento local do desenvolvedor, onde o código que você está desenvolvendo no momento é mantido. Por favor, não faça push nela.
    - `release`: branch em que o código estável é mantido para ser lançado. As releases acontecem no inicio de cada campeonato e é o código com que competiremos, e que será avaliado pela RoboCup.

- **Nomenclatura de commits**:
    - `feat`: para novas features. Ex: `feat: Adiciona código para dar mortal para trás`.
    - `fix`: para correção de bugs. Ex: `fix: Corrige bug que fazia o robô tentar exterminar a humanidade`.
    - `refactor`: para refatoração de código. Ex: `refactor: Refatora código para renomear a função anda() para nao_anda()`.
    - `issue#`: para commits relacionados a issues. Ex: `issue#1: Adiciona código para dar mortal para trás`. 
    - `release`: para commits relacionados a releases. Ex: `release: Código para ganhar dos alemão`.

**Obs**: Sempre que for fazer um commit, tente fazer um commit atômico, ou seja, um commit para cada feature, bug ou refatoração. Isso facilita a leitura do histórico de commits.

## Fluxo de trabalho:
É de bom tom que sempre inicie um novo fluxo de trabalho com um `git pull` para garantir que você está trabalhando com a versão mais atualizada do código.

Caso esteja trabalhando em uma nova feature e precise criar diferentes branches locais, faça. Mas só de push na branch `dev` quando o código ao menos compilar.
Para criar uma nova branch local e mudar para ela:
```bash
    git switch -c dev-local-agora-vai-2
```
Depois que fizer as alterações necessárias, adicione, commit e mude para a branch `dev-local`:
```bash
    git add --all
    git commit -m "Funciona!!!"
    git switch dev-local
    git merge dev-local-agora-vai-2
```

Para dar push em um código em desenvolvimento, que compila, de merge na branch `dev` e dê push nela: 
```bash
    # Na branch dev-local
    git add --all
    git commit -m "feat: Adiciona código para ensinar o NAO a dirigir escavadeira"
    git switch dev
    git merge dev-local
    git push
```

Caso esta feature esteja pronta para ser lançada, faça o merge na branch `main` e dê push nela:
```bash
    # Na branch dev
    git add --all
    git commit -m "fix: Corrige bug que fazia o NAO dirigir escavadeira em direção ao prédio da Receita Federal"
    git switch main
    git merge dev
    git push
```

Caso esta feature esteja pronta para ser lançada, faça o merge na branch `release`, crie uma tag e dê push nela:
```bash
    # Na branch main
    git add --all
    git commit -m "release: Código para ganhar dos alemão"
    git switch release
    git merge main
    git tag -a v1.0 -m "Release Robocup24 1.0"
    git push
```
Depois disso, vá no github e [crie uma release](https://www.treinaweb.com.br/blog/criando-release-no-github) com a tag que você acabou de criar. 

## Conflitos
Caso você tenha conflitos ao dar merge em uma branch, o git vai te avisar. Para resolver, abra o arquivo que está com conflito e resolva manualmente. Depois, adicione, commit e dê push. Caso não consiga resolver, peça para o Gerente.