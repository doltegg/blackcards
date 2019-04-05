---
layout: post
title: "Criando uma galeria de imagens infinita com JavaScript puro"
date: 2019-04-05 04:48:04
image: 'https://res.cloudinary.com/dm7h7e8xj/image/upload/v1554487569/infinite-gallery_kvnpbj.jpg'
description: Neste artigo ensino a criar uma galeria de slides bem legal usando poucas linhas de código.
category: 'tutorial'
tags:
- css
- javascript
twitter_text: Neste artigo ensino a criar uma galeria de slides bem legal usando poucas linhas de código.
introduction: Neste artigo ensino a criar uma galeria de slides bem legal usando poucas linhas de código.
---

Fazer uma galeria de imagens é algo recorrente pra quem trabalha com front-end, essa semana eu tive a oportunidade de criar uma bem maneira e gostaria de compartilhar com vocês.

Ao final deste artigo, você vai ter algo assim:

<p class="codepen" data-height="400" data-theme-id="dark" data-default-tab="result" data-user="thiagorossener" data-slug-hash="WmqQyo" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="Infinite slides gallery using vanilla JS">
  <span>See the Pen <a href="https://codepen.io/thiagorossener/pen/WmqQyo/">
  Infinite slides gallery using vanilla JS</a> by Thiago Rossener (<a href="https://codepen.io/thiagorossener">@thiagorossener</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Antes de começarmos, vamos entender o que será feito.

A ideia é termos um efeito de continuidade sempre que clicarmos para **voltar** ou **avançar**, para isso, precisamos de pelo menos mais um slide escondido do lado esquerdo e mais um do lado direito.

Se houverem mais slides, eles deverão se acumular do lado direito, simplesmente porque é mais fácil trabalhar com o CSS dessa maneira.

Ao clicar para mostrar o slide anterior, o último slide escondido do lado direito deve se tornar o primeiro slide da lista.

Ao clicar para mostrar o próximo slide, o primeiro slide (escondido) deverá ir para a última posição da lista.

Pra você visualizar melhor, criei esse esquema:

![Esquema em SVG](https://res.cloudinary.com/dm7h7e8xj/image/upload/v1554489474/scheme_g28htl.svg)

Muito bem, agora que está tudo explicado, vamos primeiro criar a estrutura HTML e estilizar esses slides, então adicionar as funcionalidades.

## Nossa estrutura

A estrutura do HTML é bem simples, vou utilizar classes para adicionar as imagens como plano de fundo e manter os slides proporcionais.

```html
<div class="gallery">
  <div class="gallery__prev"></div>
  <div class="gallery__next"></div>
  <div class="gallery__stream">
    <div class="gallery__item bg-1"></div>
    <div class="gallery__item bg-2"></div>
    <div class="gallery__item bg-3"></div>
    <div class="gallery__item bg-4"></div>
    <div class="gallery__item bg-5"></div>
    <div class="gallery__item bg-6"></div>
    <div class="gallery__item bg-7"></div>
  </div>
</div>
```

## Estilizando!

Primeiro vamos estilizar a galeria e seus itens. Note que os elementos estão centralizados verticalmente, fiz isso somente para servir de exemplo.

```css
/* Global */

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  background-color: #000;
}

/* Galeria */

.gallery {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  width: 100%;
  height: 90%;
  max-height: 28vw;
  overflow: hidden;
}

.gallery__stream {
  position: relative;
  top: 50%;
  transform: translateY(-50%);
  width: 100%;
  height: 100%;
}

.gallery__item {
  position: absolute;
  width: 50%;
  height: 100%;
  transition: 1s all ease;
  background-size: cover;
  background-repeat: no-repeat;
  background-position: center center;
  border-radius: 5px;
}
```

Perceba que foi adicionada uma propriedade `transition` no nosso item da galeria, é esse cara que vai dar esse efeito legal de escala quando trocamos de slide 😄

Agora posicionamos cada item da galeria, considerando também sua posição no eixo Z, e assim a gente evita que slides fiquem sobrepostos de maneira errada.

Para isso vamos usar o `:nth-child`, o seletor de filhos do CSS em que iremos estilizar os elementos de acordo com a ordem em que eles se apresentam dentro de um elemento pai em comum, mais informações sobre isso você encontra [aqui](https://www.w3schools.com/cssref/sel_nth-child.asp).

```css
/* Itens */

.gallery__item:nth-child(1) {
  left: 0;
  z-index: 1;
  transform: translateX(-100%) scale(.8);
}

.gallery__item:nth-child(2) {
  left: 0;
  z-index: 2;
  transform: translateX(-50%) scale(.8);
}

.gallery__item:nth-child(3) {
  left: 50%;
  z-index: 4;
  transform: translateX(-50%) scale(1);
}

.gallery__item:nth-child(4) {
  left: 100%;
  z-index: 2;
  transform: translateX(-50%) scale(.8);
}

.gallery__item:nth-child(n+5) {
  left: 100%;
  z-index: 1;
  transform: scale(.8);
}
```

Até aqui nada está sendo exibido, já que ainda não temos nossas imagens, então vamos criar as classes com os backgrounds:

```css
/* Backgrounds */

.bg-1 {
  background-image: url(https://res.cloudinary.com/dm7h7e8xj/image/upload/v1554487589/star-wars-1_kgt8dr.jpg);
}

.bg-2 {
  background-image: url(https://res.cloudinary.com/dm7h7e8xj/image/upload/v1554487590/star-wars-2_tgzyxe.jpg);
}

.bg-3 {
  background-image: url(https://res.cloudinary.com/dm7h7e8xj/image/upload/v1554487591/star-wars-3_bkcmeb.jpg);
}

.bg-4 {
  background-image: url(https://res.cloudinary.com/dm7h7e8xj/image/upload/v1554487591/star-wars-4_opgyza.jpg);
}

.bg-5 {
  background-image: url(https://res.cloudinary.com/dm7h7e8xj/image/upload/v1554487591/star-wars-5_ulc9tx.jpg);
}

.bg-6 {
  background-image: url(https://res.cloudinary.com/dm7h7e8xj/image/upload/v1554487591/star-wars-6_la8whc.jpg);
}

.bg-7 {
  background-image: url(https://res.cloudinary.com/dm7h7e8xj/image/upload/v1554487590/star-wars-7_l3fcor.jpg);
}
```

Pronto, o Anakin já pintou na tela!

Para fazer nossos botões de **próximo** e **anterior**, iremos estilizá-los exatamente como os slides visíveis na tela do lado esquerdo e do lado direito.

Iremos também posicioná-los uma camada acima desses slides, mas com um fundo transparente, assim irá parecer que estamos clicando nos slides, quando na verdade estamos clicando nos botões 😆

```css
/* Controles */

.gallery__prev,
.gallery__next {
  position: absolute;
  top: 50%;
  z-index: 4;
  width: 50%;
  height: 100%;
  transform: translateX(-50%) translateY(-50%) scale(.8);
  cursor: pointer;
}

.gallery__prev {
  left: 0;
}

.gallery__next {
  left: 100%;
}
```

Finalmente, por uma simples questão de estética, vamos adicionar um efeito de degradê nas laterais da galeria, utilizando os pseudo elementos `:before` e `:after`, mais sobre eles [aqui](https://www.w3schools.com/css/css_pseudo_elements.asp).

> **Nota:** Atenção ao `z-index` aqui! Se esses caras estiverem numa camada acima dos botões, não será possível clicar neles. Por isso, posicionamos esses elementos entre os slides e os botões transparentes.

```css
/* Sombras */

.gallery:before,
.gallery:after {
  display: block;
  content: "";
  position: absolute;
  top: 0;
  width: 20%;
  height: 100%;
  z-index: 3;
}

.gallery:before {
  left: 0;
  background:  linear-gradient(to right, rgba(0,0,0,1) 0%, rgba(0,0,0,0) 100%);
}

.gallery:after {
  right: 0;
  background:  linear-gradient(to left, rgba(0,0,0,1) 0%, rgba(0,0,0,0) 100%);
}
```

## Fazendo funcionar

Agora que a parte mais trabalhosa já foi, vamos fazer funcionar. Primeiro temos de garantir que todos os elementos estão na tela, criando nossas funções após o evento `DOMContentLoaded`.

```js
document.addEventListener('DOMContentLoaded', function() {
    // O código vem aqui
});
```

Então selecionamos os elementos que precisamos:

```js
document.addEventListener('DOMContentLoaded', function() {
  var stream = document.querySelector('.gallery__stream');
  var items = document.querySelectorAll('.gallery__item');
  var prev = document.querySelector('.gallery__prev');
  var next = document.querySelector('.gallery__next');
});
```

E por fim, adicionamos os eventos de clique nos botões `prev` e `next`:

```js
document.addEventListener('DOMContentLoaded', function() {
  var stream = document.querySelector('.gallery__stream');
  var items = document.querySelectorAll('.gallery__item');
  var prev = document.querySelector('.gallery__prev');
  var next = document.querySelector('.gallery__next');

  prev.addEventListener('click', function() {
    stream.insertBefore(items[items.length - 1], items[0]);
    items = document.querySelectorAll('.gallery__item');
  });

  next.addEventListener('click', function() {
    stream.appendChild(items[0]);
    items = document.querySelectorAll('.gallery__item');
  });
});
```

> **Atenção!** Perceba que atualizamos o valor da variável `items`, isso é muito importante, pois uma vez que modificamos a posição dos itens no DOM, a variável precisa ter esses itens atualizados na ordem correta ou estaremos selecionando os itens errados no próximo clique.

A ideia deste post foi mostrar como é fácil criar uma galeria bem bacana com poucas linhas de código, obviamente este código possui algumas limitações, como precisar ter *no mínimo 5 slides para funcionar corretamente*.

Além disso, há uma série de melhorias que poderiam ser implementadas aqui, deixo como sugestão algumas delas para você tentar:

- Utilizar a [lazysizes](https://github.com/aFarkas/lazysizes), ou qualquer outra lib de **lazy loading** para carregar as imagens somente quando elas forem ser exibidas.
- Controlar a velocidade do clique do usuário, evitando que ele clique repetidas vezes nos botões, perdendo todo o efeito de transição.
- Adicionar indicadores para o usuário saber em que slide está.

Espero que tenha curtido e até a próxima!
