---
author: rodrigopanachi
comments: true
date: 2009-07-13 03:30:48+00:00
layout: post
slug: paginacao-no-rails-com-will_paginate-e-ajax-de-modo-facil
title: Paginação no Rails com will_paginate e Ajax de modo fácil
wordpress_id: 503
categories:
- quick tips
- rails
- web
tags:
- gem
- ruby on rails
---

Paginação é um recurso simples e indispensável em qualquer aplicação séria. Em se tratando de Rails, a solução mais popular é a gem [WillPaginate](http://github.com/mislav/will_paginate/tree/master) que basicamente adiciona o método "paginate_" aos models do ActiveRecord e fornece um helper para renderização dos links da paginação nas views.

Instalando a gem:

    
    sudo gem install will_paginate


Para utilizar na aplicação, adicione no final do `config/environment.rb`:

    
    require 'will_paginate'


Altere o controller para utilizar paginação:

    
    def index
      @posts = Post.paginate :all, :page => params[:page], :per_page => 10
    end


E adicione os links da paginação na view:

    
    <%= will_paginate @posts %>


Pronto! Ao clicar nos links da paginação o parâmetro "page" será incluído automaticamente na requisição.

[![](http://1up4dev.org/wp-content/uploads/2009/07/posts-300x253.png)](http://1up4dev.org/wp-content/uploads/2009/07/posts.png)


## Legal, mas cadê o "ajax"?


Por padrão o WillPaginate não se preocupa com isso. O próprio desenvolvedor [recomenda usar javascript](http://wiki.github.com/mislav/will_paginate/ajax-pagination) para interceptar o "click" dos links e renderizar o resultado na mesma página.

Outra alternativa seria estender a classe responsável por renderizar os links da paginação para utilizar requisições com ajax.

Inclua em `app/helpers` o arquivo `ajax_will_paginate.rb` com o código:

    
    class AjaxWillPaginate < WillPaginate::LinkRenderer
      def prepare(collection, options, template)
        @update = options[:update]
        super
      end
      protected
      def page_link(page, text, attributes = {})
        @template.link_to_remote(text, {
          :url     => url_for(page),
          :method  => :get,
          :update => @update
        })
      end
    end


Então adicione no final do arquivo `config/environment.rb`:

    
      WillPaginate::ViewHelpers.pagination_options[:renderer] = 'AjaxWillPaginate'


E altere a chamada do helper na view para:

    
    <%= will_paginate @posts, :update => 'div_principal' %>


Informe na opção `:update` o Id de um objeto html que contenha todo o conteúdo da paginação que será substituído nas requisições seguintes.

É importante lembrar que esta solução altera o comportamento de todos os helpers de paginação da aplicação, por isso deve ser utilizada com cautela. [Outras](http://www.botvector.net/2008/08/willpaginate-on-ajax.html) [soluções](http://weblog.redlinesoftware.com/2008/1/30/willpaginate-and-remote-links) parecidas podem ser encontradas [aqui](http://www.google.com.br/search?q=ajax+will+paginate).
