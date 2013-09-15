---
author: rodrigopanachi
comments: true
date: 2010-07-02 09:00:23+00:00
layout: post
slug: gerando-rotas-com-parametros-dinamicos-no-rails-de-modo-facil
title: Gerando rotas com parâmetros dinâmicos no Rails de modo fácil
wordpress_id: 680
categories:
- quick tips
- rails
- ruby
- web
tags:
- quick tips
- ruby
- ruby on rails
---

A [API de rotas](http://api.rubyonrails.org/classes/ActionController/Routing.html) do Rails simplifica consideravelmente o desenvolvimento fornecendo um padrão de geração e utilização de URLs para toda aplicação. Porém algumas necessidades especificas e relativamente simples podem gerar dores de cabeça se forem implementadas incorretamente.

Um caso bastante comum são URLs compostas que sempre apontam para um mesmo recurso. Por exemplo, um blog que possua rotas para seus posts no formato `/posts/autor/categoria/permalink` provavelmente terá uma rota mapeada como `map.post "posts/:author/:category/:permalink"` gerando automaticamente os helpers `post_path` e `post_url`.

Muito bom, porém para [usufruirmos desta facilidade](http://guides.rubyonrails.org/routing.html#generating-urls-from-code) precisamos fornecer os valores dos parâmetros dinâmicos nos _controllers_ e _views_:

    
    post_path(:author => @post.author, :category => @post.category, :permalink => @post.permalink)


E mesmo que você forneça os [parâmetros em um array](http://guides.rubyonrails.org/routing.html#route-generation-from-arrays), vai dar muito trabalho além de deixar muito código repetido pela aplicação.


## Entendi! Mas como resolvo este problema?


Para este caso, apenas implementar o método `[to_param](http://api.rubyonrails.org/classes/ActiveRecord/Base.html#M001840)` do model não vai servir. Uma solução seria reescrever o método `post_path` (que é gerado automaticamente) no respectivo helper (posts_helper.rb):

    
    def post_path(post, options = {})
      super(post, :author => post.author, :category => post.category, :permalink => post.permalink)
    end


Outra solução seria sobrescrever o método `[default_url_options](http://api.rubyonrails.org/classes/ActionController/Base.html#M000467)` no _controller_ para retorna os parâmetros padrões da rota:

    
    def default_url_options(options = {})
      options.merge!(:author => @post.author, :category => @post.category, :permalink => @post.permalink) if options[:action] == "show"
    end


A má notícia é que você terá que fazer isto para todos seus controllers e respectivos models.

A terceira solução (e mais elegante) é padronizar a maneira com que os parâmetros opcionais da rota são obtidos a partir do controller e seu respectivo model. Basta adicionar os seguintes métodos no seu _ApplicationController_:

    
    def default_url_options(options = {})
      if (model = controller_model(options))
        dynamic_route_params(options).each do |param|
          options[param] = model.send(param) if model.respond_to?(param)
        end
      end
      options
    end
    
    def controller_model(options = {})
      clazz = (options[:controller].singularize.camelize.constantize rescue ActiveRecord)
      options.select { |key, value| value.is_a?(clazz) }.first.second
    rescue
      nil
    end 
    
    def dynamic_route_params(options = {})
      returning [] do |dynamic_params|
        matched_routes = ActionController::Routing::Routes.routes.select do |route|
          route.matches_controller_and_action?(options[:controller], options[:action])
        end
        dynamic_segments = matched_routes.map(&:segments).flatten.each do |segment|
          dynamic_params << segment.key if segment.is_a?(ActionController::Routing::DynamicSegment)
        end
      end
    end


Assim, os valores defaults dos parâmetros da rota serão obtidos diretamente do model. Caso queiram contribuir, este código está disponível no [github](http://github.com/1up4dev/random-samples/blob/master/dynamic_route_params/dynamic_route_params.rb).

**Referências**
[Rails Routing from the Inside Out](http://railsguts.com/routing_inside_out.html)
[Rails Guides: Routing](http://guides.rubyonrails.org/routing.html)
