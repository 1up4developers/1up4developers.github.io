---
author: rogerbarreto
comments: true
date: 2008-08-18 17:02:43+00:00
layout: post
slug: tpw-colocando-dicas-em-pratica
title: TPW - Colocando dicas em prática
wordpress_id: 91
categories:
- real world
tags:
- pragmatic waterfall
- ruby
---

Depois de ler [The Pragmatic Programmer](http://1up4dev.org/2008/05/the-pragmatic-programmer-no-ambiente-waterfall-e-claro/) é natural ficar empolgado, bom, uma prova disso foi meu próprio post sobre o assunto. Depois de ter "digerido" o livro, lancei as [dicas para o desenvolvedor](http://1up4dev.org/2008/06/tpw-dicas-para-o-desenvolvedor/), acabei inventando um termo legal _The Pragmatic Waterfall_, que resolvi transformá-lo numa série.

A primeira dica do "dicas para o desenvolvedor", foi:
- Gerador de código descartável.

Pois bem, hoje, vou mostrar um exemplo de "código descartável" que utilizei e deu certo ! :D

Problema: No projeto que estou existem algumas queries em arquivos XML do hibernate, preciso convertê-las para Spring. Pra cada query do hibernate, precisei gerar dois arquivos de XML do Spring e mais uma classe Java que herda de SqlUpdate ... blá blá blá. Bom, os detalhes não importam muito, **o que importa é que vamos ler o XML do hibernate e gerar o que for necessário**, tudo isso com **Ruby**!

Antes de qualquer coisa, já aviso que ainda não domino totalmente Ruby, e o script que fiz é do tipo código "descartável", então não tive o menor cuidado em deixá-lo bonito, apenas funcional. Acho que a melhor maneira de aprender uma nova linguagem é escrevendo código com ela, não somente lendo sobre.

Exemplo super simplificado do XML do Hibernate:
script generator-bean em Ruby: 

É isto ! O script é simples e direto, deixei a saida pro console mesmo e para jogar em arquivo é muito simples:

$ ruby generator-bean.rb > spring-exemplo.xml

O script que coloquei é uma versão bem reduzida, já que a idéia do post é mostrar que é possível transformar tarefas chatas e trabalhosas em scripts semi-automáticos ! Ah, lembrando que com ele consegui terminar a minha tarefa em 2 dias, sendo que a previsão era quase uma semana de trabalho !

Precisa de mais informações de como ler XML com Ruby !?

Segue as referências.
Tirei deste [post](http://www.meupost.com/2008/07/10/lendo-xml-com-o-rexml-ruby/), muito legal por sinal.
* API do REXML: [http://www.ruby-doc.org/stdlib/libdoc/rexml/rdoc/index.html](http://www.ruby-doc.org/stdlib/libdoc/rexml/rdoc/index.html)
* Sobre o Electric XML: [http://www.xml.com/pub/r/1098](http://www.xml.com/pub/r/1098)
* Tutorial um pouco mais complexo sobre XML com Ruby: [http://www.xml.com/pub/a/2005/11/09/rexml-processing-xml-in-ruby.html](http://www.xml.com/pub/a/2005/11/09/rexml-processing-xml-in-ruby.html)

**OBS:** Estou usando o [gist.github.com](http://gist.github.com/) para colocar o xml e script de exemplo, provavelmente não vai aparecer no leitor de feed.
