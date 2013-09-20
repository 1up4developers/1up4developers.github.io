---
author: rogerbarreto
comments: true
date: 2012-11-28 15:49:06+00:00
layout: post
slug: minionserver-a-real-server-to-mock-servers
title: MinionServer - a real server to mock servers!
wordpress_id: 1238
categories:
- quick tips
- rails
- real world
- ruby
- web
tags:
- http server
- integration
- minion_server
- mocks
- rubygem
- test
- test integration
- tiny http server
---

[MinionServer](https://github.com/rogerleite/minion_server) is a ruby gem to help you with integration tests. You can create an app using Rack Builder and start a tiny server very easy. Let me show you some code:


{% codeblock lang:ruby %}
require 'minion_server'

# build your integration app
IntegrationApp = Rack::Builder.new do
  map "/" do
    run lambda { |env|
      [200, {"Content-Type" => "text/plain"}, ["Be happy!"]]
    }
  end
end

server = MinionServer.new(IntegrationApp)
server.start("localhost", 1620)  # default: localhost, 4000

# do your calls
system "curl http://localhost:1620" # => "Be happy!"

server.shutdown
{% endcodeblock %}


You can see more examples at [http_monkey](https://github.com/rogerleite/http_monkey)'s integration tests.
Hope that helps!

**pt-br moment**: Está em inglês porque eu publiquei no [coderwall](https://coderwall.com/p/ibr4ig) e depois tive a idéia brilhante de postar aqui, com a preguiça mais brilhante ainda de traduzir em pt-br.
