---
author: rogerbarreto
comments: true
date: 2010-05-19 13:14:18+00:00
layout: post
slug: active-record-em-como-adicionar-comportamento-as-suas-associacoes
title: 'Active Record em: Como adicionar comportamento as suas associações'
wordpress_id: 657
categories:
- quick tips
- rails
- real world
- ruby
tags:
- active record
- pragmatismo
- rails
- ruby
- ruby on rails
---

Qualquer um que comece a desenvolver com [Active Record](http://api.rubyonrails.org/files/vendor/rails/activerecord/README.html) (AR), minha primeira recomendação é, para tudo e leia:  [A Guide to Active Record Associations](http://guides.rubyonrails.org/association_basics.html) ou [O Guia de Associações do Active Record](http://guias.rubyonrails.pro.br/association_basics.html). O guia é bem completo, e descreve muito bem os tipos de associações que estão disponíveis no AR.


## Association Proxy, #wtf !


As associações:



	
  * belongs_to

	
  * has_one

	
  * has_many

	
  * has_and_belongs_to_many


Quando usadas, adicionam alguns métodos (veja [Detailed Association Reference](http://guides.rubyonrails.org/association_basics.html#detailed-association-reference)). Por exemplo, ao declarar uma associação _belongs_to_, o model "ganhará" os seguintes métodos:



	
  * association(force_reload = false)

	
  * association=(associate)

	
  * build_association(attributes = {})

	
  * create_association(attributes = {})


Onde association, será substituído pelo nome da associação. Exemplo retirado do guides:

    
    class Order < ActiveRecord::Base
       belongs_to :customer
    end


Cada instância de Order, conterá os métodos:





	
  * customer

	
  * customer=

	
  * build_customer

	
  * create_customer


O _association proxy_, é o objeto que faz a ligação do objeto que contém a associação, conhecido como _owner_, e o objeto associado, conhecido como _target_.


## Legal e daí !?!


Graças ao _association proxy_, ao declarar uma associação, podemos extendê-la e adicionar comportamentos "customizados". No guia, é citado como [Association Extensions](http://guides.rubyonrails.org/association_basics.html#association-extensions). O código de exemplo abaixo, está no github em [random-samples](http://github.com/rogerleite/random-samples).

Para exemplificar, vamos criar um modulo que adiciona o comportamento de uma galeria a qualquer coleção.


    
    
    module GalleryColletion
    
      def current=(curr = nil)
        @current, @index = nil
    
        if curr.nil?
          @current = collection.first
          @index = 0
        else
          collection.each_with_index do |item, index|
            if item.id.to_i == curr.to_i
              @current = item
              @index = index
            end
          end
        end
        @current
      end
    
      def current
        @current
      end
    
      def position
        @index + 1
      end
    
      def previous?
        return false if @index.nil?
        !!(@index - 1 >= 0)
      end
      def previous
        collection[@index - 1] if previous?
      end
    
      def next?
        return false if @index.nil?
        !!(@index + 1 < collection.size)
      end
      def next
        collection[@index + 1] if next?
      end
    
      private
    
      def collection
        proxy_owner.send(proxy_reflection.name)
      end
    
    end
    


[Gallery Collection](http://github.com/rogerleite/random-samples/blob/master/association_extend/lib/extensions/gallery_collection.rb)

Note que o modulo está na pasta lib, logo, a pasta tem que ser adicionada no path via config/environment.rb.

Para extender a associação, declare:


    
    
    class Article < ActiveRecord::Base
      has_and_belongs_to_many :images, :extend => GalleryColletion
    end
    


[Article model](http://github.com/rogerleite/random-samples/blob/master/association_extend/app/models/article.rb), [Image model aqui](http://github.com/rogerleite/random-samples/blob/master/association_extend/app/models/image.rb).

Agora para navegar entre as imagens, você pode usar:

    
    a = Article.first
    a.images.current = 1 #1 e o Image.id que deseja selecionar
    a.images.current
    a.images.position
    a.images.next?
    a.images.next
    a.images.previous?
    a.images.previous


Caso esteja com coragem, baixe o projeto e veja rodando.

Dúvidas, sugestões, algum "case" de sucesso, comente!
