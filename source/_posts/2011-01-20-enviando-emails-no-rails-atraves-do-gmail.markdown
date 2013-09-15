---
author: rodrigopanachi
comments: true
date: 2011-01-20 09:00:09+00:00
layout: post
slug: enviando-emails-no-rails-atraves-do-gmail
title: Enviando emails no Rails através do Gmail
wordpress_id: 774
categories:
- quick tips
- rails
- web
---

Enviar emails de uma aplicação Rails é [muito simples](http://guides.rubyonrails.org/action_mailer_basics.html). A parte complicada fica com a configuração do ambiente e do servidor de email. Se você não souber ou não tiver muita paciência para fazer isso (por exemplo instalar e configurar [sendmail](http://www.sendmail.org), [mutt](http://www.mutt.org/), etc), pode facilmente usar sua conta do [Gmail](http://lindsaar.net/2010/3/15/how_to_use_mail_and_actionmailer_3_with_gmail_smtp) para enviar as mensagens da sua aplicação.

Basta incluir as seguintes configurações no bloco `config` dos respectivos ambientes da aplicação (por exemplo, `config/environments/production.rb`):

    
      config.action_mailer.delivery_method = :smtp
      config.action_mailer.smtp_settings = {
        :enable_starttls_auto => true,
        :authentication => :plain,
        :address => smtp.gmail.com,
        :port => 587,
        :user_name => "seuemail@gmail.com",
        :password => "suasenha"
      }


O toque mágico é a chave `:enable_startls_auto => true` para habilitar [TLS](http://en.wikipedia.org/wiki/Transport_Layer_Security). Sucesso!
