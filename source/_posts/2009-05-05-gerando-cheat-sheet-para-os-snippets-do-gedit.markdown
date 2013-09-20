---
author: rogerbarreto
comments: true
date: 2009-05-05 08:00:37+00:00
layout: post
slug: gerando-cheat-sheet-para-os-snippets-do-gedit
title: Gerando cheat sheet para os snippets do gedit
wordpress_id: 482
categories:
- projetos
tags:
- gem
- ruby
---

Acredito que a maioria de vocês conhecem e/ou já usaram as famosas _cheat sheets_ (tradução para "cola"?) para algo. Costumo usá-las quando quero fixar algum conceito novo ou simplesmente para consultas rápidas. Com o pai Google, é possível encontrar os mais diversos tipos: [HTML](http://www.google.com.br/search?q=cheat+sheet+HTML&ie=utf-8&oe=utf-8&aq=t&rls=com.ubuntu:en-US:unofficial&client=firefox-a), [Ruby](http://www.google.com.br/search?hl=pt-BR&client=firefox-a&rls=com.ubuntu%3Aen-US%3Aunofficial&hs=Ejh&q=cheat+sheet+Ruby&btnG=Pesquisar&meta=), [Ruby on Rails](http://www.google.com.br/search?hl=pt-BR&client=firefox-a&rls=com.ubuntu%3Aen-US%3Aunofficial&hs=WQ2&q=cheat+sheet+Ruby+on+Rails&btnG=Pesquisar&meta=), [Shell Script](http://www.google.com.br/search?hl=pt-BR&client=firefox-a&rls=com.ubuntu%3Aen-US%3Aunofficial&hs=h7M&q=cheat+sheet+Shell+Script&btnG=Pesquisar&meta=)... etc.

Atualmente venho praticando Ruby, sendo o gedit - "tunado" com vários plugins - o meu maior aliado. Um dos plugins que mais me ajuda é o [Snippets](http://live.gnome.org/Gedit/Plugins/Snippets) e neste [post do Cássio Marques](http://cassiomarques.wordpress.com/2008/09/16/gedit-snippets/), você encontra uma breve descrição do que é, e um link para snippets de exemplo.

Tudo isso foi só pra contar o que me levou a criar a gem gedit-snippets-tool. Com ela é possível criar _cheat sheets_ dos seus snippets. Supondo que você tenha ruby e rubygem instalados, seu modo de uso é muito simples. Para instalar a gem execute:


{% codeblock lang:bash %}
sudo gem install rogerleite-gedit-snippets-tool -s http://gems.github.com
{% endcodeblock %}


Após a instalação, para gerar um _cheat sheet_ com todos os seus snippets, execute:


{% codeblock lang:bash %}
gedit-snippets-tool -cs > ~/mycheatsheet.xhtml
{% endcodeblock %}


Caso tenha muitos snippets, e deseja criar um cheat sheet somente com Ruby e Ruby on Rails por exemplo, execute:


{% codeblock %}
gedit-snippets-tool -cs ruby* > ~/mycheatsheet.xhtml
{% endcodeblock %}


[![Meu cheat sheet de exemplo](/images/uploads/2009/04/cheatsheet-example-291x300.png)](/images/uploads/2009/04/cheatsheet-example.png)

Levando em conta que a gem foi feita em três dias e focamos somente o necessário para lançarmos uma versão 0.x "em produção", vamos as limitações:




  * O gedit-snippets-tool lê os snippets (arquivos XML) que estão na pasta "home". Constatei que o gedit "limpo" logo após habilitar o plugin Snippets, guarda os snippets na pasta "/usr/share/gedit-2/plugins/snippets/". Bom, se for este o seu caso, peço a gentileza de copiá-los para a "home" em "{home-folder}/.gnome2/gedit/snippets/".


  * O template usado para gerar a página xhtml está bem "rústico". O lado bom disso é que está bem fácil de alterá-lo. Vejam o código do template (via gist, leitores de RSS, sorry):




Quem tiver sugestões (**inclusive** de template), podem forkar o projeto ou se quiser, deixe um comentário que eu entro em contato.


### Sobre o Desenvolvimento


Assim que tive a idéia, análisei o que seria necessário, e resumindo:




  * Ler os XMLs dos snippets;


  * Uma engine para gerar páginas através de "templates";


  * Fazer uma gem executável... pois assim é mais fácil e rápido para quem quiser usá-la;


A parte do XML é fácil, pois até já fiz coisa parecida [antes](http://1up4dev.org/2008/08/tpw-colocando-dicas-em-pratica/). A parte da engine para templates, o pai Google me guiou a uma ótima, chamada [Erubis](http://www.kuwata-lab.com/erubis/), que é uma gem e torna as coisas muito mais fáceis.

Para criar o esqueleto da gem, usei o [gemhub](http://github.com/dcrec1/gemhub) do [Diego Carrion](http://www.mouseoverstudio.com/blog/2008/10/27/criando-gems-com-gemhub-nunca-foi-tao-simples/), que facilitou muito o meu trabalho, sem contar a ajuda que me deu na hora de publicar o gemspec no github... valeu truta! Bom, o resto foram três noites programando e aprendendo a fazer a minha primeira gem. Quem quiser participar estão todos convidados a "forka-lo" no [github](http://github.com/rogerleite/gedit-snippets-tool).
