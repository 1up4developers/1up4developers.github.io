---
author: rodrigopanachi
comments: true
date: 2009-05-31 18:33:06+00:00
layout: post
slug: agendando-tarefas-em-aplicacoes-rails-com-rufus-scheduler
title: Agendando tarefas em aplicações Rails com rufus-scheduler
wordpress_id: 501
categories:
- quick tips
- web
tags:
- gem
- ruby on rails
---

[Rufus](http://rufus.rubyforge.org/) é um conjunto de gems utilizado para [Workflow](http://pt.wikipedia.org/wiki/Fluxo_de_Trabalho) e [BPM](http://pt.wikipedia.org/wiki/Business_Process_Management). O [rufus-scheduler](http://rufus.rubyforge.org/rufus-scheduler/) é a gem responsável pelo agendamento e execução de tarefas (jobs). Se você programa em Java e conhece o [Quartz](http://www.opensymphony.com/quartz/wikidocs/QuickStart.html) não vai ter dificuldade em utilizá-la.

Instalação:

    
    sudo gem install rufus-scheduler


Utilização:

    
    require 'rubygems'
    require 'rufus/scheduler'
    
    scheduler = Rufus::Scheduler.start_new
    
    scheduler.every '5m' do
      puts 'Executando a cada 5 minutos'
    end
    
    scheduler.schedule '0 18 * * *' do
      puts 'Executando todos os dias as 18h'
    end


Simples assim! Consulte a [documentação](http://rufus.rubyforge.org/rufus-scheduler/) oficial ou [contribua com o código](http://github.com/jmettraux/rufus-scheduler/tree/master).
