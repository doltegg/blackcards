---
layout: post
title: "16:9, 4:3, 1:1 - Como criar &lt;div&gt;s proporcionais e responsivas"
date: 2019-04-16 18:09:51
image: 'https://res.cloudinary.com/dm7h7e8xj/image/upload/v1555451923/antman_qmf7qk.jpg'
description: Manter o aspect ratio de um elemento com CSS é bem fácil, neste artigo explico como podemos fazer isso.
category: 'css'
tags:
- css
- aspect ratio
twitter_text: Aprenda a manter o aspect ratio de elementos na tela com CSS facilmente.
introduction: Aprenda a manter o aspect ratio de elementos na tela com CSS facilmente.
---

Sabe aquele mosaico bonito de elementos criado pelo design onde cada coisa tem sua devida proporção, não importa qual é o tamanho da tela do usuário?

E aquela `<div>` que você tem que manter em 480x360 a todo custo pro layout funcionar?

Casos assim não são tão frequentes, mas quando aparecem costumam dar um certo trabalho.

O objetivo deste artigo é ensinar a como lidar com elementos que devem permanecer proporcionais o tempo todo.

Aqui estão alguns exemplos:

<p class="codepen" data-height="450" data-theme-id="dark" data-default-tab="result" data-user="thiagorossener" data-slug-hash="ROLKyP" style="height: 450px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="16:9, 4:3, 1:1 - Aspect ratio panels">
  <span>See the Pen <a href="https://codepen.io/thiagorossener/pen/ROLKyP/">
  16:9, 4:3, 1:1 - Aspect ratio panels</a> by Thiago Rossener (<a href="https://codepen.io/thiagorossener">@thiagorossener</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p><script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Para entendermos onde queremos chegar, vou pegar como exemplo a criação de um elemento com a proporção **4:3**.

Vou chamar este elemento de `panel` e criá-lo dentro de um `container`:

```html
<div class="container">
    <div class="panel"></div>
</div>
```

Vou também limitar a largura do *container* a `50%`, centralizá-lo na tela e colocar uma cor vermelha no painel, assim podemos visualizá-lo:

```css
.container {
    width: 50%;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}

.panel {
    background-color: red;
}
```

## Abordagem 1

Digamos que eu tenha um elemento com `400px` de largura, e quero que ele seja proporcional. Para isso, fazemos uma regra de 3 simples:

<table cellpadding="0" cellspacing="0" class="no-border">
    <tr>
        <td class="gutter gl">400px</td>
        <td class="gutter gl">-></td>
        <td class="gutter gl">4</td>
    </tr>
    <tr>
        <td class="gutter gl">x</td>
        <td class="gutter gl">-></td>
        <td class="gutter gl">3</td>
    </tr>
</table>

<p class="center">x = (400 * 3)/4 = <strong>300px</strong></p>

Legal, temos a largura e a altura, bora atribuir os valores (vou adicionar também `margin: 0 auto;` pra centralizar o painel).

```css
.panel {
    background-color: red;
    width: 400px;
    height: 300px;
    margin: 0 auto;
}
```

Então temos:

<p class="codepen" data-height="450" data-theme-id="dark" data-default-tab="result" data-user="thiagorossener" data-slug-hash="bJamyz" style="height: 450px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="Fixed aspect ratio example">
  <span>See the Pen <a href="https://codepen.io/thiagorossener/pen/bJamyz/">
  Fixed aspect ratio example</a> by Thiago Rossener (<a href="https://codepen.io/thiagorossener">@thiagorossener</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>

### Problema!

A *abordagem 1* é bem direta e funciona bem se precisamos de um elemento com dimensões fixas, mas e se o usuário está visualizando no celular? Com dimensões fixas, provavelmente irá aparecer um scroll para que ele consiga ver o elemento inteiro.

Podemos adicionar *media queries* para alterar esses valores dependendo da resolução do usuário, mas conseguimos fazer melhor que isso, vamos à próxima abordagem.

## Abordagem 2

E se ao invés de usarmos valores fixos para as dimensões, usássemos porcentagens? Assim garantimos que nenhum scroll vai aparecer, já que o tamanho do elemento se manterá proporcional ao tamanho da tela, certo? Vamos tentar.

Digamos que queremos um elemento com 100% da largura do seu elemento pai. Fazendo uma regra de 3 novamente:

<table cellpadding="0" cellspacing="0" class="no-border">
    <tr>
        <td class="gutter gl">100%</td>
        <td class="gutter gl">-></td>
        <td class="gutter gl">4</td>
    </tr>
    <tr>
        <td class="gutter gl">x</td>
        <td class="gutter gl">-></td>
        <td class="gutter gl">3</td>
    </tr>
</table>

<p class="center">x = (100% * 3)/4 = <strong>75%</strong></p>

Agora atribuímos `width: 100%;` e `height: 75%;` e está resolvido. **ERRADO!**

Isso está errado por causa da forma como a propriedade `height` funciona com porcentagens no CSS.

Segundo a [documentação](https://developer.mozilla.org/en-US/docs/Web/CSS/height):

> Percentages: The percentage is calculated with respect to the height of the generated box's containing block. If the height of the containing block is not specified explicitly (i.e., it depends on content height), and this element is not absolutely positioned, the value computes to auto. A percentage height on the root element is relative to the initial containing block.

Ou seja, se o elemento usando a propriedade `height` com porcentagem não tiver um **pai** que tenha a altura devidamente especificada (ex: `px`, `rem`, `em`, ou outra unidade que não seja porcentagem), ou que esteja posicionado usando `position: absolute;`, ela é computada para o valor `auto` e a porcentagem atribuída é ignorada.

### Problema!

Na *abordagem 2*, temos que atribuir um valor fixo para o elemento pai, e aí o filho usando um `height` com porcentagem vai ter sua altura proporcional a altura fixa do pai, perdendo todo o sentido de se usar a porcentagem e voltando para o problema da *abordagem 1*.

Então como atribuímos porcentagem ao elemento de uma forma que a altura fique proporcional a qualquer largura que a gente possa querer?

## Abordagem 3

Sem mais rodeios, o pulo do gato aqui é usar a propriedade `padding` com porcentagem!

Segundo a [documentação](https://developer.mozilla.org/en-US/docs/Web/CSS/padding):

> Percentages: refer to the width of the containing block

"Porcentagens são referentes a largura do bloco contendo o elemento", perfeito! É disso que precisamos.

Então atribuímos `width: 100%;` e `padding-top: 75%;`:

```css
.panel {
    background-color: red;
    width: 100%;
    padding-top: 75%;
}
```

E temos:

<p class="codepen" data-height="450" data-theme-id="dark" data-default-tab="css,result" data-user="thiagorossener" data-slug-hash="ZZvmbW" style="height: 450px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid black; margin: 1em 0; padding: 1em;" data-pen-title="Flexible aspect ratio example">
  <span>See the Pen <a href="https://codepen.io/thiagorossener/pen/ZZvmbW/">
  Flexible aspect ratio example</a> by Thiago Rossener (<a href="https://codepen.io/thiagorossener">@thiagorossener</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p><script async src="https://static.codepen.io/assets/embed/ei.js"></script>

Resolvido!

> **Nota:** Usei o `padding-top`, mas poderia ter usado também o `padding-bottom`, tanto faz. A mágica só acontece porque o tamanho de qualquer *padding* em porcentagem sempre se refere a **largura** do elemento.

## Refinando a solução

O elemento está criado, é proporcional e responsivo, mas há algumas melhorias que ainda podemos fazer:

### Sem contas na mão!

Ao invés de fazer as contas manualmente, podemos usar a [função `calc()` do CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/calc) para fazer isso por nós. Alterando o código:

```css
.panel {
    background-color: red;
    width: 100%;
    padding-top: calc((3 / 4) * 100%);
}
```

### Inserindo conteúdo

Você deve ter percebido que se quisermos inserir conteúdo no painel, a proporção será perdida, pois qualquer coisa dentro de `<div class="panel"></div>` será inserida considerando o espaçamento do *padding*.

Para resolver esse problema, podemos simplesmente alterar o painel para que ele tenha um posicionamento relativo, e seu conteúdo tenha um posicionamento absoluto, assim o conteúdo fica sobreposto e não sofre ação do *padding*.

HTML:

```html
<div class="container">
    <div class="panel">
        <div class="content">Conteúdo vem aqui</div>
    </div>
</div>
```

CSS:

```css
.panel {
    background-color: red;
    width: 100%;
    padding-top: calc((3 / 4) * 100%);
    position: relative;
}

.content {
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
}
```

Fico por aqui e espero que tenha curtido as dicas. Até a próxima!

> Aja antes de falar e, portanto, fale de acordo com os seus atos. **- Confúcio**
