---
author: rogerbarreto
comments: true
date: 2010-05-14 10:00:21+00:00
layout: post
slug: teste-sua-aplicacao-de-linha-de-comando-com-cucumber
title: Teste sua aplicação de Linha de Comando com Cucumber!
wordpress_id: 643
categories:
- projetos
- quick tips
- real world
- ruby
tags:
- cucumber
- linha de comando
- rake
- ruby
- tutorial
---

É engraçado como tudo é questão de treino e disciplina. Levei um tempo para me acostumar com [TDD](http://en.wikipedia.org/wiki/Test-driven_development), [Vim](http://www.vim.org/) e não poderia ser diferente com testes funcionais, sendo mais especifico, [Cucumber](http://cukes.info/).

Até o momento, só tinha usado cucumber em projetos web. E quando voltei a desenvolver o [rubygems_snapshot](http://github.com/rogerleite/rubygems_snapshot), senti falta de algo para testar funcionalmente. Baseado no [pik](http://github.com/vertiginous/pik), montei um esquema simples para validar qualquer aplicação de linha de comando.


## Como instalar


Basicamente, será necessário (fontes via gist):



	
  * rake para executar o cucumber

	
  * env_terminal.rb

	
  * terminal_steps.rb




Dado que você tem cucumber instalado, com o esquema da pasta "features".



	
  * Copie o cucumber.rake para a raiz.

	
  * Copie o env_terminal.rb para a pasta features.

	
  * Copie o terminal_steps.rb para a pasta features/step_definitions/terminal_steps.rb.

	
  * Edite o  env.rb incluindo (pode ser no começo):



    
    require "env_terminal"





	
  * Dentro do Rakefile, pode ser no final mesmo, adicione:



    
    load "cucumber.rake"




## Como usar


Todas features:

    
    rake cucumber


Features com a tag @wip, também conhecida como Work in Progress.

    
    rake cucumber:wip




## Informações Extras


Caso precise de mais informações, você tem a opção de ver a saída dos comandos, executando a rake assim:

    
    rake cucumber show_output=true


ou

    
    rake cucumber:wip show_output=true


No caso do snapshot, tive a necessidade de "modificar" o comando gem toda vez que era executado, ou melhor, passar um parâmetro para controlar o ambiente. Dentro do env_terminal.rb, existe o método **gsub_command**, nele você pode "redefinir" comandos, caso necessite.


## **Gostei, quero mais!**


A solução acima, é bem "caseira". Para projetos simples com funcionalidades simples, funciona bem.

Caso queira algo mais robusto, você tem a opção da [gem Aruba](http://github.com/aslakhellesoy/aruba).

Tem este post como introdução:

[Aruba - Cucumber Goodness for the Command-Line](http://www.themodestrubyist.com/2010/04/22/aruba---cucumber-goodness-for-the-command-line/)
