---
author: rogerbarreto
comments: true
date: 2012-06-25 10:05:09+00:00
layout: post
slug: concepcao-do-rologames
title: Concepção do RoloGames
wordpress_id: 1184
categories:
- marketing
- rails
- real world
- ruby
- web
tags:
- desenvolvimento de site
- playstation 3
- ps3
- rede social. troca de jogos
- rologames
---

Este post é um tópico da [Experiência de lançar o RoloGames](http://1up4dev.org/2012/06/a-experiencia-de-lancar-o-rologames/).


## Quando surgiu a idéia


Como jogador, nunca achei um serviço legal pra trocar jogos usados. Haviam poucas opções e normalmente as trocas eram feitas via fórum (UOL Jogos, por exemplo). Na época minha filha tinha acabado de nascer, eu estava num emprego novo e acabei congelando a idéia. Recentemente surgiram alguns sites de trocas, tentei usar mas não gostei, pois eram muito "abertos" e mais aborrecia do que gerava oportunidade real. Sofrendo com isso que o Panachi e eu bolamos o [RoloGames](http://rologames.com.br), onde a troca deve ser sempre 1 por 1 e o site só te avisa se o _match _for exato.


## Definindo um mantra


[![A Arte do Começo - The Art of Start](/images/uploads/2012/06/kawasaki-sm.jpg)](http://www.youtube.com/watch?v=VKhEg79xLio)

Totalmente baseado nas dicas do Sr. Kawasaki em [A Arte do Começo](http://www.youtube.com/watch?v=VKhEg79xLio), definimos uma filosofia para o site. Lembrando que se mantivermos esta filosofia sempre em mente, o site não perderá seu foco:




  * **Oportunidades confiáveis**. Queremos que o usuário encontre somente as oportunidades de troca que **façam sentido** a ele, e não receba spams que infortunam a vida ou propostas _sem noção_ (por exemplo, oferecerem um jogo antigo em troca de um lançamento).


  * **Toda troca deve ser justa**. Infelizmente, é uma prática considerada normal a troca de 3 jogos por 1, onde pessoas ganham dinheiro em cima de jogadores. As propostas e trocas são sempre de um jogo por outro, de acordo com os desejos e ofertas dos usuários.


  * **Fácil de usar**. O site não deve atrapalhar a vida do usuário com mensagens que ele não queira receber ou oportunidades que não façam sentido. É claro que a velocidade de navegação do site conta neste quesito.


  * **Social**. Os usuários podem acompanhar a atividade de seus amigos, enviar mensagens diretas e ter a escolha de trocar os jogos somente com quem quiser.




## Provas de Conceitos


Antes de encostar no código, o Panachi e eu definimos o objetivo do site e algumas premissas para começar o projeto, e a mais importante era: ter um banco de dados de jogos. Definimos somente as informações necessárias de um jogo, e partimos para uma prova de conceito. Em dois dias conseguimos montar um banco de dados bem completo, e a nossa solução foi simplesmente um [crawler](http://1up4dev.org/2011/01/criando-um-webcrawler-de-modo-facil-e-rapido-com-ruby-e-nokogiri/) de jogos! Por sinal, foi tão produtivo que nossa outra prova de conceito originou o [psn_trophies](https://github.com/rogerleite/psn_trophies).


## Interface no papel


Em paralelo às provas de conceito, começamos a rascunhar as telas do site. O processo foi bem simples, uma pilha de sulfite e lápis (na falta de uma caneta [Sharpie](http://37signals.com/svn/posts/466-sketching-with-a-sharpie)). A partir dos esboços, começamos a definir quais funcionalidades o site teria. Em seguidas priorizamos as mais importantes e fechamos o escopo da primeira versão. Durante o desenvolvimento, muita coisa mudou no layout, mas os "rabiscos" da concepção foram a essência de tudo.


## Início do desenvolvimento


Já tinhamos uma prova de conceito, uns rascunhos das páginas e uma lista de funcionalidades. E todo processo levou cerca de uma semana.

Sem perder mais tempo, partimos para o código! E foi então que começamos a tropeçar nos detalhes... mas este será um assunto para o próximo post. Até lá!
