# Configurando o Git

Caso você não tenha configurado seu Git ainda.

## Configurando Username e Email

A primeira coisa que você deve fazer ao instalar o Git é configurar seu nome de usuário e email. Isso é importante para que o Git saiba quem está fazendo as alterações no código ao gerar o commit.

```bash
git config --global user.name "Seu Nome"
git config --global user.email "Seu Email"
```
Ideal que utilize seu email institucional ou o fornecido à Rinobot.

## Criando e adicionando a SSH-Key
Desde 2023, o Github não aceita mais autenticação por senha, então é necessário criar uma chave SSH (Secure Shell Protocol) para autenticação. Você pode seguir o tutorial oficial do Github [aqui](https://docs.github.com/pt/github/authenticating-to-github/connecting-to-github-with-ssh). Caso já tenha essa parte configurada, você pode pular para o próximo passo. Qualquer dúvida, entre em contato com a equipe.