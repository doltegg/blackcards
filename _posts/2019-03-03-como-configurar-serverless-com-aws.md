---
layout: post
title: "Como configurar Serverless com AWS Lambda"
date: 2019-03-03 00:12:05
image: 'https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551651632/serverless-aws-lambda_dsjjwy.png'
description: Um tutorial mastigado de como deixar seu ambiente pronto para usar o AWS Lambda com a toolkit Serverless.
category: 'tutorial'
tags:
- serverless
- aws
twitter_text: Um tutorial mastigado de como deixar seu ambiente pronto para usar o AWS Lambda com a toolkit Serverless.
introduction: Um jeito simples de entender e configurar seu ambiente para trabalhar com serverless e AWS.
---

Fala pessoal! Tudo bem?

A intenção deste artigo é ajudar vocês a configurarem o [Serverless Framework](https://serverless.com/) com o [AWS Lambda](https://aws.amazon.com/lambda/) e utilizarmos isso para agendarmos postagens no Jekyll num próximo artigo.

Para isso, vou dividir esse post em três tópicos:

- [O que é serverless?](#o-que-e-serverless)
- [O que é AWS?](#o-que-e-aws)
- [Passo a passo](#passo-a-passo)

## <a name="o-que-e-serverless"></a>O que é serverless?

Serverless é uma arquitetura de sistema onde, como o próprio nome diz, "não há servidor".

Em outras palavras, não precisamos gerenciar uma máquina e sermos responsáveis por sua manutenção, segurança e/ou custos decorrentes do tempo que os seus recursos não estão sendo utilizados.

Nesse modelo, o provedor na nuvem é o responsável por executar pedaços de códigos com recursos alocados dinamicamente, cobrando apenas pelos recursos realmente usados na execução.

Geralmente, o código é executado em *containers stateless*, ou seja, esses containers não armazenam estado, sendo que sua função será executada e após a requisição ser concluída, o container em que ela estava sendo hospedada será apagado.

Esses containers podem ser ativados de diversos modos, como por exemplo:

- Requisições HTTP
- Eventos de banco de dados
- Serviços de filas
- Alertas de monitoramento
- Upload de arquivos
- Eventos agendados

E muitos outros.

Pelo fato do código enviado ao provedor ser escrito em forma de funções, muitas vezes a arquitetura serverless é referenciada como *FaaS* (*"Functions as a Service"*, ou Funções como Serviço).

Entre os maiores provedores de *FaaS* do mercado atualmente, temos:

- AWS, com o [AWS Lambda](https://aws.amazon.com/lambda/)
- Microsoft Azure, com o [Azure Functions](https://azure.microsoft.com/en-us/services/functions/)
- Goodle Cloud, com o [Cloud Functions](https://cloud.google.com/functions/)

Bom, eu resumi do jeito mais simples possível, mas se quiser mais informações sobre essa arquitetura, recomendo este link [aqui](https://serverless-stack.com/chapters/pt/what-is-serverless.html).

Neste tutorial, iremos configurar o [Serverless Framework](https://serverless.com/) (um conjunto de ferramentas gratuito para construirmos aplicações serverless) para trabalhar com o AWS Lambda da Amazon.

## <a name="o-que-e-aws"></a>O que é AWS?

Segundo a Wikipédia:

> AWS, ou *Amazon Web Services*, é uma plataforma de serviços de computação em nuvem oferecida pela Amazon.com

Simples, não é? Pois bem, o AWS Lambda, que iremos configurar a seguir, é o serviço de serverless oferecido pela plataforma.

Se ficou curioso por informações mais detalhadas sobre o serviço, recomendo este [link](https://serverless-stack.com/chapters/pt/what-is-aws-lambda.html).

## <a name="passo-a-passo"></a>Passo a passo

Antes de começarmos, saiba que o passo a passo que fiz é baseado na [documentação oficial do Serverless Framework](https://serverless.com/framework/docs/providers/aws/guide/iam/).

Outra coisa importante é que você irá precisar de um **cartão de crédito** para criar uma conta na AWS, mas relaxa, **nada será cobrado**, pois o AWS Lambda faz parte do *Free Tier* oferecido pela Amazon. 🙃

Então vamos lá:

### 1. Crie uma conta na AWS

Acesse este [link](https://portal.aws.amazon.com/billing/signup#/start), preencha o formulário, e quando for pedido para escolher o plano, selecione o **Basic Plan** (o gratuito).

> **Nota:** Você vai precisar de um cartão de crédito, mesmo no plano gratuito, infelizmente não tem como fugir disso.

![Plano da AWS](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551639711/aws-sign-in_uggxtr.png)

### 2. Crie um usuário e atribua as permissões

**2.1.** Com o painel da AWS aberto, clique em *Serviços*, no canto superior esquerdo:

![Serviços](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551639712/services_am0tu0.png)

**2.2.** No menu, vá para *Security, Identity, & Compliance > IAM*

![IAM](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551639713/iam_viq3yv.png)

**2.3.** Na tela seguinte, clique no botão *Add User*

![Adicionar usuário](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551639710/add-user-1_bf6wod.png)

**2.4.** Crie um *username* para identificar o usuário que irá usar o serviço e marque o tipo de acesso para *Programmatic access*. Depois clique em *Next*

![Username](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551639711/add-user-2_qo9f72.png)

**2.5.** Na tela seguinte você vai precisar criar uma política de permissões para este usuário.

Clique em *Attach existing policies directly*, depois em *Create policy*.

![Adicionar política](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551639710/add-user-3_ezzemr.png)

**2.6.** Na janela que se abrir, clique na aba *JSON*, copie e cole o conteúdo [desse gist](https://gist.github.com/ServerlessBot/7618156b8671840a539f405dea2704c8).

![Criar política](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551639711/add-user-4_lqej8z.png)

**2.7.** Revise os dados. Aqui você pode dar um nome pra essa política de permissões e clicar em *Create Policy*.

![Revisar política](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551639710/add-user-5_is7qmp.png)

**2.8.** Volte a tela para adicionar usuário, procure pela política de permissões que você acabou de criar, selecione e clique em *Next*

![Selecionar política](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551639715/add-user-6_uimxbs.png)

**2.9.** Aqui pode deixar tudo em branco e clicar em *Next*

![Adicionar tags](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551639712/add-user-7_k9ktnb.png)

**2.10.** Revise tudo e clique em *Create user*

![Criar usuário](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551639711/add-user-8_zfuosm.png)

**2.11.** Finalmente, copie o *Access key ID* e a *Secret access key* para algum lugar temporário.

> **Nota:** Você não conseguirá voltar para ver essa tela novamente, por isso tenha certeza de que copiou os dados corretamente. Caso os perca, deverá criar um novo usuário mais uma vez.

![Keys](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1551639713/add-user-9_dau6n7.png)

Feche a janela, e pronto! Seu usuário está configurado. Moleza 😄 #sqn

### 3. Adicione as chaves de usuário nas variáveis de ambiente

Como último passo para termos o *Serverless Framework* configurado com a *AWS*, precisamos adicionar as chaves geradas nas variáveis de ambiente:

```
$ export AWS_ACCESS_KEY_ID=<access-key-id>
$ export AWS_SECRET_ACCESS_KEY=<secret-access-key>
```

## Antes de ir embora

Um desabafo, faz tempo que não publico nada por aqui e estava sentindo falta disso. Por isso, se algo não ficou claro o suficiente ou se surgir algo com que eu possa ajudar, é só comentar aqui embaixo.

Espero que tenham gostado, e no próximo artigo pretendo ensinar a como usar essas configurações para fazer *deploy* de uma função que publica posts agendados no Jekyll.

Até breve!

> O homem superior atribui a culpa a si próprio; o homem comum aos outros. **- Confúcio**














