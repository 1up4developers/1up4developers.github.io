---
author: rodrigopanachi
layout: post
title: "[QuickTips] Do Wordpress para Octopress/Jekyll no GitHub Pages"
date: 2013-09-20 22:38
comments: true
categories:
- quick tips
- web
tags:
- jekyll
- octopress
---

Quando migramos nosso blog do Wordpress para o GitHub Pages, escrevi um email para nossos autores com instruções resumidas para configurar e postar com Octopress/Jekyll. Percebi que dando um tapa nesse email, poderia publicá-lo aqui no blog como um guia rápido e talvez incentivar outros blogueiros a fazer o mesmo.

## Por que GitHub Pages?

Corte de custos! Manter o blog no Wordpress requer um hosting, um banco de dados e um domínio. Reduzimos as despesas apenas para o registro de domínio (por enquanto).

Performance! GitHub Pages é estático, e conteúdo estático é servido naturalmente mais rápido.

Desafio! Estavamos "acostumados" ao Wordpress. Aprender Jekyll e a postar "commitando em um projeto" permite que tenhamos novas idéias, ou no pior dos casos, aprendamos novas tecnologias.

## Requisitos

Para utilizar o GitHub Pages, crie um repositório com o nome **usuariogithub.github.io**, incluindo o "github.io". O GitHub gerencia este repositório e publica o conteúdo estático no endereço http://usuariogithub.github.io

Agora, para gerar o conteúdo estático vamos usar o [Octopress](http://octopress.org).

## Instalação

Basta clonar o repositório do Octopress localmente:

{% codeblock lang:bash %}
$ git clone git@github.com:imathis/octopress.git
{% endcodeblock %}

instalar as gems necessárias e em seguida rodar a rake para configuração:

{% codeblock lang:bash %}
$ rake setup_github_pages
{% endcodeblock %}

e informar o seu repositório do GitHub Pages:


    git@github.com:username/username.github.io.git


Pronto! Os remoting points do projeto serão configurados para seu repositório do GitHub, como segue:


{% codeblock lang:bash %}
$ git remote -v

octopress	git@github.com:imathis/octopress.git (fetch)
octopress	git@github.com:imathis/octopress.git (push)
origin	 git@github.com:username/username.github.io.git (fetch)
origin	 git@github.com:username/username.github.io.git (push)
{% endcodeblock %}

## Postando

Para criar um novo post, basta rodar a rake:

{% codeblock lang:bash %}
$ rake new_post["o titulo do seu post"]
{% endcodeblock %}

o que vai criar o arquivo _source/\_posts/2013-09-17-o-titulo-do-seu-post.markdown_. Escreva o conteúdo do seu post normalmente em Markdown (recomendo utilizar o [Markup Editor](http://markup.herokuapp.com/)) e execute:

{% codeblock lang:bash %}
$ rake generate
{% endcodeblock %}

para gerar o site estático no diretório __deploy_. Caso queira dar um preview no que será publicado, basta rodar:

{% codeblock lang:bash %}
$ rake preview
{% endcodeblock %}

e acessar no browser http://localhost:4000.

## Publicando

Quando terminar seu post, basta rodar:

{% codeblock lang:bash %}
$ rake deploy
{% endcodeblock %}

para publicar o site no seu repositório do GitHub Pages.

Pronto! Não se esqueça de subir os fontes do site (branch source), commitando suas alterações e executando o classico _git push_.

## Migrando

Caso já tenha um site publicado no Wordpress, você pode seguir este guia para importar todo o conteúdo na estrutura do Jekyll:

[How to Migrate from WordPress to Jekyll Running on Github](http://johnnycode.com/2012/07/10/how-to-migrate-from-wordpress-to-jekyll-running-on-github/)

## Referências

* [GitHub Pages](http://pages.github.com/)
* [Octopress.org](http://octopress.org/)
* [Jekyll](http://jekyllrb.com/)
