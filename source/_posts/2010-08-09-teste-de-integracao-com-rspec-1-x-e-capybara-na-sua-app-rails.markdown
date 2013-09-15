---
author: rogerbarreto
comments: true
date: 2010-08-09 10:00:35+00:00
layout: post
slug: teste-de-integracao-com-rspec-1-x-e-capybara-na-sua-app-rails
title: Teste de integração com Rspec 1.x e Capybara na sua app Rails
wordpress_id: 704
categories:
- quick tips
- rails
- web
tags:
- quick tips
- rspec
- ruby on rails
- testes unitários
---

Este não é daqueles posts que explicam o que é rspec, testes, diferenças entre teste funcional, integração e etc. Este post é só uma rapidinha, pra quem já conhece [rspec](http://rspec.info/) e [capybara](http://github.com/jnicklas/capybara).

A documentação do Capybara é boa, mas deixa a desejar nas instruções de como integrá-lo somente com rspec. Com a ajuda do [George Guimarães](http://blog.georgeguimaraes.com/) da [Plataforma](http://blog.plataformatec.com.br/), cheguei num passo a passo mega simples.

No _config/environments/test.rb_ declare:

    
    config.gem "rspec",            :version => "= 1.3.0", :lib => false
    config.gem "rspec-rails",      :version => "= 1.3.2", :lib => false
    config.gem "capybara",         :version => "= 0.3.9", :lib => false


No _spec/spec_helper.rb_ coloque:

    
    require 'capybara/rails'
    [...]
    Spec::Runner.configure do |config|
    
      [...]
      config.include(Capybara, :type => :integration)
    
    end


Pronto! Isto é o suficiente para utilizar o Capybara nos testes de integração.
Para finalizar, você pode montar um teste simples de validação. Por exemplo:

Crie _spec/integration/site_spec.rb_ e coloque:

    
    require 'spec_helper'
    
    describe "Site sample" do
    
      context "root" do
    
        before do
          visit root_url
          save_and_open_page
        end
    
        it "should have Hello" do
          page.should have_content("Hello")
        end
    
      end
    
    end


Para executar somente os testes de integração:

    
    rake spec:integration




## have_content e um hot tip que pode te salvar muitas horas !


Caso aconteça o erro:

    
    undefined method `have_content' for #<ActionController::Integration::Session:0xb618ad10>


Graças a esta mensagem [Can't make matchers work with cucumber and rails 2.3.2](http://groups.google.com/group/ruby-capybara/browse_thread/thread/28467e031beb7ba1/19a368c21a278eb8?lnk=gst&q=have_content#19a368c21a278eb8), descobri que o _have_content_ não funciona com o rails 2.3.2, eu resolvi o problema fazendo um upgrade para a última versão do rails. #ficaadica

Dúvidas, melhorias, sugestões e elogios são sempre bem vindos !
