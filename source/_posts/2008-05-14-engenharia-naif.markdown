---
author: hbulhoes
comments: true
date: 2008-05-14 02:36:00+00:00
layout: post
slug: engenharia-naif
title: Engenharia naïf
wordpress_id: 12
categories:
- questionamento
tags:
- devaneios
---

-- Inaugurando a minha participação no blog --

Sabe aqueles quadros que costumam vender na rua? Esses de feirinha, que são verdadeiros clássicos: casas toscas num gramado, cenários praianos, [coisas deste naipe](http://shoe-me.blogspot.com/2008_01_01_archive.html#3365084320162022222). São parte do que é chamado de [arte naïf](http://pt.wikipedia.org/wiki/Arte_na%C3%AFf): obras não necessariamente intelectuais ou acadêmicas. Que podem até ser tecnicamente bem-feitas, mas dificilmente vão transformar seus autores em Van Goghs, mesmo que eles arranquem as próprias orelhas com os dentes.

Lembrei dessas coisas no mês passado, quando eu estava a ponto de precisar desenvolver um parser para uma certa linguagem de programação antiga. O parser seria um pedaço de uma ferramenta que extrai informações sintáticas e estruturais de um programa a partir do seu fonte -- coisa que qualquer IDE atual disponibiliza quando se faz refactoring ou checagem de referências e dependências. Mas, como a tal linguagem não tem exatamente um IDE, a ferramenta (e o parser) tem que ser desenvolvidos "do zero". E lá estava eu estudando as necessidades do meu trabalho.

Em pouco tempo concluí (instintivamente, não objetivamente) que o ideal era construir um mecanismo de parsing genérico, que servisse para qualquer linguagem. Entendi que esse mecanismo era possível de existir, desde que o léxico (sintaxe para definir sintaxes) fosse potente. Foi divertido então definir um léxico que permitisse uma estruturação a la [BNF](http://en.wikipedia.org/wiki/Backus-Naur_form). E de forma bastante ingênua parti para montar um protótipo de parser. Um protótipo que entendesse uma linguagem simples. Um parser de XPath, por exemplo, só pra começar...

Foi aí que eu vi que ainda preciso comer muito arroz e feijão! O fato é que eu nunca precisei fazer um compilador na faculdade (ha!), e portanto não sabia que o buraco é muito mais embaixo. Existe um monte de formas de se [construir um parser](http://en.wikipedia.org/wiki/Parser_generator), uma porção de [ferramentas](http://en.wikipedia.org/wiki/Comparison_of_parser_generators), documentos, exemplos... e eu lá, escavando a pedra pra fazer a roda.

Pra mim foi interessante entender como esses mecanismos funcionam, e também foi bacana ter imaginado alguns algoritmos que, depois vi, são usados no ramo há muito tempo. Mas o ruim da história é ter entrado num caminho arriscado ao criar uma solução duvidosa, por pura ignorância. Nesse episódio eu fui um engenheiro naïf!

A minha "obra" até poderia ser passável tecnicamente, mas sem dúvida eu deixaria uma série de defeitos para trás, que qualquer um mais capacitado iria bater o olho e pensar: mas quem fez esta merda? Que é mais ou menos o que eu penso quando vejo uma ou outra pog alheia...

Seu código se sentiria à vontade na feirinha hippie?
