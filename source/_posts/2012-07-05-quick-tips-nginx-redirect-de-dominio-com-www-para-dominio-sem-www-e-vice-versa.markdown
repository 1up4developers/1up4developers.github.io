---
author: rodrigopanachi
comments: true
date: 2012-07-05 13:00:45+00:00
layout: post
slug: quick-tips-nginx-redirect-de-dominio-com-www-para-dominio-sem-www-e-vice-versa
title: '[QuickTips] Nginx: redirect de domínio com www para domínio sem www e vice-versa'
wordpress_id: 1210
categories:
- quick tips
- web
tags:
- domain
- nginx
- redirect
---

Simples, basta usar o [HttpRewriteModule](http://wiki.nginx.org/HttpRewriteModule).

Para redirecionar de _www.dominio.com_ para _dominio.com_ faça:

    
    server {
        server_name  www.dominio.com;
        rewrite ^(.*) http://dominio.com$1 permanent;
    }
    
    server {
        server_name  dominio.com;
        #configurações do servidor aqui
    }


E para redirecionar de _domínio.com_ para _www.dominio.com_, faça:

    
    server {
        server_name  dominio.com;
        rewrite ^(.*) http://www.dominio.com$1 permanent;
    }
    
    server {
        server_name  www.dominio.com;
        #configurações do servidor aqui
    }


_Voilà!_
