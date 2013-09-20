---
author: rogerbarreto
comments: true
date: 2009-07-21 09:45:58+00:00
layout: post
slug: cuidado-com-casos-de-uso
title: Cuidado com Casos de Uso
wordpress_id: 516
categories:
- real world
---

Na Engenharia de Software, um caso de uso (ou _use case_) é um tipo de classificador representando uma unidade funcional coerente provida pelo sistema, subsistema, ou classe manifestada por seqüências de mensagens intercambiáveis entre os sistemas e um ou mais atores. Pode ser representado por uma elipse contendo, internamente, o nome do caso de uso. (fonte: [Wikipédia](http://pt.wikipedia.org/wiki/Caso_de_uso))

A própria explicação do Caso de Uso demonstra o que costumam ser na prática, ou seja, um monte de _buzzwords_ para enganar o usuário e levar a sua assinatura. Este por sinal, só é citado no segundo parágrafo...


## É uma cilada Bino!!!


[![](/images/uploads/2009/07/cilada_bino.jpg)](http://pt.wikipedia.org/wiki/Carga_Pesada)

Pontos negativos que podem tornar numa cilada:




  * Não tem público definido. Casos de Uso são feitos para os analistas, desenvolvedores, testadores e as vezes para o usuário.


  * Casos de Uso não compilam. O usuário quer um sistema e não papéis para assinar.


  * Não traz o famoso ROI, traduzindo, retorno de investimento. Já vi muitos projetos de "anos em análise", e no final, tinham uma bíblia inútil junto com um grande rombo nas contas.


  * Difícil de manter atualizado. É natural que as coisas mudem, e a cada mudança ter que atualizar quilos de documentos, não é prático e muito caro.


  * Pelo fato de terem um público abrangente, é consumido muito tempo com detalhes inúteis, como diagramas de "tudo", que não ajudam em nada no desenvolvimento. Mais um item que consome tempo, recurso e muito dinheiro.


A lista pode continuar fácil, mas devemos ir para a parte que interessa:


## Como corresponder a expectativa do usuário?


Vou colocar o que está dando certo para mim. Caso encontrem alguma semelhança com o capítulo 7 do [Pragmatic Programmer](http://www.pragprog.com/titles/tpp/the-pragmatic-programmer), não estranhem, foi de lá mesmo que eu me "inspirei".

Nada melhor do que a dica 51 (uma boa idéia!) para começar:


_Don't gather requirements -- Dig for them_



Numa tradução livre e tosca, colocaria assim:


Não reuna requisitos, cave-os!







  * Este negócio de "cavar", é algo como: descobrir por que o usuário faz, e não somente como. Descobrindo o por que, você consegue sugerir maneiras diferentes de como fazer. Isso nos leva a próxima dica.


  * Lembre-se que, requisitos não são arquitetura, nem design, muito menos interface do usuário. Requisitos são necessidades!


  * Não seja escravo de nenhuma anotação. Use o melhor método que se comunica com o seu público. No meu caso, papel e caneta, com rascunhos da futura tela, já está funcionando e bem.


  * Não ter medo de sugerir novas idéias. Acredito que se dependesse da maioria dos usuários, todos os sistemas teriam cara de Excel! :D Com o porque em mente, fica mais fácil sugerir funcionalidades como filtros, layout etc.


  * _Some things are better done, than described._ -> Algumas coisas são melhores feitas do que descritas. Pra mim, esta dica tem funcionado com reformulação de telas, algum "filtro maluco" etc.


Sei que este post é muito "abstrato", rápido e sem referência nenhuma, mas a idéia em si é causar uma reflexão em ti, sobre o modo que tu trabalha. Hoje, ele é prático? o cliente está satisfeito com resultado? o cliente está satisfeito com a velocidade, desde a análise a concepção? está havendo desperdício de verba?

Estas perguntas são interessantes, e sempre devem ser feitas. Por mais que o software seja problemático, se conseguirmos corresponder a expectativa do usuário, teremos mais um cliente satisfeito na carteira.

O tema Caso de Uso, é polêmico, e não quero causar _flame_. Pra cada ambiente, equipe, produto/projeto existe uma maneira diferente de trabalhar. O que espero evitar, são aqueles papéis inúteis, com: "se a ação teve sucesso, deve mostrar ok" ... bah!

Por sinal, um dos [cinco grandes erros não técnicos, cometidos por programadores](http://rafael.adm.br/p/os-top-cinco-erros-nao-tecnicos-cometidos-por-desenvolvedores/) ([original aqui](http://www.makinggoodsoftware.com/2009/07/07/5-top-non-technical-mistakes-made-by-programmers/)) é: Esquecer do usuário. Lembre disto! ;)
