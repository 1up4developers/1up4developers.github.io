---
author: rodrigopanachi
comments: true
date: 2008-11-10 21:33:43+00:00
layout: post
slug: foco-no-problema
title: Foco no problema
wordpress_id: 243
categories:
- cascata
- real world
- waterfall
tags:
- agilidade
- dicas
- metodologia
- real world
---

Desenvolver software é uma atividade muito gratificante pois sempre podemos (ou deveríamos) exercitar nossa criatividade para solucionar os problemas dos clientes. Isto, apesar de divertido pode ser perigoso e/ou catastrófico se estivermos com o foco errado. Num ambiente cascateiro, onde cada envolvido está comprometido apenas com o processo e não se preocupa verdadeiramente com os problemas dos clientes, não é difícil que isto ocorra. Quase sempre o foco acaba sendo direcionado para a solução ao invés do problema.

Mas qual a diferença entre foco no **problema** ou **solução**? Vamos a um exemplo:


> Quando a Nasa enviou os primeiros astronautas ao espaço, descobriu que as canetas não funcionavam com gravidade zero. Para resolver esse problema, os engenheiros contrataram uma empresa especializada para projetar a caneta espacial.
Dez anos e US$ 12 milhões depois, estava pronta a caneta que podia ser usada no espaço, em qualquer posição. Nem a temperatura poderia atrapalhar: a supercaneta funcionava bem fizesse frio ou calor.
Os russos, que tiveram o mesmo problema, optaram por uma solução mais simples: passaram a usar um lápis.


[![](http://1up4dev.org/wp-content/uploads/2008/11/spaceball.gif)](http://1up4dev.org/wp-content/uploads/2008/11/spaceball.gif)A história acima é bem famosa e mesmo sendo [falsa](http://www.e-farsas.com/artigo.php?id=58), demonstra muito bem o que acontece quando o problema não está em foco. Neste caso, o problema é a impossibilidade de escrever em gravidade zero. Uma das soluções seria uma caneta que escreva nessas condições. Veja que aqui a **solução** já está em foco. Outra solução para o problema seria utilizar algo que escrevesse em gravidade zero: um pedaço de carvão ou um giz já serviriam. Assim, o problema seria resolvido.

Outro exemplo de falta de foco no problema é esta [história](http://blog.aspercom.com.br/2008/07/21/hierarquias-sao-inteligentes-nas-pontas/) da fábrica de pasta de dente, onde ocasionalmente algumas caixas da pasta de dente eram entregues vazias. Para eliminar este problema, a empresa gastou investiu milhões para garantir que durante a fabricação, nenhuma caixa ficasse sem o tubo de pasta de dente dentro. Mas o problema foi realmente resolvido depois que um operário deixou um ventilador soprando as caixas vazias para fora da esteira de produção. Simples não?

Na área de desenvolvimento de software não é tão raro acontecer algo parecido, onde o foco está inteiramente na solução. Sabe aquele sistema meio capenga, que funciona e dá dinheiro para empresa mas não é "web 2.0" nem utiliza conceitos de "SOA"? De repente a diretoria decide que este sistema deve ser "migrado" para uma tecnologia da moda mais atual, que o permita "evoluir" mais facilmente.

Para atender esta necessidade, normalmente uma equipe nova é contratada, toneladas de [documentos e diagramas](http://blog.fragmental.com.br/2008/07/25/uh-eme-ele/) são produzidos até que os programadores comecem a [codificar](http://www.martinfowler.com/bliki/CheaperTalentHypothesis.html). A esta altura, o prazo já está apertado e os "stakeholders" ainda não viram os resultados. Depois de muito tempo e dinheiro desperdiçados, um sistema feito às pressas, _bonitinho_ mas meia-boca, é entregue com os mesmos defeitos do anterior. E o problema não foi resolvido...

Desenvolver software deve ser um investimento [lucrativo](http://1up4dev.org/2008/10/software-e-sobre-investimento/), proporcionando algum ganho às partes envolvidas. Quando uma **necessidade** surgir, o primeiro passo é identificar o **problema** para então encontrar a melhor **solução**, ou seja, foco no problema. Neste exemplo da "migração", o problema é que a manutenção do software atual é muito cara, porém "migrar" o sistema inteiro não vai resolver o problema, no máximo criará um novo.

Mas de quem é a culpa quando o foco está na solução? Eu respondo: a **cascata**! Apesar das metodologias ágeis estarem em alta e aos poucos serem adotadas pelas empresas, a maldição do waterfall ainda é está entre nós. Clientes continuam com a mania de pedir tudo no início do projeto. Ao exporem seus problemas, já estão pensando na solução. Fazem questão de engordar o escopo com coisas das quais não têm certeza da utilidade, mas querem que estejam lá pois podem precisar um dia. Os desenvolvedores também não estão isentos dessa culpa. Um legítimo analista cascateiro não se envolve com os problemas do cliente, apenas ouvem suas solicitações e transformam em casos de uso ou diagramas. É aí que uma simples necessidade se transforma numa bola de neve e a lenda da caneta da Nasa se repete...

[![](http://1up4dev.org/wp-content/uploads/2008/11/software-300x225.jpg)](http://1up4dev.org/wp-content/uploads/2008/11/software.jpg)

Um verdadeiro desenvolvedor ágil deve se comprometer com o cliente, ouvir, entender e se envolver com suas necessidades para então sugerir uma solução simples, focada e que resolva o problema. Esta interação é muito importante e deve ser constante, pois o cliente passa a identificar **o que** realmente ele **precisa**, ou seja, o qual seu** problema**! Assim, começa a se concentrar em funcionalidades que realmente serão úteis e agregarão valor ao software e, consequentemente, ao negócio. Feedback é muito importante. O pessoal do Google sabe muito bem disso...
