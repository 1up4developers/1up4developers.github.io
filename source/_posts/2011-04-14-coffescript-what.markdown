---
author: pbalduino
comments: true
date: 2011-04-14 13:20:26+00:00
layout: post
slug: coffescript-what
title: CoffeeScript quem?
wordpress_id: 934
categories:
- novidades da semana
- quick tips
- rails
- ruby
- web
tags:
- coffee script
- coffeescript
- javascript
- rails
- ruby on rails
---

Muito buzz se formou depois que o DHH tornou público que a versão 3.1 do Rails virá com CofeeScript por padrão.

Aproveitando a barulheira, que é comum a cada vez que o criador da plataforma abre a boca, vamos nos ater ao que é importante e apresentar uma introdução ao resumo simplificado do CoffeeScript básico.

Confesso que minha opinião sobre essa ferramenta mudou nos primeiros instantes em que comecei a usar. No site do CoffeeScript (ok se eu começar a chamar de CS daqui para frente?) existe um link onde você digita o código do lado esquerdo e, imediatamente, do lado direito aparece o equivalente em JavaScript.

Fiz uma brincadeira lá usando um código que sempre me vem à cabeça ao tentar explicar o básico de programação funcional para alguem. Segue:

    
    mimimi = (operation, value) ->
       operation(value);
    
    dobro = (value) ->
      value + value;
    
    quadrado = (value) ->
      value * value;
    
    zero = (value) ->
      value - value;
    
    um = (value) ->
      value / value;
    
    alert(mimimi(dobro, 3));
    alert(mimimi(quadrado, 3));
    alert(mimimi(zero, 3));
    alert(mimimi(um, 3));


Nada de outro mundo, certo? O equivalente JS é o código abaixo:

    
    var dobro, mimimi, quadrado, um, zero;
    mimimi = function(operation, value) {
      return operation(value);
    };
    dobro = function(value) {
      return value + value;
    };
    quadrado = function(value) {
      return value * value;
    };
    zero = function(value) {
      return value - value;
    };
    um = function(value) {
      return value / value;
    };
    alert(mimimi(dobro, 3));
    alert(mimimi(quadrado, 3));
    alert(mimimi(zero, 3));
    alert(mimimi(um, 3));


CS permite também a checagem automática de valores e parâmetros opcionais, conforme o exemplo retirado do site:

    
    fill = (container, liquid = "coffee") ->
      "Filling the #{container} with #{liquid}..."


Ou o equivalente em JS:

    
    var fill;
    fill = function(container, liquid) {
      if (liquid == null) {
        liquid = "coffee";
      }
      return "Filling the " + container + " with " + liquid + "...";
    };


Sem dúvida, muito mais legível e expressivo.

É uma ferramenta que eu pretendo usar em algum projeto pequeno, para testar e ver o quanto acelera meu trabalho. Em projetos maiores talvez eu demore um pouco mais para usar mas, considerando que umas das minhas aplicações já conta com 79% de código total escrito em JavaScript, segundo o GitHub, imagino que isso vai melhorar consideravelmente a manutenção do código.

E no mais, se o CS não te chamou a atenção e você não quer mesmo utilizar, basta remover o require do arquivo Gemfiles e a vida segue como se nada tivesse acontecido.

Links recomendados:



	
  * Site do CoffeScript: [http://jashkenas.github.com/coffee-script/](http://jashkenas.github.com/coffee-script/)

	
  * Mais do mesmo: [http://www.rubyinside.com/rails-3-1-adopts-coffeescript-jquery-sass-and-controversy-4669.html](http://www.rubyinside.com/rails-3-1-adopts-coffeescript-jquery-sass-and-controversy-4669.html)

	
  * Exemplo bacana de como utilizar CS dentro da sua view: [http://geekiriki.blogspot.com/2010/08/jquery-meets-coffeescript.html](http://geekiriki.blogspot.com/2010/08/jquery-meets-coffeescript.html)


