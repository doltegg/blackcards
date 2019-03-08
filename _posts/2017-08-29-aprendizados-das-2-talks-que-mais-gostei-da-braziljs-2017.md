---
layout: post
title: "Aprendizados das 2 talks que mais gostei da BrazilJS 2017"
date: 2017-08-29 10:49:35
image: 'http://res.cloudinary.com/dm7h7e8xj/image/upload/c_scale,w_760/v1504023028/braziljs-2017_mazdtw.jpg'
description: Um resumo das palestras que mais me ensinaram nesse evento incrível.
category: 'webdev'
tags:
- braziljs
- eventos
- javascript
twitter_text: Um resumo das palestras que mais me ensinaram nesse evento incrível.
introduction: Foi uma experiência sensacional participar desse evento! Nesse artigo eu compartilho um pouco do que levei comigo.
---

Até recentemente estava buscando um posicionamento na minha carreira. Depois de
fechar a Sheeper, precisava me encaixar novamente no mercado e não estava, aliás,
ainda não está sendo trivial essa tarefa.

Gosto de back-end, gosto de front-end e gosto de liderar pessoas. Em que área devo
focar?

Essa é a pergunta que vêm latejando na minha cabeça há alguns meses. Pensei que
seguir pelo back-end, como eu já estava fazendo antes de abrir a empresa seria o
melhor caminho, mas fazendo uma reflexão do que eu realmente gosto de fazer, o
front-end e a parte de liderança ganharam mais força.

Pra me ajudar nessa missão, decidi que iria começar a participar de eventos pra
conhecer mais gente, conversar sobre as tendências do mercado, sobre as tecnologias
envolvidas, e enfim, sentir *a vibe* de cada área pra tentar achar qual é o
meu lugar.

E foi assim que lá no mês de maio comecei a procurar por eventos, e descobri que
o maior evento de JavaScript do mundo acontecia aqui no Brasil, em Porto Alegre,
já há alguns anos.

Não pensei duas vezes e comprei os ingressos logo no primeiro lote. Eu iria para
o meu primeiro grande evento de Web!

## Índice

