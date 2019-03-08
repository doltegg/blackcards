---
layout: post
title: "Jekflix: Um tema para o Jekyll"
date: 2017-09-22 10:24:10
image: 'http://res.cloudinary.com/dm7h7e8xj/image/upload/c_scale,w_760/v1506079212/jekflix-capa_vfhuzh.png'
description: Criei esse tema baseado no painel da Netflix para os amantes de filmes e séries.
category: 'blog'
tags:
- blog
- tema
twitter_text: Criei esse tema baseado no painel da Netflix para os amantes de filmes e séries.
introduction: Criei esse tema baseado no painel da Netflix para os amantes de filmes e séries, espero que goste.
---

Como dizia Mestre Yoda:

> Faça ou não faça, a tentativa não existe <cite>&mdash; Yoda, Mestre</cite>

Nas últimas semanas estive preparando esse novo tema para o blog, como você pode
ver na [demo aqui](https://www.rossener.com/jekflix-template/).

Meu objetivo era treinar algumas habilidades de CSS3 e aplicar algumas ideias
que tive pra deixar a experiência de ler o blog mais agradável.

Assim nasceu o **Jekflix**. Um tema para o Jekyll inspirado no painel da Netflix e feito
com muito <svg class="love"><use xlink:href="#icon-heart"></use></svg>.

O tema está disponível no [meu repositório](https://github.com/thiagorossener/jekflix-template/) (gostou? deixa uma estrelinha lá!), o layout e as funcionalidades abaixo
foi tudo feito por mim, mas o *core* devo ao Willian Justen, que fez o
[Cards Jekyll Template](https://github.com/willianjusten/cards-jekyll-template).

Entre as *features* mais legais do tema estão:

### Tag de "Novo artigo"

Quando você publica um artigo, ele ganha essa tag de "Novo artigo" por 1 semana, e
desaparece automaticamente depois disso.

A ideia era mostrar claramente para o usuário o que tem de novo no blog, assim como
a Netflix faz quando tem novos episódios.

![Imagem de comparação com a Netflix](http://res.cloudinary.com/dm7h7e8xj/image/upload/v1506080565/new-post_qeoofh.jpg)

### Layout adaptativo

Se você está no celular, no tablet ou [na maior tela de games do mundo](https://www.youtube.com/watch?v=SommOdgWwUM), não importa.

O layout irá se ajustar ao tamanho da sua tela e irá mostrar da melhor forma possível
as postagens na tela inicial.

![Imagem da tela em formatos diferentes](http://res.cloudinary.com/dm7h7e8xj/image/upload/c_scale,w_760/v1506080907/layout-adaptativo_ky8tbx.jpg)

### Mouse hover

Ao passar o mouse sobre um artigo na tela inicial a imagem da postagem vai se ampliar,
ficar em preto e branco e um botão com um livrinho aberto vai aparecer no centro.

Te lembra alguma coisa? Exato. Netflix :)

![Imagem do mouse hover no tema](http://res.cloudinary.com/dm7h7e8xj/image/upload/c_scale,w_760/v1506081370/mouse-hover_yqlbo3.jpg)

### Tempo de leitura e barra de progresso

No layout antigo havia colocado o tempo de leitura em cada artigo. Porém, assistindo
a alguns episódios de Vikings tive uma ideia mais interessante e criei uma barra de progresso.

![Imagem da barra de progresso](http://res.cloudinary.com/dm7h7e8xj/image/upload/v1505357769/jekflix-progress-bar_he7gqf.jpg)

Assim como em um filme ou episódio na plataforma de streaming, a ideia é que o usuário
tenha uma noção de quanto tempo falta pra acabar a leitura e quanto tempo ele já gastou lendo.

Não é a barra mais precisa do mundo, mesmo porque ela não considera as imagens na
postagem, mas somente o tamanho final da tela, porém, dá uma boa noção e acho que isso
melhora bastante a experiência do usuário.

### Imagens sob demanda

Quando você tem vários artigos na página inicial, eles não são carregados todos de uma vez.

Ao fazer o scroll os artigos vão aparecendo aos poucos, e não só isso, as imagens de cada
artigo também só são carregadas quando ele aparece na tela.

![Imagem do loading](http://res.cloudinary.com/dm7h7e8xj/image/upload/v1506081779/loading_eou3xh.jpg)

Isso deixa o carregamento da página inicial muito mais performático, uma vez que as imagens deixam
de ser um problema no carregamento.

### Página de Contatos

Algum tempo atrás escrevi um artigo sobre [como fazer um formulário no Jekyll usando Vue.js](https://www.rossener.com/fazendo-um-formulario-de-contato-no-jekyll-com-vue.js/),
muito bem, incorporei o formulário no tema. É só colocar seu e-mail no `_config.xml` e as pessoas
já podem falar com você.

![Imagem do formulário de Contato](http://res.cloudinary.com/dm7h7e8xj/image/upload/v1506081939/formulario-contato_qrevrz.png)

### Live Search

Essa funcionalidade não foi desenvolvida por mim, mas vale a pena destacar aqui. No canto
superior direito do Jekflix você vai encontrar um ícone de busca.

É muito simples, digitou uma palavra que está no título, em alguma tag ou na categoria do artigo
e ele te retorna o resultado na hora.

![Imagem dos resultados de busca](http://res.cloudinary.com/dm7h7e8xj/image/upload/v1506082152/busca_tpvjxc.png)

### Outras *features* legais

Poderia especificar as outras funcionalidades aqui, mas preferi fazer um resumo
das que achei mais legais.

Segue uma lista de outras funcionalidades que você vai encontrar nesse tema, caso deseje
utilizá-lo:

- Gulp
- Stylus
- Emojis
- Push Menu
- Ícones em SVG
- Script para a criação de rascunhos e artigos
- Página de Tags
- Página de About
- Feed RSS
- Sitemap.xml
- Disqus para os comentários
- Google Analytics

## Instalação

Se quiser colocar esse tema no seu blog Jekyll é só seguir as instruções [nesse link](https://github.com/thiagorossener/jekflix-template/#setup).

**Observação:** Não se esqueça de rodar um `sudo gulp` para ele compilar os assets.

## Conclusão

Muito obrigado por ler o artigo até aqui, espero que tenha gostado do tema! Se gostou
mesmo e puder dar uma estrelinha lá no repositório é só [clicar aqui](https://github.com/thiagorossener/jekflix-template/), seria muito grato.

Esse tema é totalmente customizável, e o coloquei sob a licença MIT, ou seja, você
pode usá-lo e modificá-lo como quiser sem precisar me referenciar em qualquer lugar.

Mas, se quiser dar um alô de que está usando o tema, pode mandar um tweet para [@thiagorossener](https://twitter.com/thiagorossener) e ficarei feliz de saber que gostou do tema.

Tem alguma sugestão de melhoria? Encontrou algum *bug*? Deixa nos comentários aqui
embaixo ou submete um *pull request* lá no [repositório](https://github.com/thiagorossener/jekflix-template/) e a gente discute isso juntos!
