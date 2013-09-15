---
author: rodrigopanachi
comments: true
date: 2012-03-20 03:57:44+00:00
layout: post
slug: quick-tips-habilitando-auth-basic-nginx-e-como-gerar-senhas-htpasswd
title: '[QuickTips] Habilitando auth_basic no Nginx e como gerar senhas do htpasswd'
wordpress_id: 1134
categories:
- quick tips
- ruby
- web
---

Dica para quem usa o [Nginx](http://wiki.nginx.org/Main) como web server de aplicações Rails e já apanhou para habilitar [HTTP Basic Authentication](http://wiki.nginx.org/HttpAuthBasicModule) ou para gerar as senhas criptografadas em MD5.

Para habilitar o _auth_basic_, basta adicionar dentro do bloco _server_ do arquivo _nginx.conf_:

    
    location ~ / {
            auth_basic            "Restricted";
            auth_basic_user_file  htpasswd;
            passenger_enabled on;
    }


Um detalhe importante: se estiver rodando sua app com [Passenger](http://www.modrails.com/), inclua a linha _passenger_enabled on;_

Ah, já estava esquecendo das senhas. Elas devem ficar no arquivo _htpasswd_, no mesmo diretório do arquivo_ nginx.conf_ e precisam seguir o formato user:senha em cada linha. Por exemplo:

    
    user:sd5dsjo23PwdSh
    admin:mdePW2hgrPddSA


O detalhe é que a senha precisa ser [criptografada em MD5](http://wiki.nginx.org/Faq#How_do_I_generate_an_htpasswd_file_without_having_Apache_tools_installed.3F). Uma maneira fácil (e que funciona) de fazer isso é executando:

    
    ruby -e "puts 'usuario:' + 'senha'.crypt('md5')" >> htpasswd


Sucesso!