- [Por quê escrever esse artigo?](#por-que-escrever-esse-artigo)
- [Talk 1 - Zen and the Art of Code Maintenance](#zen-and-the-art-of-code-maintenance)
- [Talk 2 - Estoicismo e Javascript](#estoicismo-e-javascript)
- [Outras talks inspiradoras](#outras-talks-inspiradoras)
- [Conclusão](#conclusao)


<h2 id="por-que-escrever-esse-artigo"> Por quê escrever esse artigo?</h2>

Durante 2 dias assisti a palestras incríveis, e muitas me impactaram de maneira
positiva. Depois de ouvir tanta coisa legal de gente tão experiente, achei que esse
conhecimento merecia ser compartilhado, por isso estou aqui.

Esse post vai ser um pouco longo, mas vou tentar resumir as duas palestras que mais
gostei na **BrazilJS Conf 2017**.

Sem dúvida não é uma tarefa fácil, mas escolher apenas 2 não significa que
as outras não foram excelentes. Só significa que no momento que estou vivendo, essas
que escolhi foram as que mais me impactaram e me agregaram como desenvolvedor e
pessoa.

Então vamos lá!

<h2 id="zen-and-the-art-of-code-maintenance">"Zen and the Art of Code Maintenance" por @JemYoung</h2>

[Jem Young](https://twitter.com/JemYoung) é engenheiro de software senior na Netflix. Ele é um cara super alto que curte código limpo e que junto com outros engenheiros de companhias famosas faz um podcast chamado
[Front End Happy Hour](https://twitter.com/FrontendHH).

<iframe width="800" height="450" src="https://www.youtube.com/embed/EpgIqysA3Fw?start=400" frameborder="0" allowfullscreen></iframe>

Achei a talk dele muito interessante porque bate bastante com a forma que eu penso sobre código.
As coisas devem ser simples e fáceis de entender para que o próximo desenvolvedor que pegar
o seu código não tenha muito trabalho.

Enxuguei a palestra dele em alguns tópicos para facilitar a leitura.

### Código bom é código de fácil manutenção

Código Usável + Código Escalável = Código Bom

#### Usável

Precisa responder a essas perguntas:

- Você pode encontrar e consertar bugs rapidamente?
- Você pode integrar com outro código sem quebrar as coisas?
- É uma implementação direta? Ou você tem que procurar na documentação pra entender o código?

#### Escalável

Não tem a ver com os recursos da máquina, mas se é escalável em relação às pessoas.

- Seu código pode ser usado por 20 pessoas? Por 100 pessoas?
- Seu código usa padrões de projeto simples? Ou o próximo desenvolvedor vai ter que gastar 2 semanas entendendo?

#### Exemplos

Código usável e **não** escalável:

```javascript
function count() {
    console.log('1');
    console.log('2');
    console.log('3');
    console.log('4');
    console.log('5');
    console.log('6');
    console.log('7');
    console.log('8');
    console.log('9');
    console.log('10');
}
```

Código usável **E** escalável:

```javascript
function count(max) {
    for (let i = 0; i <= max; i++) {
        console.log(i);
    }
}
```

### Lição 1 - Nomeie as coisas apropriadamente

Exemplo errado:

```javascript
function foo(x) {
    const a = 'er';
    return x + a;
}
```

Exemplo certo:

```javascript
function erify(word) {
    const ER = 'er';
    return word + ER;
}
```

### Lição 2 - Evite código inteligente

Exemplo de código inteligente:

```javascript
-~["a", "b"].indexOf("d")
```

O código acima funciona, mas exige um conhecimento avançado de como esses operadores
funcionam a nível de bytes para trazer um resultado **verdadeiro** ou **falso**.

E pode ser substituído simplesmente por:

```javascript
["a", "b"].indexOf("d") > -1
```

### Lição 3 - Comente o seu código

Essa lição em particular eu achei controversa, pois se você precisa comentar o
seu código, quer dizer que a lição 1 não foi bem aplicada, e você não nomeou as
coisas apropriadamente.

Por outro lado, concordo que se você tem vários `import` no seu código, fica
muito mais legível separá-los com comentários.

Exemplo:

```javascript
/* Libraries */
import React from 'react';
import PropTypes from 'prop-types';
import createReactClass from 'create-react-class';
import _ from 'lodash';
/* Loggers */
import eventLogger from '../logging/event';
import mobileLogger from '../logging/mobile';
/* Components */
import Input from '../../components/form/input';
import Checkbox from '../../components/form/Checkbox';
import scanner from '../../components/scanner';
```

### Lições 4 e 5 - Seja deliberado e consistente

Ou decidido na hora de escrever o código. Se você tem um estilo de programar,
continue com esse estilo, não importa qual seja, mas seja decidido e sempre
faça do mesmo jeito.

Seguindo um padrão, seu código fica organizado e mais compreensível para o próximo
desenvolvedor, que vai entender o seu padrão.

Exemplos práticos são o uso de *tabs* **vs** *espaços*, testes em pastas diferentes **vs**
testes na mesma pasta, css no mesmo arquivo **vs** css em arquivos separados e por aí vai.

### Lição 6 - Entenda as ferramentas

Você não precisa conhecer todas as ferramentas e/ou frameworks disponíveis a fundo,
ou saber fazê-las absolutamente do zero. Mas você precisa saber **quando usá-las e
quando não usá-las**.

O importante é entender o custo das suas escolhas e qual é a ferramenta certa
para o resultado que você deseja obter, seja usando AngularjS, React ou VueJS, por
exemplo.

<h2 id="estoicismo-e-javascript">"Estoicismo e Javascript" por @zenorocha</h2>

[Zeno Rocha](https://twitter.com/zenorocha) é um desenvolvedor front-end brasileiro, palestrante e
entusiasta open source, que vive em Los Angeles e também foi criador da lib [clipboardjs](https://clipboardjs.com/).

<iframe width="800" height="450" src="https://www.youtube.com/embed/8vtVmLviiGQ?start=37710" frameborder="0" allowfullscreen></iframe>

O que eu mais gostei da palestra do Zeno foi essa pegada mais voltada pra questões
que pouca gente debate, mas que todo mundo tem alguma vez na vida. Como por exemplo
que carreira dentro da área de TI devemos seguir ou o que estudar depois de escolher
uma área.

Ele também fala das barreiras que colocamos para não seguirmos nossos objetivos
na vida e conecta tudo isso com filosofia.

Vou tentar resumir em alguns tópicos a palestra dele.

### O que eu devo estudar?

Existem N tecnologias no mercado e ficamos tentando entender qual é a que vai vingar
para focarmos e cairmos de cabeça naquilo.

Porém, sempre existirão novas tecnologias, é impossível saber o que vai dar certo
e o que não vai.

A solução pra isso? Escolher uma área, escolher uma tecnologia e focar. Esse é o único
caminho, e não vale a pena se preocupar.

### Por que as tecnologias mudam toda hora?

No mundo front-end sempre há um novo framework ou uma nova biblioteca surgindo, e isso
é meio desesperador, porque não dá tempo de estudar tudo.

As tecnologias mudam toda hora porque o ser humano é assim. Código é o resultado 
de se expressar criativamente para resolver um problema e também não devemos nos
preocupar com isso.

Uma profissão na área de tecnologia é a única profissão onde tudo vai mudar o tempo
todo e devemos estar preparados pra isso.

O que não quer dizer que temos que aprender todo framework novo e seguir todos os *hypes*,
mas quer dizer que devemos saber lidar com essas mudanças e tomar as melhores decisões
possíveis no caminho.

### Aprender algo novo não é tão difícil quanto parece

Por as coisas mudarem bastante e muito rápido, às vezes ficamos com medo de começar
e aprender algo novo, e achamos que não vamos ficar bons naquilo porque já tem tanta
gente num nível mais avançado que parece impossível alcançar.

Porém, esse é um medo muitas vezes inexistente, basta ter paciência e seguir com passos
pequenos, mas constantes.

### O propósito por trás do Open Source

Ouvimos o tempo todo que abrir o código é o que todo desenvolvedor deve fazer, você deve colocar
seu código no Github porque é o que os recrutadores vão usar para te analisar, como
se fosse um currículo.

Ou então acham que criando algo fora do comum e colocando no mundo vai de alguma
forma trazer fama ou reconhecimento, e a verdade não é bem assim.

Devemos saber o propósito de contribuir com código open source. É ajudar outra pessoa?
De repente o problema que você teve e solucionou pode salvar 30 min de uma pessoa, que
poderiam ser gastos pra estar com a família dela, por exemplo.

O Open Source não é só colocar código no Github, mas um estilo de vida. Tudo o que
você faz hoje, pode ser compartilhado. E essa é uma ação que pode desencadear N
reações, mas que com certeza algum proveito você, ou a pessoa que está usando seu
código vai ter lá na frente.

### Código dos outros

Temos a tendência de criticar e julgar outros desenvolvedores unicamente por seu
código. Seja quando pegamos um freela de um projeto que já existe, ou quando
entramos numa nova empresa e o desenvolvedor que lidava com o projeto não está
mais lá, enfim, várias situações.

E o que precisamos entender é que o código é o resultado de uma situação, seja
a empresa que não está indo bem, ou o projeto que teve complicações, são vários
os cenários, mas não devemos julgar outros desenvolvedores unicamente por um
código que naquele momento não era tão bom.

### Ação e Reação

Por último, ouvimos muita gente falando coisas como:

*"Tenho uma ideia incrível, preciso colocar em prática..."*

*"Meu trabalho é uma droga, preciso mudar de emprego..."*

*"Esse país não tem jeito, preciso sair do Brasil..."*

E essas frases são seguidas de um **mas**:

*"...mas eu não tenho tempo."*

*"...mas eu não tenho dinheiro."*

*"...mas eu não sei inglês direito."*

E são sentenças válidas, são circunstâncias reais. Porém, também são muito cômodas, pois
não vem acompanhadas de um plano de ação pra mudar isso.

A única pessoa que impede você de alcançar alguma coisa, é você mesmo.

Devemos sempre pensar **"o que eu quero fazer?"** e o **"o que falta pra eu chegar lá?"**.

E então tomar atitudes para conquistarmos a mudança que desejamos.

<h2 id="outras-talks-inspiradoras">Outras talks inspiradoras</h2>

Além dessas que mais gostei, também teve palestras que achei muito inspiradoras
e que valem a pena assistir:

### "How to be a Compiler" pela @kosamari

[Mariko Kosaka](https://twitter.com/kosamari) é desenvolvedora web no Google e
co-organizadora do [brooklyn_js](https://twitter.com/brooklyn_js) e do [ ridgewood_js](https://twitter.com/ridgewood_js).

Nessa talk ela mostra de um jeito muito simples e divertido como desenvolver um
pequeno compilador em JavaScript.

<iframe width="800" height="450" src="https://www.youtube.com/embed/EpgIqysA3Fw?start=11694" frameborder="0" allowfullscreen></iframe>

### "Como um Projeto JS Open Source se transformou em uma empresa de 60 milhões de reais" por @gabriel_engel

[Gabriel Engel](https://twitter.com/gabriel_engel) é fundador da [Rocket.Chat](https://rocket.chat/) e nessa talk ele conta como uma equipe de brasileiros utilizou o poder do open source para
transformar um projeto paralelo em uma startup de US$ 17 milhões.

Foi bem inspiradora essa palestra, a história desses caras é sensacional.

<iframe width="800" height="450" src="https://www.youtube.com/embed/8vtVmLviiGQ?start=33016" frameborder="0" allowfullscreen></iframe>

### "JS gives you Superpowers, use them" por @_Gui_Souza

[Guilherme Souza](https://twitter.com/_gui_souza) é CTO na Revmob e tem 10 anos de experiência
com EcmaScript. Nessa talk ele simplesmente mostra como foi construir um protótipo de uma
prótese humana usando JavaScript no maior estilo Iron Man.

Foi a última palestra do evento e ele dá um tapa na cara de quem acha que sabe tudo,
essa palestra foi pura emoção.

<iframe width="800" height="450" src="https://www.youtube.com/embed/EpgIqysA3Fw?start=28621" frameborder="0" allowfullscreen></iframe>

<h2 id="conclusao">Conclusão</h2>

É difícil não falar de todas as outras talks, mas tentei dar destaque aqui para
aquelas que mais curti.

Como eu disse, esse foi meu primeiro grande evento de Web, e saí de lá motivado
e inspirado a compartilhar meu conhecimento e ser um profissional e uma pessoa cada
vez melhor.

Um evento com mais de 1400 pessoas, muito bem organizado, num ambiente de respeito,
e que contribui bastante pro networking entre a galera, ah! E na cidade com mais
barzinhos que conheci depois de São Paulo.

Indico demais esse evento e com certeza estarei nos próximos. No mais, só tenho
que agradecer o aprendizado e a oportunidade de conhecer e assistir a pessoas tão
fodas. Obrigado **BrazilJS**!

## Antes de ir...

Se ficou interessado em assistir a **todas** as palestras do evento, segue os links:

- [BrazilJS Conf 2017 - Dia 1](https://www.youtube.com/watch?v=8vtVmLviiGQ)
- [BrazilJS Conf 2017 - Dia 2](https://www.youtube.com/watch?v=EpgIqysA3Fw)






















