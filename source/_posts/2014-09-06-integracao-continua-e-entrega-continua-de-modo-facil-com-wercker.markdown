---
layout: post
title: "Integração Contínua e Entrega Contínua de modo fácil com Wercker"
slug: "integracao-continua-e-entrega-continua-de-modo-facil-com-wercker"
date: 2014-09-06 12:33
author: rodrigopanachi
comments: true
categories: [continuous integration, continuous delivery, agile]
---

**TL;DR** [Wercker](http://wercker.com/) é uma plataforma de integração contínua, baseado em
containers e focado em facilitar o build e deploy de aplicações na nuvem. Totalmente
integrado com [Github](http://github.com), [BitBucket](BitBucket), [Heroku](Heroku),
[AWS](http://aws.amazon.com/) e [OpenShift](https://www.openshift.com/), em questão de minutos e com apenas alguns cliques
é possível configurar o build e deploy da sua aplicação. O serviço está em beta, permitindo
**repositórios privados grátis**!

![](/images/posts/wercker-1.png)

## Como funciona

Cada vez que você faz um `git push`, o Wercker recebe um sinal que houve atualização,
baixa o código do repositório, monta o ambiente do build definindo variáveis de ambiente,
rodando as migrations, etc, e executa os testes ou qualquer outro passo que você definir, como compilar os assets, etc.

Uma vez que o build execute com sucesso, fica disponível para deploy na plataforma que escolher,
como Heroku ou AWS. Da mesma forma do build, o deploy consiste em passos definidos, como rodar o
Capistrano ou fazer push no repositório do Heroku.

Durante as duas etapas deste processo de build e deploy, o Wercker ainda pode notificar seu
[HipChat](https://www.hipchat.com/) ou [Slack](https://slack.com/) :)

![](/images/posts/wercker-2.png)

## Por que é diferente?

Existem vários serviços de CI com a mesma proposta, como o [Travis](https://travis-ci.org/) ou o
[Codeship](https://www.codeship.io/). Mas o Wercker permite total customização do ambiente do build
e do deploy, uma vez que é baseado em containers.

Você pode escolher um container oficial para seu build, como Ubuntu com Ruby instalado, por
exemplo, ou pode procurar um container disponibilizado por outro usuário no [diretório do Wercker](https://app.wercker.com/#explore/boxes/search/).

Mas se precisar de algo muito específico, você pode criar seu próprio container (ou box) com pacotes e serviços
que desejar, versões, etc, e utilizá-lo no seu build. E você também pode disponibilizar este box
no diretório do Wercker para outros usuários utilizarem!

## Boxes

Um box é um ambiente virtual empacotado e versionado com pacotes e serviços necessários para executar o build.
Em outras palavras, é o container "base" que rodará seu build.

Para utilizar um box, basta declarar no início do `wercker.yml`:

{% codeblock %}

box: wercker/ubuntu12.04-ruby2.0.0

{% endcodeblock %}

Tem box para [Ruby](http://devcenter.wercker.com/articles/languages/ruby.html),
[Node.js](http://devcenter.wercker.com/articles/languages/nodejs.html),
[Go](http://devcenter.wercker.com/articles/languages/go.html),
[Java/Android](http://devcenter.wercker.com/articles/languages/android.html)
e até para [Docker](http://blog.wercker.com/2013/11/28/Announcing-docker-support.html). Na prática,
os boxes oficiais vão atender a maioria dos casos, mas você pode criar seu próprio box como desejar.

Para definir um box, basta criar um repositório com o arquivo `wercker-box.yml` descrevendo a base
(Ubuntu, por exemplo), o provisionamento ([Chef](http://www.getchef.com/chef/),
[Puppet](http://puppetlabs.com/), etc, e adicioná-lo ao build pipeline do Wercker.

Uma vez que o build passar, é possível fazer o deploy para disponibilizá-lo no diretório do
Wercker. Assim, é possível utilizar este box no processo de build da sua aplicação.

Mais informações: [http://devcenter.wercker.com/articles/boxes/](http://devcenter.wercker.com/articles/boxes/)

## Services

Serviços são boxes prontos disponibilizados pelo Wercker, como MySql, PostgreSql, MongoDB, Redis,
etc. Em outras palavras, é um container configurado com um serviço ready to use.

Para utilizar um service, basta adicionar ao seu `wercker.yml`:

{% codeblock %}

box: wercker/ruby
services:
    - wercker/mongodb
    - wercker/redis

{% endcodeblock %}

Mais informações: [http://devcenter.wercker.com/articles/services/](http://devcenter.wercker.com/articles/services/)

## Steps

As steps são os "comandos" executados pelo Wercker no build ou deploy da aplicação, como
`bundle-install`, `database-migration`, `script`, etc.

Você pode utilizar as steps disponibilizadas pelo Wercker ou criar suas próprias steps,
executando os comandos que julgar necessário no seu build ou deploy.

Para definir as steps do seu build, basta adicionar o `wercker.yml`:

{% codeblock %}

build:
    steps:
        - rvm-use:
            version: 2.1.2
        - bundle-install
        - rails-database-yml
        - script:
            name: db migrate
            code: bundle exec rake db:migrate
        - script:
            name: rspec
            code: bundle exec rspec

{% endcodeblock %}

Mais informações: [http://devcenter.wercker.com/articles/steps/](http://devcenter.wercker.com/articles/steps/)

## Deploy pipeline

O Wercker permite fazer o deploy da sua aplicação em vários [PaaS](http://en.wikipedia.org/wiki/Platform_as_a_service)
como [Heroku](https://www.heroku.com/) automaticamente ou definir o processo de deploy manualmente,
utilizando o [Capistrano](http://capistranorb.com/) ou executando um script de sua preferência.

Durante a processo de deploy, é possível definir [chaves SSH](http://devcenter.wercker.com/articles/gettingstarted/repositoryaccess.html) específicas, [variáveis de ambiente](http://devcenter.wercker.com/articles/steps/variables.html), etc.

Para configurar seu deploy, adicione ao `wercker.yml` (por exemplo):

{% codeblock %}

deploy:
    steps:
        - heroku-deploy
        - s3sync:
            key_id: $AWS_ACCESS_KEY_ID
            key_secret: $AWS_SECRET_ACCESS_KEY
            bucket_url: $AWS_BUCKET_URL
            source_dir: build/

{% endcodeblock %}

Mais informações: [http://devcenter.wercker.com/articles/deployment/](http://devcenter.wercker.com/articles/deployment/)

## #comofas?

O Wercker é legal, fácil, bla bla bla. Vamos à um exemplo prático, passo-a-passo, de como configurar
seu projeto no Wercker, fazer o build e o deploy automaticamente no Heroku.

0 - [Crie sua conta](https://app.wercker.com/users/new) :)

1 - Escolha o repositório, adicione as chaves SSH, finalize;

![](/images/posts/wercker-gs.png)

2 - Adicione o arquivo `wercker.yml` ao seu projeto, parecido com este:

{% codeblock %}

box: wercker/rvm
services:
    - wercker/postgresql
build:
    steps:
        - rvm-use:
            version: 2.1.2
        - bundle-install
        - rails-database-yml
        - script:
            name: db migrate
            code: bundle exec rake db:migrate RAILS_ENV=test
        - script:
            name: rspec
            code: bundle exec rspec -f d
    after-steps:
        - hipchat-notify:
            token: $HIPCHAT_TOKEN
            room-id: 123456
            from-name: wercker

{% endcodeblock %}

Mais informações: [http://devcenter.wercker.com/articles/werckeryml](http://devcenter.wercker.com/articles/werckeryml)

3 - Dispare o build com `git push`;

{% codeblock %}

git commit -m "Configurando Wercker"
git push origin master

{% endcodeblock %}

4 - Configure o deploy no Heroku:

![](/images/posts/wercker-deploy.png)

5 - Dispare o build/deploy com `git push` ou manualmente:

![](/images/posts/wercker-deploy-build.png)

6 - \o/

## Espalhe a palavra!

Fácil né? Não há mais ~~choro~~ desculpas para não utilizar um CI em seu projeto.

Configure seu projeto, tire um print screen do primeiro build com sucesso e Tuite para o
[@wercker](https://twitter.com/Wercker) para receber stickers _de grátis_!

Compartilhe este post! Dúvidas e sugestões nos comentários abaixo. Sucesso!
