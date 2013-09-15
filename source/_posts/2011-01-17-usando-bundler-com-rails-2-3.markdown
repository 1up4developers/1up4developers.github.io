---
author: pbalduino
comments: true
date: 2011-01-17 11:00:55+00:00
layout: post
slug: usando-bundler-com-rails-2-3
title: Usando Bundler com Rails 2.3
wordpress_id: 745
categories:
- rails
- tutorial
---

Iniciando minha participação neste blog, vou apresentar uma solução rápida para um problema que eu tive e levei um certo tempo para encontrar uma resposta.

Caso você não conheça, Bundler é uma ferramenta criada para facilitar a instalação e atualização de aplicações e suas gems, resolvendo facilmente as dependências e criando ambientes homogêneos em máquinas diferentes. Em outras palavras, resolve de uma vez por todas aquele inferno que era ter que baixar gem por gem da sua aplicação e ainda assim correr o risco de encontrar alguma incompatibilidade.

O Rails 3 utiliza o Bundler por padrão, mas aplicações em Rails 2.x ainda dependem das gems declaradas no arquivo `config/environment.rb`, e ao copiar os arquivos para uma determinada máquina, você é obrigado a usar `rake gems:install`, que vai levar algum tempo rodando e não vai instalar tudo o que você precisa.

Ou pelo menos era assim que funcionava até você ler esse post.

Primeiro, considerando que você esteja no diretório raiz da sua aplicação, execute a sequência abaixo:

Antes de qualquer outra coisa, instale o Bundler na sua máquina executando o comando `gem install bundler`

No seu arquivo `config/boot.rb`, imediatamente antes da linha `Rails.boot!`, insira o bloco abaixo:

    
    class Rails::Boot
      def run
        load_initializer
    
        Rails::Initializer.class_eval do
          def load_gems
            @bundler_loaded ||= Bundler.require :default, Rails.env
          end
        end
    
        Rails::Initializer.run(:set_load_path)
      end
    end
    


Crie um arquivo `config/preinitializer.rb`. Esse arquivo não existe no Rails 2.x, então se você não criar, não vai funcionar. Adicione as linhas abaixo nesse arquivo:

    
    begin
      require "rubygems"
      require "bundler"
    rescue LoadError
      raise "Could not load the bundler gem. Install it with `gem install bundler`."
    end
    
    if Gem::Version.new(Bundler::VERSION) <= Gem::Version.new("0.9.24")
      raise RuntimeError, "Your bundler version is too old for Rails 2.3." +
       "Run `gem install bundler` to upgrade."
    end
    
    begin
      # Set up load paths for all bundled gems
      ENV["BUNDLE_GEMFILE"] = File.expand_path("../../Gemfile", __FILE__)
      Bundler.setup
    rescue Bundler::GemNotFound
      raise RuntimeError, "Bundler couldn't find some gems." +
        "Did you run `bundle install`?"
    end
    


Crie um arquivo chamado `Gemfile` no raiz da sua aplicação. Entre em `config/environment.rb`, remova todas as instruções config.gem e as adicione no arquivo `Gemfile` conforme abaixo, sem o prefixo `config.` (ponto incluído).

Seu arquivo `config/environment.rb` era assim:

    
    # Be sure to restart your server when you modify this file
    
    # Specifies gem version of Rails to use when vendor/rails is not present
    RAILS_GEM_VERSION = '2.3.10' unless defined? RAILS_GEM_VERSION
    
    # Bootstrap the Rails environment, frameworks, and default configuration
    require File.join(File.dirname(__FILE__), 'boot')
    
    Rails::Initializer.run do |config|
      # Settings in config/environments/* take precedence over those specified here.
      # Application configuration should go into files in config/initializers
      # -- all .rb files in that directory are automatically loaded.
    
      # Add additional load paths for your own custom dirs
      # config.load_paths += %W( #{RAILS_ROOT}/extras )
    
      # Specify gems that this application depends on and have them installed with rake gems:install
      config.gem "bj"
      config.gem "hpricot", :version => '0.6', :source => "http://code.whytheluckystiff.net"
      config.gem "sqlite3-ruby", :lib => "sqlite3"
      config.gem "aws-s3", :lib => "aws/s3"
      config.gem "authlogic", :version => "=2.1.6"
      config.gem "slim_scrooge", "=1.0.10"
    
      # Only load the plugins named here, in the order given (default is alphabetical).
      # :all can be used as a placeholder for all plugins not explicitly named
      # config.plugins = [ :exception_notification, :ssl_requirement, :all ]
    
      # Skip frameworks you're not going to use. To use Rails without a database,
      # you must remove the Active Record framework.
      # config.frameworks -= [ :active_record, :active_resource, :action_mailer ]
    end
    


E agora é assim:

    
    # Be sure to restart your server when you modify this file
    
    # Specifies gem version of Rails to use when vendor/rails is not present
    RAILS_GEM_VERSION = '2.3.10' unless defined? RAILS_GEM_VERSION
    
    # Bootstrap the Rails environment, frameworks, and default configuration
    require File.join(File.dirname(__FILE__), 'boot')
    
    Rails::Initializer.run do |config|
      # Settings in config/environments/* take precedence over those specified here.
      # Application configuration should go into files in config/initializers
      # -- all .rb files in that directory are automatically loaded.
    
      # Add additional load paths for your own custom dirs
      # config.load_paths += %W( #{RAILS_ROOT}/extras )
    
      # Only load the plugins named here, in the order given (default is alphabetical).
      # :all can be used as a placeholder for all plugins not explicitly named
      # config.plugins = [ :exception_notification, :ssl_requirement, :all ]
    
      # Skip frameworks you're not going to use. To use Rails without a database,
      # you must remove the Active Record framework.
      # config.frameworks -= [ :active_record, :active_resource, :action_mailer ]
    end
    


Bem mais limpo, não?

Já o seu recém criado arquivo `Gemfile` vai ficar assim:

    
    source 'http://gemcutter.org'
    gem "bj"
    gem "hpricot", "~> 0.6"
    gem "sqlite3-ruby", "~> 1.3.1", :require => "sqlite3"
    gem "aws-s3", :lib => "aws/s3"
    gem "rails", "= 2.3.10"
    gem "authlogic", "~> 2.1.6"
    
    group :development, :test do
      gem 'mongrel', :require => false
    end
    
    group :production do
      gem "slim_scrooge", "~> 1.0.10"
    end
    


Note que eu criei dois grupos: um com `:development, :test` e um com `:production`. Significa que as gems do primeiro grupo somente serão utilizadas em desenvolvimento e em testes, e as do segundo grupo somente serão usadas no ambiente de produção. O que está fora dos grupos será usado em qualquer ambiente.

Agora, finalmente você digita `bundle install`, aguarda alguns instantes e pronto! A sua aplicação está com todas as gems instaladas e suas devidas dependências.

Para listar as gems utilizadas, execute `bundle list`.

Sugestões, correções e críticas são bem vindas nos comentários.

Para mais informações, leia a documentação no site do Bundle: [http://gembundler.com/](http://gembundler.com/)
