---
layout: post
title: "Como agendar posts no Jekyll usando a AWS"
date: 2019-03-06 00:48:25
image: 'https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551836833/mission_impossible_djiusi.jpg'
description: Se você sentiu falta da funcionalidade de agendar postagens em sites estáticos, esse artigo resolve o seu problema.
category: 'tutorial'
tags:
- jekyll
- aws
twitter_text: É possível! Nesse artigo ensino a fazer agendamento de postagens passo a passo.
introduction: Se você sentiu falta da funcionalidade de agendar postagens em sites estáticos, esse artigo resolve o seu problema.
---

Quando resolvi passar meu blog do [Wordpress para o Jekyll](https://www.rossener.com/novo-layout-blogando-como-um-dev-com-jekyll/) encontrei vantagens que só quem usa um gerador de website estático poderia entender.

De repente tudo ficou mais fácil e divertido de fazer, escrevia artigos com markdown, editava o tema do jeito que eu queria sem sofrimento, tinha uma página com um carregamento super rápido e por aí vai.

Foram dias felizes... até eu querer agendar uma postagem.

<img src="https://media.giphy.com/media/3o72FiAgLm34QKLSnK/giphy.gif" width="480" alt="Raivoso">
*Por que diabos é tão difícil?*

Com uma pesquisa rápida é possível encontrar algumas soluções, e a que vamos tratar aqui é o [Post Scheduler](https://serverless.com/blog/static-site-post-scheduler/).

É um tanto chato configurar tudo o que ele fala pra configurar nesse artigo, mas eis aqui a boa notícia, **funciona**.

Então, se é isso mesmo que você quer, dá uma aquecida nessa cadeira e vamos pra luta!

<img src="https://media.giphy.com/media/ksEIrSh3BVMnoWxmF9/giphy.gif" width="480" alt="Se aquecendo">

> **Nota:** No momento em que escrevo isso, acabo de descobrir que existe uma outra maneira usando o Travis CI, mas esse vai ficar pra um próximo artigo 😄

## Antes de começarmos

O que nós vamos fazer aqui é seguir a [documentação do Post Scheduler](https://github.com/serverless/post-scheduler/) passo a passo.

Ele funciona da seguinte maneira:

1. Um webhook do GitHub é disparado quando um *pull request* (contendo o commit com o novo post) é atualizado.

2. Se o comentário do *pull request* contém uma string com o formato `schedule(MM/DD/YYYY H:MM pm)` e a pessoa que comentou é um colaborador do projeto, o post é agendado.

3. Um cron job serverless roda de hora em hora na AWS para verificar se algum post está pronto para ser publicado.

4. Quando o post está pronto para ser publicado, a função automaticamente faz o *merge* do *pull request* na `master`, fazendo o deploy do site.

5. A branch do post agendado é automaticamente deletada.

Um pouco complexo, mas vale a pena. Prontos?

## Partiu!

### 1. Configure o Serverless + AWS

Para seguirmos, a primeira coisa que você deve fazer, caso não tenha esses caras configurados, é seguir [esse tutorial aqui](https://www.rossener.com/como-configurar-serverless-com-aws/).

### 2. Instale o Serverless localmente

```
$ npm install serverless -g
```

### 3. Clone o repositório do projeto

```
$ git clone https://github.com/serverless/post-scheduler.git
```

### 4. Crie um token de acesso pessoal no GitHub

**4.1.** Acesse as configurações no seu perfil do GitHub.

<img src="https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551896983/token-1_hgti0c.png" width="200" alt="Settings">

**4.2.** Vá em *Developer Settings*

<img src="https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551896983/token-2_v0vdu5.png" width="200" alt="Developer settings">

**4.3.** No menu a esquerda vá em *Personal access tokens*

![Personal access tokens](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551896983/token-3_sggu7z.png)

**4.4.** Clique em *Generate new token*

![Generate new token](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551896984/token-4_bmesbn.png)

**4.5.** Digite uma descrição qualquer para o token e selecione o escopo *repo*

![Preencher as informações](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551896984/token-5_y1m6fs.png)

**4.6.** Copie o token gerado para utilizarmos na configuração em seguir.

![Copiar o token gerado](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551896983/token-6_m5m9oo.png)

### 5. Altere as configurações

De volta ao projeto clonado, renomeie o arquivo `config.prod.example.json` para `config.prod.json` e altere os dados:

```
// config.prod.json
{
  "serviceName": "blog-scheduler",
  "region": "sa-east-1",
  "TIMEZONE": "America/Sao_Paulo",
  "CRON": "cron(0 * * * ? *)",
  "GITHUB_REPO": "serverless/blog",
  "GITHUB_WEBHOOK_SECRET": "YOUR_GITHUB_WEBHOOK_SECRET_HERE",
  "GITHUB_API_TOKEN": "YOUR_GITHUB_API_TOKEN_HERE",
  "GITHUB_USERNAME": "YOUR_GITHUB_USERNAME_HERE"
}
```

- `serviceName`: nome do serviço que vai aparecer na sua conta AWS
- `region`: região da AWS para fazer *deploy* das funções e do banco de dados. Note que eu alterei para **sa-east-1** (que é no Brasil)
- `TIMEZONE`: A timezone em que o cron irá rodar. Veja no arquivo `timezones.json` as timezones que estão disponíveis (no meu caso alterei para **America/Sao_Paulo**).
- `CRON`: Com que frequência você quer que ele verifique se há posts agendados? Por padrão, ele roda de hora em hora.
- `GITHUB_REPO`: O *proprietario-do-repositório/nome-do-repositório* do seu projeto.
- `GITHUB_WEBHOOK_SECRET`: Qualquer string que você quiser. Isso será usado nas suas configurações do *webhook*. Para gerar uma string aleatória você pode acessar [este site](http://www.unit-conversion.info/texttools/random-string-generator/).
- `GITHUB_API_TOKEN`: Token de acesso pessoal criado no passo anterior.
- `GITHUB_USERNAME`: Seu nome de usuário do GitHub.

### 6. Faça o deploy e copie a url de retorno

```
$ serverless deploy
```

Copie a url retornada em *POST* para configurarmos o webhook:

![Webhook URL](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551902259/webhook-url_fvfyqg.png)

### 7. Configure o webhook

**7.1.** No seu repositório, vá em *Settings*

![Repo Settings](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551903046/webhook-1_rpyhlw.png)

**7.2.** No menu a esquerda, clique em *Webhooks*

![Webhooks](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551903047/webhook-2_equfde.png)

**7.3.** Na tela seguinte, clique em *Add webhook*

![Add webhook](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551903047/webhook-3_aqgm0n.png)

**7.4.** Crie o webhook

- Em *Payload URL* cole a url retornada no *POST* do passo anterior.
- Em *Content type* altere para **application/json**
- Em *Secret* coloque a string que você adicionou em `GITHUB_WEBHOOK_SECRET` no arquivo `config.prod.json`
- Nos eventos que irão disparar o webhook, selecione *Let me select individual elements* e depois selecione somente *Issue comments*

Por fim, clique em *Add webhook*

![Criar o webhook](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551903046/webhook-4_o1owbt.png)

### 8. Testando se funciona

**8.1.** No terminal do seu projeto com o Jekyll, crie uma nova branch com qualquer nome, eu chamei a minha de *readme*.

```
$ git checkout -b readme
```

**8.2.** Faça alguma modificação no projeto e envie o commit. No meu caso, criei um arquivo `README.md`

```
$ git add .
$ git commit -m "Added README file"
$ git push -u origin readme
```

**8.3.** No seu repositório, clique em *Pull requests*

![Pull requests](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551904175/pr-1_fkgr3a.png)

**8.4.** Na tela seguinte, clique em *New pull request*

![New pull request](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551904175/pr-2_ufcevz.png)

**8.5.** Selecione a branch que você criou e clique em *Create pull request*

![Create pull request](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551904176/pr-3_mvs5g8.png)

**8.6.** Agora é o momento decisivo, insira um comentário com a data e hora que você gostaria de agendar o post, usando o formato `schedule(MM/DD/YYYY H:MM pm)`

![Comment](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551904176/pr-4_bbwvct.png)

**8.7.** Após alguns segundos, se um novo comentário como esse aparecer, você conseguiu! Parabéns, agora você pode agendar postagens com o Jekyll 👏🎉

![Sucesso](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551904176/pr-5_hffgoz.png)

> **Nota 1**: Se nada acontecer, pode ser que o webhook não tenha sido disparado (já aconteceu comigo). Tente inserir um novo comentário com uma data novamente e ele deve agendar.

> **Nota 2**: Para cancelar o agendamento de um post basta deletar o comentário retornado pelo serviço com os dados do agendamento.

Finalmente, depois dessa saga, você consegue fazer agendamentos, é só seguir o passo #8 sempre que desejar.

Espero que tenha gostado e tenha conseguido atingir o seu objetivo. Até mais!

> A melhor maneira de ser feliz é contribuir para a felicidade dos outros. **- Confúcio**


