---
author: rodrigopanachi
comments: true
date: 2011-01-18 09:00:41+00:00
layout: post
slug: criando-um-webcrawler-de-modo-facil-e-rapido-com-ruby-e-nokogiri
title: Criando um WebCrawler de modo fácil e rápido com Ruby e Nokogiri
wordpress_id: 725
categories:
- quick tips
- ruby
- web
tags:
- shellscript
---

Quantas vezes você precisou buscar alguma informação externa para sua aplicação, como um número de telefone ou uma foto em algum site? A idéia é simples: criar um [crawler](http://en.wikipedia.org/wiki/Web_crawler) script para fazer parse de algum conteúdo HTML. E basta apenas Ruby e [Nokogiri](http://nokogiri.org/).


### Exemplo


Digamos que eu queira obter a última versão de uma dada gem consultando diretamente no site [http://rubygems.org](http://rubygems.org/) para me ajudar quando precisar instalar novas gems no meu ambiente. Vamos ao código:

    
    #!/usr/bin/env ruby
    
    require "rubygems"
    require "open-uri"
    require "nokogiri"
    
    gem = ARGV[0]
    site = open("http://rubygems.org/gems/#{gem}")
    document = Nokogiri::HTML(site)
    version = document.css(".title h3").text
    
    puts "#{gem} version #{version}"


Vou salvar esse script no arquivo `gem-version.sh`, torna-lo executável com `chmod +x gem-version.sh` em seguida executar no terminal:

    
    $ ./gem-version rails
      rails version 3.0.3


Pronto, agora você também pode brincar à vontade com [Nokogiri](http://github.com/tenderlove/nokogiri) no seu shell!
