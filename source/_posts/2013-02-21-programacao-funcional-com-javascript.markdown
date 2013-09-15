---
author: pbalduino
comments: true
date: 2013-02-21 15:58:02+00:00
layout: post
slug: programacao-funcional-com-javascript
title: Programação funcional com JavaScript
wordpress_id: 1280
categories:
- tutorial
tags:
- javascript
- programação funcional
---

> "JavaScript is LISP in C's clothing" - Douglas Crockford




## O que é programação funcional


Programação funcional é, assim como a orientação a objetos, uma forma de se pensar em como resolver problemas.

A base do que hoje é a programação funcional foi criada paralelamente por Alan Turing e Alonzo Church na década de 1930, antes mesmo da existência dos computadores como o conhecemos.

Infelizmente, a programação funcional passou muito tempo restrita aos meios acadêmicos, o que faz com que o iniciante no assunto fique assustado com toda aquela notação matemática e com os termos incompreensíveis.


    
    ω := λx.x x
    Ω := ω ω 
    Y := λg.(λx.g (x x)) (λx.g (x x))



Felizmente, o conceito de programação funcional é muito simples. Assim como na orientação a objetos a menor parte de um sistema é um objeto, você pode atribuir objetos a variáveis, pode passá-los por parâmetro e ter métodos retornando objetos, na programação funcional a menor parte do seu sistema é uma função.

Isso implica que você pode atribuir funções a variáveis, pode passá-las por parâmetro e mesmo fazer com que uma função retorne outra função. Alguma linguagens implementam também imutabilidade, que é quando todo valor é tratado como se fosse uma constante, e outros conceitos periféricos, o que não é o caso do JavaScript. Todos os demais conceitos de programação funcional derivam do fato de você lidar com funções como se fossem um valor como qualquer outro.


## High order functions


Uma high order function (não achei uma tradução decente para o Português), apesar do nome intimidador, é simplesmente uma função que recebe outra função como parâmetro ou devolve uma função como resultado. Quando você usa callbacks no JavaScript e no jQuery, você está fazendo uso de high order functions.

    
    $("button.mallandro")
      .click(function() {
        alert("Ié ié!");
      });


O método click é uma high order function, e a função anônima que faz _Ié ié!_ é um callback.

Mais para frente vou mostrar mais formas de usar high order functions.


## Escopo


Apesar do conceito de escopo não ser exclusivo da programação funcional, é importantíssimo que você entenda como funciona o escopo no JavaScript para que não fique confuso ao ver _closures_ e _currying_.

Como na maioria das linguagens comerciais, uma variável declarada em um escopo maior é visível em um escopo menor, enquanto o contrário não é verdadeiro.

Na prática significa que uma variável global é visível por todo mundo:

    
    var x = 1;
    
    function foo() {
      console.log(x);
    }
    
    foo();
    
    // Saída:   1


Significa também que uma variável local só é vista dentro do escopo em que foi criada, mesmo que tenha o mesmo nome de uma variável global:


    
    
    var x = 1;
    
    function bar() {
      var x = 99;
      var y = 42
    
      console.log(x, y);
    }
    
    bar();
    // 99 42
    
    console.log(x);
    // 1
    
    console.log(y);
    // ReferenceError: y is not defined
    



Quando uma função altera o valor de uma variável global, isso afeta toda a aplicação. Por isso o uso de variáveis globais não é considerado uma boa prática. Porém, uma variável global passada por parâmetro para uma função não tem o seu valor alterado:


    
    
    var x = 1;
    var y = 11;
    
    function meh(x) {
      console.log("Dentro: ", x, y);
      x++;
      y++;
      console.log("Dentro: ", x, y);
    }
    
    meh(x);
    // Dentro:  1 11
    // Dentro:  2 12
    
    console.log("Fora: ", x, y);
    // Fora:  1 12
    





## Closures



Outra característica do escopo é que uma função guarda as variáveis do contexto em que foi criada. Isso significa que uma função pode continuar acessando variáveis que só existiam no momento em que ela foi criada.


    
    
    function counter() {
      var x = 0;
    
      return function() {
        return ++x;
      }
    }
    
    var count = counter();
    
    console.log(count());
    // 1
    console.log(count());
    // 2
    console.log(count());
    // 3
    console.log(count());
    // 4
    console.log(x);
    // ReferenceError: x is not defined
    



O que acontece aqui é que count recebe uma função que incrementa o valor da variável x, só que essa variável existe apenas dentro da função counter. O que aconteceu aqui é que a função armazenada em count _se lembra_ da variável que foi criada em outra função, mas que não está mais sendo executada.

Ao tentarmos exibir o valor da variável x, recebemos um erro, pois ela não existe no escopo global.



## Currying



Juntando tudo o que vimos até aqui sobre high order functions, escopo e closures, chegamos ao _currying_. Currying é uma operação em que você transforma uma função que receberia mais de um parâmetro em uma série de chamadas de funções com apenas um parâmetro cada. 

Um dos usos dessa técnica é evitar, de forma elegante, que o mesmo parâmetro seja passado para a mesma função.

Vamos pegar um exemplo escrito de forma imperativa. Temos uma função hey, que recebe os parâmetros texto e nome e, a partir disso, imprime uma saudação.


    
    
    function hey(texto, nome) {
      console.log(texto + ", " + nome);
    }
    
    hey("Bom dia", "João");
    // Bom dia, João
    
    hey("Bom dia", "José");
    // Bom dia, José
    
    hey("Bom dia", "Nicolau");
    // Bom dia, Nicolau
    



Você pode dizer que poderíamos guardar "Bom dia" em uma variável. Concordo, mas isso não mudaria nada dentro do que estamos apresentando.

Reescrevendo a mesma função usando currying, teremos o código abaixo:


    
    
    function hey(texto) {
      return function(nome) {
        console.log(texto + ", " + nome);
      }
    }
    
    var bomDia = hey("Bom dia");
    
    bomDia("João");
    // Bom dia, João
    
    bomDia("José");
    // Bom dia, José
    
    bomDia("Nicolau");
    // Bom dia, Nicolau
    



Programação funcional é muito mais do que os conceitos que apresentei aqui, mas como um primeiro contato já dá para fazer muita coisa bacana.

Recomendo que você estude e pesquise a respeito. Mesmo que você não use uma linguagem funcional no seu dia-a-dia, o fato de conhecer um novo modo de pensar acaba alterando a forma como você resolve problemas, fazendo com que você tenha idéias melhores e mais elegantes.

**Update em 26/02:**

Este artigo foi publicado também no iMasters, no endereço [http://imasters.com.br/front-end/javascript/programacao-funcional-com-javascript/](http://imasters.com.br/front-end/javascript/programacao-funcional-com-javascript/).

_Esse texto é parte do livro [Dominando JavaScript com jQuery](http://www.casadocodigo.com.br/products/livro-javascript-jquery), publicado pela [Editora Casa do Código](http://www.casadocodigo.com.br)_.
