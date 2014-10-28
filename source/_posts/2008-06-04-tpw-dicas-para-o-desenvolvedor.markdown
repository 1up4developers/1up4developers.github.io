---
author: rogerbarreto
comments: true
date: 2008-06-04 12:30:00+00:00
layout: post
slug: tpw-dicas-para-o-desenvolvedor
title: TPW - Dicas para o Desenvolvedor
wordpress_id: 35
categories:
- cascata
- processos
- waterfall
tags:
- dicas
- pragmatic waterfall
---

[![](http://bp3.blogger.com/_XL8FQmVF9qY/SDya_OnB-kI/AAAAAAAAAGA/PNvOGr9fz1A/s320/dilbert2733310071001.gif)](http://bp3.blogger.com/_XL8FQmVF9qY/SDya_OnB-kI/AAAAAAAAAGA/PNvOGr9fz1A/s1600-h/dilbert2733310071001.gif)




Tradução rápida do diálogo:







Chefe (com chifrinhos): Por que você levou seis meses para completar esta simples tarefa ?




Dilbert: Por causa das suas mudanças contínuas, sua comunicação confusa e seu pequeno expediente de trabalho.




Chefe (com chifrinhos): Estou procurando por alguma coisa mais parecida como você sendo preguiçoso.



Hello hello hello ! Estas tirinhas do Dilbert estão de matar ultimamente. E já pra avisar, o TPW significa _The Pragmatic Waterfall_, um novo termo para o que buscamos aqui neste blog, ajudar quem sofre com o _Waterfall_ ! Ok, isso inclui a mim mesmo.

Quem vive num malditodigno ambiente Waterfall já deve ter vivenciado muito disso que ocorreu acima, o gerente "junior" procurando uma desculpa de porque o gant chart está vermelho para repassar ao gerente "pleno" que este repassará ao "senior junior" e que este repassará "senior pleno" ... bom, já entenderam até onde a desculpa vai chegar.

Na tentativa de transformar este post de "muro das lamentações" para Pragmatic Waterfall, vamos as dicas didáticas (ou seria um guia de sobrevivência?) de como tentar contornar este tipo de situação frustante:

[![](/images/uploads/2008/06/14-03-06_sharon.jpg?w=300)](/images/uploads/2008/06/14-03-06_sharon.jpg)==>>[![Waterfall Model](/images/uploads/2008/06/waterfall_model.png?w=300)](/images/uploads/2008/06/waterfall_model.png)



	
  * Gerador de código descartável. Sim, é isso mesmo que você leu, o gerador de código você já sabe o que é, agora o descartável é o que você deve estar imaginando. Isso não tem muito a ver com o Waterfall, mas todo projeto que trabalhei tem aquelas camadas que repassam chamadas e seguem um padrão comum. Logo, você não precisa - e nem deve - escrevê-los, para isto inventaram o computador. Se você tem sorte de usar um sistema unix like, se aventure com shell scripts, vale a pena, caso não tenha esta sorte ... err, primeiro sinto muito por ti ... mas você tem opção de linguagens de scripts como Perl, Python e Ruby em suas mãos, aproveite ! Crie estes scripts descartáveis e gere toneladas de código, seu chefe vai ficar feliz da vida com o aumento da produtividade.

	
  * Conheça todos os recursos que a sua IDE ou seu editor de texto oferecem. Isso parece ser uma dica besta, mas pode acreditar que não é, já vi muita gente usando um mesmo editor por mais de meses e ficam "abobados" ao descobrirem que o Ctrl+H abre a tela de Replace !!! Bom no meu caso que uso Eclipse o dia todo, as dicas são:

	
    * Aprenda a usar teclas de atalho, elas realmente aumentam a produtividade. _Cheat Sheets_ como [este](http://www.n0sl33p.org/dev/eclipse_keys.html), ajudam no processo de adaptação.

	
    * Use e abuse dos Templates, aquelas configurações chatas de xml que toda hora são necessárias, não perca tempo, crie um template para isso e seja feliz. Você pode criar também para aqueles métodos chatos com assinaturas iguais sempre (tipo do Struts mesmo sabe !?!) ... o céu é o limite !

	
    * Aprenda a usar as opções do menu Refactor e - adivinhe! - Geradores de Códigos.




	
  * Mantenha um _checklist_ de documentos a atualizar, aqueles do tipo, Requisitos, Casos de Uso, Arquitetura, Alteração, Instalação ... etc. Com um _checklist_ você não precisa ocupar a cabeça com este passo muito importante do _waterfall_ e lembre-se que neste ambiente o que é valorizado são os documentos e não as pessoas.

	
  * Antes de começar a sua nova tarefa que está no Gantt, descubra quem são os envolvidos, neste nosso ambiente temos especialistas para todos os lados, converse com eles e feche pactos, pois é muito comum no final da tarefa você descobrir que a procedure retornará um [tipo complexo](http://blogs.ittoolbox.com/oracle/guide/archives/learn-oracle-miscellaneous-user-defined-and-complex-types-17977) e não um cursor como você imaginava.

	
  * Sei que isso pode parecer irreal para você que está num _waterfall_ enraízado, porém, tente pelo menos. Tenha um ambiente [mock](http://en.wikipedia.org/wiki/Mock_object) para desenvolvimento, isso pode te salvar ao manter sua tarefa "verdinha" no Gantt Chart. Sim, todos nos [sabemos que o Gantt Chart não funciona](http://blog.aspercom.com.br/2007/11/15/ganttchartnaofunciona/trackback/), porém eu e você que estamos no _waterfall_ temos que usar e até fingir que [funciona](http://gc.blog.br/2007/12/04/cronogramas-nao-funcionam/).


Espero que com essas dicas você consiga se livrar de suas tarefas em menos tempo, e com o tempo que sobrar, aproveite para aprender coisas novas, metodologias novas e descobrir que existe vida fora do _waterfall_ ... e acredite ! Estão documentando, [aqui](http://blog.aspercom.com.br/2008/05/29/um-exemplo-a-ser-seguido/trackback/), [aqui](http://gc.blog.br/2008/05/27/como-estamos-indo-com-a-adocao-de-scrum-na-globocom/) e [aqui](http://blog.seatecnologia.com.br/articles/2008/05/29/experiencias-%C3%81geis-na-sea-episodio-i-%E2%80%93-a-ameaca-fantasma) !
