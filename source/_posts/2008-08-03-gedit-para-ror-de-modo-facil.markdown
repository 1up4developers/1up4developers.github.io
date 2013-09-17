---
author: rogerbarreto
comments: true
date: 2008-08-03 17:04:26+00:00
layout: post
slug: gedit-para-ror-de-modo-facil
title: GEdit para RoR de modo fácil
wordpress_id: 55
categories:
- tutorial
tags:
- gedit
- rails
- ruby
---

Olá Pessoal, terceira reinauguração do blog, e agora finalmente resolvemos o sonho do dominio próprio ! :D

Uma das grandes dificuldades para quem está iniciando com Ruby on Rails no linux, é descobrir qual editor usar, já que o sempre-citado [Textmate](http://macromates.com/) é somente para Mac.

O que eu estou usando atualmente é o [gedit](http://www.gnome.org/projects/gedit/), editor de texto padrão, que vem com o Ubuntu e provavelmente com o Gnome.

Quando comecei minha busca pelo editor perfeito, encontrei vários tutoriais que ensinavam a encher o gedit de plugins, só que era muito manual, tinha que baixar um por um ... quando tive que fazer a segunda vez, desisti, e resolvi buscar por soluções melhores.

Sempre ouvi o [Rails Podcast](http://podcast.rubyonrails.pro.br/), e em algum destes, o Carlos Brando citou o [github](http://www.github.com/). Entrei lá, e fui verificar o que tinha de bom, e até hoje não sei como encontrei os projetos, só sei que comecei a  usá-los.

Vamos ao que interessa !

No projeto [gedit-rails](http://github.com/mig/gedit-rails/tree/master) faça o download [aqui](http://github.com/mig/gedit-rails/tarball/master) ou clique em download na página do projeto (estamos no linux, use o formato tar). Descompacte o arquivo, entre na pasta e execute o install.sh veja a imagem abaixo:

[caption id="attachment_65" align="aligncenter" width="300" caption="install do gedit-rails"][![install-gedit-rails](/images/uploads/2008/08/install-gedit-rails-300x131.png)](/images/uploads/2008/08/install-gedit-rails.png)[/caption]

No projeto [gedit-themes](http://github.com/mig/gedit-themes/tree/master) você encontrará temas para o visual do gedit, o meu preferido é o darkmate, acho muito confortável para os olhos, mas existem várias opções. O processo de instalação é mais simples que os plugins.

Baixe os temas em tar [aqui](http://github.com/mig/gedit-themes/tarball/master) ou no botão download na página do projeto. Descompacte o tar numa pasta separada, e na tela de preferências, fontes e cores do gedit, você consegue adicionar o novo tema que deseja.

[![](/images/uploads/2008/08/add-gedit-theme2-300x181.png)](/images/uploads/2008/08/add-gedit-theme2.png)

Veja o resultado aqui:

[![](/images/uploads/2008/08/result-gedit2-300x158.png)](/images/uploads/2008/08/result-gedit2.png)

Infelizmente estou sem tempo para entrar nos detalhes de cada plugin, de qualquer maneira, vou deixar aos interessados os links, que contém informações valiosas:



	
  * [HTML Tidy](http://www.eng.tau.ac.il/~atavory/gedit-plugins/html-tidy/)

	
  * [Rails Hot Commands](http://code.google.com/p/rhc/)

	
  * [Rails Hotkeys](http://simplesideias.com.br/rails-hotkeys/)

	
  * [Snapopen](http://www.upperbound.net/snapopen/)


Existem muitos plugins para o gedit, caso estejam procurando por mais, tentem na [Lista de Plugins para o gedit](http://simplesideias.com.br/lista-de-plugins-para-o-gedit/) do Fernando Vieira, e tem também a [lista oficial de plugins](http://live.gnome.org/Gedit/Plugins) do gedit.

Dicas de novos plugins, comentários e/ou críticas são bem vindas !

Abraço e sucesso!

UPDATE: Sugestões do Philipe Farias, via comentários ! Valeu pelas dicas !

Plugins:

- Snippets ou Trechos - padrão do Gedit (tem que ativar). Quando o gedit-rails é instalado ele adiciona os snippets para erb, ruby e shoulda;

[![](/images/uploads/2008/08/gedit-snippets-249x300.png)](/images/uploads/2008/08/gedit-snippets.png)

- [gedit_formatter](http://github.com/urubatan/gedit_formatter/tree/master)

[](http://github.com/urubatan/gedit_formatter/tree/master)- [gemini](http://www.garyharan.com/index.php/2006/11/16/gemini-gedit-plugin-for-all-those-textmate-fans/)

- [gedit TODO list](http://alexandredasilva.wordpress.com/gedit-todo-list-plugin/)

Esta parte é interessante para seguir ao padrão Ruby, "Recomenda-se também configurar a “largura das tabulações” para 2 e ativar “inserir espaços em vez de tabulações” nas preferências do Gedit".

Aqui já entra configurações mais pessoais que você encontra na tela de preferências do gedit. Também de ativar “mostrar números de linhas”, “destacar linha atual” e “destacar parêntesis correspondentes”.

Quanto ao esquema de cor acho o Oblivion mais “completo” para Rails. Screenshot do gedit com Oblivion:

[![](/images/uploads/2008/08/gedit-oblivion-300x216.png)](/images/uploads/2008/08/gedit-oblivion.png)
