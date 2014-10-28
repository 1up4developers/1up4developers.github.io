---
author: admin
comments: true
date: 2009-04-20 04:00:42+00:00
layout: post
slug: remarkable-jqgrid-no-rails-heroku-e-github-issues
title: Remarkable, jqGrid no Rails, Heroku e Github issues
wordpress_id: 412
categories:
- novidades da semana
- real world
- web
tags:
- jquey
- rails
- ruby
- ruby on rails
---

Este é o primeiro post que publicaremos semanalmente reunindo novidades sobre desenvolvimento, artigos, notícias e temas variados sobre Rails.


## [Remarkable 3.0](http://www.nomedojogo.com/2009/04/14/remarkable-30-is-out-and-its-well-remarkable/)


Foi lançada a nova versão do Remarkable, um framework para testes com RSpec. Dentre as novidades, estão:



	
  * I18n, possibilitando gerar o output das specs no seu idioma favorito fluente

	
  * Pending Macros, facilitando o agrupamento das specs pendentes

	
  * Macro stubs e mais opções de Matchers, simplificando testes com mocks


Para instalar basta um `sudo gem install remarkable_rails`

Saiba mais no site do [Carlos Brando](http://www.nomedojogo.com/2009/04/14/remarkable-30-is-out-and-its-well-remarkable/), lendo a [documentação](http://remarkable.rubyforge.org/rails/) ou espiando o código no [Github](http://github.com/carlosbrando/remarkable/tree/master).


## [jqGrid no Rails](http://www.2dconcept.com/jquery-grid-rails-plugin)


[jQuery](http://jquery.com/) é um dos frameworks Javascript mais populares do mercado. O que [esse cara](http://www.2dconcept.com/articles/8-jquery-grid-rails-plugin) fez foi juntar o [jqGrid](http://www.trirand.com/jqgrid35/jqgrid.html), que é um plugin muito bom para trabalhar com grids, num plugin para Rails, facilitando sua utilização.

Veja [aqui](http://www.2dconcept.com/jquery-grid-rails-plugin) um guia de utilização do plugin, a [aplicação de exemplo](http://github.com/ahe/jqgrid_demo_app/tree/master) ou confira o código no [Github](http://github.com/ahe/2dc_jqgrid/tree/master).


## [Heroku](http://heroku.com/)


Apesar da simplicidade do [Business Bingo Generator](http://1up4dev.org/2009/04/business-bingo-generator/), houve muitas "manhas" aprendidas no seu desenvolvimento. O Heroku foi uma delas, onde pudemos publicar rapidamente a aplicação. Se você quer hospedar um projeto simples feito em Rails, nós o recomendamos. O seu uso é muito simples e a incrível idéia de usar o git como interface para deploy é realmente sensacional. Basta instalar o client do Heroku, dar um "git push" e pronto: a aplicação está no ar!


## [Github Issue Tracker](http://github.com/blog/411-github-issue-tracker)


Nova funcionalidade no GitHub para facilitar nossas vidas, permitindo informar os "bugs" nos projetos. No link acima, contém o vídeo "I_ntroduction to GitHub Issues_" para mais detalhes.

Caso já use um Issue Tracker para o seu projeto no github, você pode desabilitá-lo na seção  do _Features _na aba_ Admin_.
