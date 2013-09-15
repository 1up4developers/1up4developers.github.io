---
author: rodrigopanachi
comments: true
date: 2011-01-26 10:00:59+00:00
layout: post
slug: deploy-de-varias-aplicacoes-no-nginx-passenger-usando-subdomain-ou-suburi
title: Deploy de várias aplicações no Nginx + Passenger usando subdomain ou suburi
wordpress_id: 770
categories:
- quick tips
- rails
- web
---

Se você está pensando em fazer o deploy da sua aplicação Rails em um server [Nginx](http://nginx.org/) com [Phusion Passenger](http://www.modrails.com/), você está no caminho certo! Não apenas pela segurança e performance do Nginx, mas também pela facilidade de instalação e configuração quando comparado à outros web servers populares. 

A [instalação do Nginx](http://wiki.nginx.org/Install) bem como [configuração do módulo do Passenger](http://www.modrails.com/install.html) são relativamente simples. A documentação de [ambos é bem completa](http://www.modrails.com/documentation/Users%20guide%20Nginx.html#_installing_phusion_passenger). Você provavelmente não terá muita dificuldade. Eu recomendo seguir o passo-a-passo da instalação do Passenger:


    
    
      passenger-install-nginx-module
    



Uma vez que tudo esteja instalado, é hora de configurar o Nginx editando o arquivo `nginx.conf` (provavelmente localizado em `/usr/local/nginx/conf/nginx.conf`)



### Usando sub URI



O deploy como sub URI torna sua aplicação acessível com o endereço `http://dominio.com/app1`, sendo `app1` o nome da sua aplicação. Supondo que diretório público do Nginx esteja em `/var/www` e sua aplicação Rails em `/var/rails/app1`, configure-o assim:


    
    
    http {
        ...
        server {
            listen 80;
            server_name dominio.com www.dominio.com;
            root /var/www;
            passenger_enabled on;
            passenger_base_uri /app1;
        }
        ...
    }
    



Ainda é preciso criar um link simbólico para que o contexto da aplicação Rails seja visível pelo document root em `/var/www`:


    
    
    ln -s /var/rails/app1/public /var/www/app1
    



Reiniciando o Nginx, sua aplicação já estará rodando. Para adicionar outras aplicações, basta adicionar:


    
    
            passenger_base_uri /app1;
            passenger_base_uri /app2;
            passenger_base_uri /app3;
    



E em seguida criar os links simbólicos como descrito acima. Nota importante: em alguns casos, será necessário informar a URL relativa correta nas configurações do Rails:


    
    
       config.action_controller.relative_url_root = "/app1"
    





### Usando subdomain



O deploy como subdomain torna sua aplicação acessível com o endereço `http://app1.dominio.com`. Basta configurar o Nginx como segue:


    
    
    http {
        ...
        server {
            listen 80;
            server_name app1.dominio.com;
            root /var/rails/app1/public;
            passenger_enabled on;
        }
        ...
    }
    



Note que o `root` aponta diretamente para o diretório "public" da aplicação Rails. Para fazer o deploy de outras aplicações como subdomínio, basta configurar outro "server", alterando `root` e o `server_name`:


    
    
    http {
        ...
        server {
            listen 80;
            server_name app1.dominio.com;
            root /var/rails/app1/public;
            passenger_enabled on;
        }
        ...
        server {
            listen 80;
            server_name app2.dominio.com;
            root /var/rails/app2/public;
            passenger_enabled on;
        }
        ...
    }
    



Uma vez que o DNS do host esteja corretamente configurado, suas aplicações estarão acessíveis em `http://app1.dominio.com`, `http://app2.dominio.com`, etc.

Dúvidas ou sugestões, utilizem os comentários. Sucesso!
