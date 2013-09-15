---
author: rogerbarreto
comments: true
date: 2011-02-24 03:52:40+00:00
layout: post
slug: documentacao-nao-pode-ser-chato-como-fazer-igual-ao-rails-guides
title: Documentação não pode ser chato. Como fazer igual ao Rails Guides !
wordpress_id: 878
categories:
- rails
- real world
- ruby
- web
tags:
- documentação
- rails
- rails guides
- ruby
---

> Documentação é aquilo que você reclama para fazer e reclama quando não encontra.


Por [Plínio](http://1up4dev.org/author/pbalduino/) "Chico Xavier" via [Twitter](http://twitter.com/#!/p_balduino/status/35689585455407105).

Isto me fez refletir (bem rápido) sobre as possíveis causas da não-documentação:



	
  * É muito chato ! Editar wiki, doc ... é incrível como toda documentação geralmente é feita em algo não produtivo.

	
  * Espaço e Tempo (ou seja, prazo!). Sempre é deixado por último e nunca sobra tempo pra ser feito.

	
  * Não é levado a sério. Levanta a mão que já viu a secretária documentando "tela" de sistema ... :D

	
  * Normalmente o "investidor" do software não vê valor nisto, ou melhor, espera alguém pedir e repassa a tarefa com prioridade "faz rapidinho".


Ok. Após reflexão feita, hora de atacar o problema.


## Usando "guides" para criar um Guide !


Este é um resultado da experiência abaixo:


[![Snapshot Guide (inspirado no Rails Guides)](http://1up4dev.org/wp-content/uploads/2011/02/snapshot_guide-300x241.png)](http://rogerleite.github.com/rubygems_snapshot/)


Com a gem [guides](https://github.com/wycats/guides), em segundos você tem um _scaffold_, pronto para ser preenchido.

    
    <span style="font-family: Georgia, 'Bitstream Charter', serif; color: #444444;"><span style="line-height: 22px;">$ gem install guides</span></span>



    
    <span style="font-family: Georgia, 'Bitstream Charter', serif; color: #444444;"><span style="line-height: 22px;"> </span></span><span style="color: #444444; font-family: Georgia, 'Bitstream Charter', serif; line-height: 22px;">$ guides #mostra os comandos disponíveis</span>



    
    <span style="font-family: Georgia, 'Bitstream Charter', serif; color: #444444;"><span style="line-height: 22px;">$ guides new sample</span></span>



    
    <span style="font-family: Georgia, 'Bitstream Charter', serif; color: #444444;"><span style="line-height: 22px;">$ cd sample</span></span>



    
    <span style="font-family: Georgia, 'Bitstream Charter', serif; color: #444444;"><span style="line-height: 22px;">$ guides build && guides preview</span></span>


Pronto. Em http://localhost:9292 você terá acesso ao novo guia gerado.


### Preenchendo o seu Guia


Em _guides.yml_ (exemplo [aqui](https://github.com/rogerleite/rubygems_snapshot/blob/master/guides/guides.yml)), está o básico do seu Guia. Nele está o índice de "capítulos", com resumo e nome do arquivo que o link deve seguir. Os arquivos textile  gerados/criados ficarão na pasta _source_, o resultado do "guides build" ficará na pasta _output_.

As páginas são em [textile](http://www.textism.com/tools/textile/), o que facilita bastante. Mas o grande diferencial, está em alguns _helpers_ disponibilizados pela gem, que torna muito simples criar áreas de destaque como INFO, WARNING ... etc e adicionar comandos e códigos.

Para ter idéia de uma página em textile, segue o link [source/getting_started.textile](https://github.com/rogerleite/rubygems_snapshot/raw/master/guides/source/getting_started.textile) (raw).


### Repensando a documentação


Com certeza, usando a _gem guides_, vocẽ consegue um boost na hora de documentar. Já evitando aquela chatice de editar wiki, .doc, controlando versão ... etc. Usando textile, seu editor preferido e git, não tem mais desculpas.


#### Desenvolvendo Frameworks e APIs


Já faz um tempo que eu topo com a _vibe_ [Documentation Driven Development](http://www.google.com.br/search?sourceid=chrome&ie=UTF-8&q=Documentation+Driven+Development). Como tudo na vida, é questão de bom senso. Documentar uma API, nada mais é do que definir a interface dela, e dependendo do caso, isto ajuda bastante antes de começar o desenvolvimento.


#### Você é um Usuário !


Uma documentação com um monte de imagem e seta, descrevendo o óbvio não serve para nada (além de passar raiva). Uma documentação **sucinta**, com o básico, por exemplo Instalação, Modo de Usar, Configurações Disponíveis é bem melhor que muito dossiê por aí ! Tente ao máximo se colocar no lugar do usuário ao escrever alguma documentação ou ajuda.

---

Não deixe de contribuir com o post. Mande sugestões, comentários, reclamações etc.


