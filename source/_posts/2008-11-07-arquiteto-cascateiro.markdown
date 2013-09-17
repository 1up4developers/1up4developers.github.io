---
author: rogerbarreto
comments: true
date: 2008-11-07 09:44:44+00:00
layout: post
slug: arquiteto-cascateiro
title: Arquiteto Cascateiro
wordpress_id: 210
categories:
- cascata
- real world
- waterfall
tags:
- dicas
- pragmatic waterfall
---

> Este post é uma homenagem aos Arquitetos defensores do _waterfall_/cascata.


Recentemente tive o desprazer de conhecer um arquiteto, é isso mesmo, aquele com certificado e tudo, com direito a broche da Sun em seu terninho. Aliás, certificado é um tema polêmico que eu não tenho uma opinião muito certa e/ou formada... bom, vou deixar esta parte para um próximo post, quem sabe.

Voltando ao assunto, hoje no fretado, comecei a pensar nas semelhanças que um arquiteto de sistemas (certificado que decorou patterns inutéis da Sun) tem com um arquiteto de obras. Só para deixar claro, na tabela abaixo estou usando dois estados: _FAIL_ e Ok. Fail quer dizer que vai dá merda não vai dar certo e não tem jeito, caso queira uma definição mais formal, o [wikipédia](http://en.wikipedia.org/wiki/Failure) ajuda, agora se você prefere imagens, o [Fail Blog](http://www.failblog.net/) também serve.

[caption id="attachment_235" align="aligncenter" width="300" caption="Exemplo de FAIL"][![Exemplo de FAIL](/images/uploads/2008/11/soccer_fail-300x201.jpg)](/images/uploads/2008/11/soccer_fail.jpg)[/caption]







Objetivos
Cascateiro
De Obras





Colocam as futuras "obras" no papel antes de começar.


FAIL


OK






Ainda no papel, colocam **todas** as necessidades do cliente, do início ao fim.


FAIL


OK






O cliente do Arq. de Obras sabe que depois que começar não pode mudar.


FAIL


OK






O arquiteto de Obras **não** define quais tipos de blocos, cimento e ferro a obra vai usar, o Cascateiro **sim**.


FAIL


OK



Parei a tabela por aqui pois já dá pra saber que o FAIL tende a infinito né.
Pergunta: o que ambos arquitetos estão fazendo!?!
Resposta educada: Estão **fechando o escopo** do projeto.

[caption id="attachment_220" align="alignleft" width="227" caption="Arquiteto Cascateiro trabalhando ..."][![Arquiteto Cascateiro trabalhando ...](/images/uploads/2008/11/construcao-crea-300x225.jpg)](/images/uploads/2008/11/construcao-crea.jpg)[/caption]

A resposta acima é uma frase chave pra você ter certeza que vive num projeto waterfall cascateiro. Fechar o escopo do projeto inteiro deve ser muito bom para o arquiteto de obras, já para um sistema, o efeito é contrário. Acredito muito na teoria que [desenvolver software não é construir prédios](http://gc.blog.br/2008/07/20/cuidando-para-que-o-software-nao-apodreca/). Livros de renome como [Pragmatic Programmer](http://1up4dev.org/2008/05/the-pragmatic-programmer-no-ambiente-waterfall-e-claro/) citam isso.

Sei que este tema de construção civil já está batido. Comecei a escrever este post ao mesmo tempo que o Sr. Panachi publicou o [anterior](http://1up4dev.org/2008/10/software-e-sobre-investimento/), e com a idéia de ficar menos repetitivo, já vou _linkar_ as sugestões dos nossos incríveis leitores:



	
  * O [Diego Carrion](http://www.mouseoverstudio.com/blog/) (grande peruano! :D) cita este [_link_](http://agiletips.blogspot.com/2008/07/agile-bridge-analogy.html), que fala que a engenharia civil também consegue ser ágil em alguns casos.

	
  * O [Witaro](http://witaro.wordpress.com/), fez um ótimo post "[Desenvolvendo software como uma Rock Band](http://witaro.wordpress.com/2008/08/11/desenvolvendo-software-como-uma-rock-band/)" que quebra a barreira da analogia com a engenharia civil. Cara, continue escrevendo, porque a sua visão é muito legal!


Bom, agora que acabou o desabafo, vamos as possíveis soluções. O que fazer com o Arquiteto Cascateiro?

Acho que a primeira coisa seria conscientizá-lo de que ele não é o [Oscar Niemeyer](http://pt.wikipedia.org/wiki/Oscar_Niemeyer) e que a primeira versão de seu software nunca será completa de uma vez. Você deve conversar sobre iterações com ele e mostrar que o software deve evoluir conforme o cliente também evolui nas descobertas das suas reais necessidades. Sei que o post já está cheio de _links_, mas este post do Phillip Calçado, [Analista Pedreiro](http://blog.fragmental.com.br/2008/08/09/analista-pedreiro/), resume bem o que quero dizer.

**Arquiteto**, este nome ou termo ou cargo ou seja-lá-o-que-for, é coisa de modelo _waterfall_/cascata. Numa equipe, não deve haver distinção desta maneira. Todos programam, modelam, configuram, trabalham no Banco de Dados quando necessário, ou seja, ninguém deve exercer um papel único. Papéis únicos, representam [Guardiões](http://1up4dev.org/2008/11/os-guardioes-da-cascata/) que defendem somente seus interesses e não trabalham em pró da equipe/cliente/projeto.

O Arquiteto deve programar, colocar a mão na massa, assim como toda a equipe, pois UML, Caso de Uso, Diagrama de Sequência, etc. **sempre compilam**! Muito diferente na vida real, onde muitas vezes você é obrigado a implementar uma coisa diferente e torta para acompanhar estes documentos cascateiros. Caso você seja obrigado a gerar a documentação fútil acima, pense em algo que seja automatizado após você ter programado e testado, com certeza você será umas cinco vezes mais produtivo.

E por último e não menos importante, a equipe (inclusive o Arquiteto) tem que conhecer o negócio que implementa. Quando se inicia um novo projeto ou até mesmo decidem reestruturar um existente, o arquiteto cascateiro sempre prioriza novas tecnologias e frameworks, o que na maioria das vezes, não é necessário. Novos projetos ou _refactoring_ em existentes, devem ter um único prioritário objetivo: [KISS](http://pt.wikipedia.org/wiki/Keep_it_Simple_Stupid). Com esta prioridade em mente, novas tecnologias e frameworks serão escolhidos naturalmente, e não somente usar porque é a última moda no estilo SunTechDays.

E vocês leitores?! Sofrem ou já sofreram muito com Arquitetos Cascateiros!?!
