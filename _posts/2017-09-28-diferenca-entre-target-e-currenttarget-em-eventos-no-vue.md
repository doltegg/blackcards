---
layout: post
title: "Diferença entre target e currentTarget em eventos no Vue.js"
date: 2017-09-28 01:30:24
image: 'http://res.cloudinary.com/dm7h7e8xj/image/upload/v1506567296/sniper-americano_wmr1a2.jpg'
description: Saiba a diferença para entender a hora certa de usar cada um.
category: 'javascript'
tags:
- javascript
- vuejs
twitter_text: Saiba a diferença para entender a hora certa de usar cada um.
introduction: Neste artigo falo sobre propagação de eventos entre elementos do DOM.
---

Usar bibliotecas como o jQuery, ou frameworks como o Vue.js, muitas vezes faz a gente
esquecer do básico no JavaScript.

Uma das coisas que por esses dias tive de pesquisar para lembrar é a diferença entre
as propriedades **e.target** e **e.currentTarget**, que vem do objeto que é passado nas
*callbacks* de eventos.

A razão pela qual precisamos de duas propriedades para lidar com o *target* é por causa
do efeito de borbulhar, ou *bubbling*, que temos nos eventos.

Esse efeito ocorre quando um evento disparado em um nó percorre todo o DOM até chegar no nó raiz do documento, disparando todos os listeners que ele encontra no caminho, como num efeito em cascata.

Pra ficar mais fácil de entender, vamos colocar isso no código. Imagine que temos a seguinte estrutura HTML:

```html
<div id="localizacao">
    <div class="europa" @click="mostraLocal($event)" title="Europa">
        <div class="inglaterra" @click="mostraLocal($event)" title="Inglaterra">
            <div class="londres" @click="mostraLocal($event)" title="Londres">
                <div class="baker-street" title="Baker St."></div>
            </div>
        </div>
    </div>
</div>
```

Então pra cada camada a gente tem um listener para o evento *click*, exceto a camada
mais interna, que chamei de **Baker St.**.

Agora vamos criar uma instância do Vue para logar o título de cada camada quando
clicarmos.

```js
new Vue({
    el: '#localizacao',
    methods: {
        mostraLocal: function (event) {
          console.log('target >', event.target.title,
                      'currentTarget >', event.currentTarget.title);
        }
    }
});
```

Assim temos um nó mais externo que chamei de **Europa**, dentro um nó **Inglaterra**,
dentro dele um nó **Londres** e dentro um nó **Baker St.**.

Quando clicamos em *Baker St.*, um evento de *click* irá disparar e vamos ter o
seguinte resultado no console:

```js
// target > Baker St. currentTarget > Londres
// target > Baker St. currentTarget > Inglaterra
// target > Baker St. currentTarget > Europa
```

Observe que:

1. O *target* sempre foi *Baker St*, porém o *currentTarget* mudou várias vezes, pois o evento foi disparado no elemento mais interno, e foi "borbulhando" para cima, ativando o listener de **Londres**, depois de **Inglaterra**, depois de **Europa**.
1. Clicamos em *Baker St.*, mas ele nunca foi o *currentTarget*, simplesmente porque não adicionamos um listener para o elemento.

Adicionando um CSS para ficar mais claro, a gente tem esse resultado:

<p data-height="408" data-theme-id="0" data-slug-hash="boWeyv" data-default-tab="result" data-user="thiagorossener" data-embed-version="2" data-pen-title="Event bubbling 1" class="codepen">See the Pen <a href="https://codepen.io/thiagorossener/pen/boWeyv/">Event Bubbling</a> by Thiago Rossener (<a href="https://codepen.io/thiagorossener">@thiagorossener</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

Note que se você clicar em cada camada, somente a camada atual e as camadas mais externas que tenham o listener vão mudar.

Assim, a gente conclui que:

> O *target* é **exatamente** o elemento que eu cliquei. O *currentTarget* é o elemento que está **ouvindo o evento**.

## Curiosidade

Se você está fazendo o log de um evento no Vue.js vai perceber que sempre que você faz algo como:

```js
console.log(e);
```

Ele está **null**.

![Imagem do currentTarget null](http://res.cloudinary.com/dm7h7e8xj/image/upload/v1506602334/current-target-null_kno3cf.png)

Porém, se você faz:

```js
console.log(e.currentTarget);
```

Ele está **populado**. Veja o exemplo [aqui](https://codepen.io/thiagorossener/pen/xXdWEM).

Bizarro né? Bem, isso é o resultado de como o console trabalha quando você faz o log
de um objeto.

O log não contém uma cópia de todas as propriedades do objeto, ele só contém uma **referência**
para o objeto.

Quando você clica no triângulo para expandir o objeto, ele procura por todas as propriedades
para mostrá-las.

Nesse caso, quando você chama `console.log(e)`, existe um elemento DOM na propriedade **currentTarget**, mas algum tempo depois essa propriedade é *setada* para `null` pelo Vue, por alguma razão interna no seu funcionamento (se souber o porque ele faz isso posta nos comentários lá embaixo 😁).

Um exemplo simples para ver como o `console.log()` funciona é esse aqui:

```js
const foo = { };
for (let i = 0; i < 4; i++) {
    foo['n' + i] = i;
}
console.log(foo);
foo.n3 = 'opa';
```

No código aí em cima a gente faz o log do objeto logo em seguida de populá-lo,
porém se você expandir o objeto vai ver que o terceiro elemento já aparece alterado, mesmo que tenhamos alterado o valor dele **depois** de exibir o objeto.

Segue o exemplo [aqui](https://codepen.io/thiagorossener/pen/YrVegb).

![Imagem do console.log](http://res.cloudinary.com/dm7h7e8xj/image/upload/v1506601896/console-log_gyyl6a.png)

E é isso aí galera, espero que tenham gostado do artigo, se algo não ficou claro é
só comentar e vou tentar responder o mais rápido que puder.

Abraço pra todos vocês!




