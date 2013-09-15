---
author: rogerbarreto
comments: true
date: 2013-01-23 15:29:16+00:00
layout: post
slug: http-monkey
title: HTTP Monkey - lançado!
wordpress_id: 1274
categories:
- marketing
- projetos
- real world
- ruby
- web
tags:
- faraday
- http client
- HTTP Monkey
- http_monkey
- restfulie
---

O [HTTP Monkey](https://github.com/rogerleite/http_monkey) é um cliente http simples, com **interface fluente**, suporte a **múltiplos _adapters_** (Net::HTTP, Curb, HTTPClient, EM-HTTP-Request) e **_middlewares _**no_ estilo rack._

Pontos positivos:



	
  * [Documentação](http://rogerleite.github.com/http_monkey). Está um pouco desorganizado, mas tem. :D

	
  * [Autenticações](http://rogerleite.github.com/http_monkey/#light_and_powerful). Suporta basic, digest e SSL.

	
  * [Middlewares](http://rogerleite.github.com/http_monkey/#more_power_to_the_people_for_god_sake) são simples classes Ruby. Por exemplo temos o [middleware que dá suporte a cookies](https://github.com/rogerleite/http_monkey-cookie).

	
  * [Callback](http://rogerleite.github.com/http_monkey/#flexibility) por Response code.

	
  * Helpers no headers response. Ex: resp.headers.content_type?; resp.headers.cache_control.max_age

	
  * Pouco código, fácil manutenção (mais fácil do povo contribuir também).


Pontos negativos:

	
  * Gem nova. Ainda não tem um case em produção.

	
  * Falta de middlewares para funcionalidades como Cache, OAuth … etc.

	
  * Não suporta adapter que permite requisições em paralelo.

	
  * Tem o [Faraday](https://github.com/lostisland/faraday) como concorrente, que tem base em produção e bastantes _middlewares_.

	
  * A comunidade nacional e internacional ainda não conhece o Monkey (comecei agora a trabalhar nisso).


Na página [HTTP Monkey an alternative to Faraday](http://rogerleite.github.com/http_monkey/http_monkey_an_alternative_to_faraday.html), comecei um trabalho de "localizar" o desenvolvedor que está acostumado com o Faraday em como trabalhar com o Monkey. Lembrando que a DSL do HTTP Monkey, foi feita pensando em substituir o uso do [Restfulie](https://github.com/caelum/restfulie), muita usada nos projetos da Abril Mídia.

Tem também [uns slides](http://www.slideshare.net/rogerleite14/http-monkey) que apresentei na Abril em alguns tech talks.




** [HTTP Monkey](http://www.slideshare.net/rogerleite14/http-monkey) ** from **[Roger Leite](http://www.slideshare.net/rogerleite14)**




Valeu e aceito numa boa sugestões e críticas referentes ao projeto, por favor comentem! :D
