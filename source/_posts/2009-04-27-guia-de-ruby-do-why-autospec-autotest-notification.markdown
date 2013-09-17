---
author: admin
comments: true
date: 2009-04-27 04:00:03+00:00
layout: post
slug: guia-de-ruby-do-why-autospec-autotest-notification
title: Guia de Ruby do Why e Autospec-notification
wordpress_id: 422
categories:
- novidades da semana
- quick tips
- tutorial
- web
tags:
- novidades da semana
- rspec
- ruby
- ruby on rails
- testes unitários
---

## O comovente guia de Ruby do Why


Este [livro](http://poignantguide.net/ruby/) é sensacional e demonstra exatamente o espírito do Ruby: papo de programador.

[![](/images/uploads/2009/04/thefoxes-3.png)](/images/uploads/2009/04/thefoxes-3.png)

O livro é bem divertido. As tirinhas das raposas são ótimas. Várias [pessoas](http://www.nomedojogo.com/2008/10/28/why%E2%80%99s-poignant-guide-to-ruby-em-portugues/) [contribuíram](http://akitaonrails.com/2008/5/14/vamos-traduzir-o-why-s-poignant-guide-to-ruby) com a tradução para pt-br que está disponível no [Github](http://github.com/carlosbrando/poignant-br/tree/master).

[Leitura obrigatória](http://why.nomedojogo.com/). Chunky bacon!


## Autospec-notification


O [Autospec](http://www.nateclark.com/articles/2008/09/17/_autotest_-is-now-_autospec_-how-to-set-up-autospec-for-rspec-and-rails-with-zentest) é um script gerado pelo [RSpec](http://rspec.info/) que utiliza o Autotest para rodar os testes automaticamente a cada alteração no código.

Unindo o útil ao agradável, foi criado o [Autospec-notification](http://www.mouseoverstudio.com/blog/2008/10/10/autospec-autotest-notification-autospec-notification-e-novidades/), que exibe as notificações do autospec no desktop.

[![](/images/uploads/2009/04/sucesso.png)](/images/uploads/2009/04/sucesso.png)

Para instalar, comece pela gem ZenTest:

    
    sudo gem install ZenTest


No linux, instale o Libnotify:

    
    sudo apt-get install libnotify-bin


Agora instale a gem do autotest-notification:

    
    sudo gem install carlosbrando-autotest-notification --source=http://gems.github.com


Ative o autotest-notification e rode o autospec no seu projeto:

    
    an-install



    
    script/autospec


O código está no [Github](http://github.com/carlosbrando/autotest-notification/tree/master). Escreva seus testes e divirta-se!
