---
author: rogerbarreto
comments: true
date: 2011-11-08 11:00:28+00:00
layout: post
slug: dica-rapida-removendo-o-test-unit-de-um-projeto-rails-3
title: 'Dica Rápida: Removendo o test unit de um projeto Rails 3'
wordpress_id: 1114
categories:
- quick tips
- rails
- web
tags:
- quick tips
- rails
---

Este post serve mais como "cola" de referência, pois toda vez que vou fazer isso eu não encontro fácil no google e nunca lembro. Por sinal é mega simples, é só editar o _config/application.rb_ e remover a linha:

    
    require 'rails/all'


E substituir por:

    
    require "active_record/railtie"
    require "action_controller/railtie"
    require "action_mailer/railtie"
    require "active_resource/railtie"
    require "sprockets/railtie"


Após isso, já não aparece mais as tasks de testes, e você pode remover a pasta test.

    
    $ git rm -r test/


Pra finalizar, já segue a dica de alterar a task _default_, no final do _Rakefile_ é só colocar:

    
    Rake::Task[:default].prerequisites.clear
    task :default => :spec  #no caso do rspec


Sucesso!
