---
author: rodrigopanachi
comments: true
date: 2012-08-29 11:00:07+00:00
layout: post
slug: quicktips-migrando-sua-app-rails-do-mysql-para-postgresql-sem-dor
title: '[QuickTips] Migrando sua app Rails do MySQL para PostgreSQL sem dor'
wordpress_id: 1217
categories:
- quick tips
- rails
- real world
- web
tags:
- mysql
- postgresql
- rails
---

## Motivação


Meu primeiro contato com banco de dados relacionais foi o PostgreSQL, ou Postgres, nos tempos da faculdade. Achei muito complicado na época, difícil de instalar, administrar, etc. Quando comecei a programar profissionalmente, por volta de 2004, conheci o [MySQL](http://www.mysql.com/) e gostei muito da sua simplicidade de configuração e ótima performance. Passou a ser o banco de dados padrão para a maioria dos projetos pessoais ou onde havia possibilidade de escolha.

Tudo andava bem, até que a [Oracle comprou a Sun Microsystems em 2009](http://info.abril.com.br/noticias/negocios/oracle-compra-sun-por-us-7-4-bilhoes-20042009-5.shl). Não é segredo que nunca fui fã da Oracle, e considerando os acontecimentos recentes com o [Java](http://br-linux.org/2011/oracle-descontinua-licenca-que-permitia-incluir-sua-java-vm-em-distribuicoes-linux/) e o [OpenOffice](http://idgnow.uol.com.br/ti-corporativa/2011/04/19/oracle-desiste-do-openoffice/), não demoraria muito para o MySQL também receber seu [toque de Midas da escrotização](http://blog.mariadb.org/disappearing-test-cases/).

Dentre várias alternativas, optei pelo [PostgreSQL](http://www.postgresql.org/) pela sua performance, muitas vezes comparável com o Oracle DB, suporte da comunidade, e por ser [Open Source](http://en.wikipedia.org/wiki/Open-source_software).


## Instalando o Postgres


Se você estiver no Ubuntu, basta rodar:

    
    $ sudo apt-get install postgresql-9.1 libpq-dev


Finalizada a instalação, inicie o serviço e abra o client do Postgres:

    
    $ sudo service postgresql start
    $ sudo -u postgres psql


Aproveite o terminal e mude a senha do usuário _postgres_:

    
    # \password


Para sair, digite:

    
    # \q




## Migrando o banco


Estou usando a gem [mysql2psql](http://rubygems.org/gems/mysql2psql), que embora esteja deprecated, funciona muito bem e não tem dependência do stack do Rails, como o _activerecord_. Assim é possível rodar a migração fora do contexto da aplicação e descartar o MySQL quando terminar.

O funcionamento é simples: conecta no banco MySQL de origem e copia a estrutura e os dados para um banco Postgres destino configurado. Para instalar:

    
    $ sudo gem install mysql2psql


Em seguida, execute:

    
    $ mysql2psql


Isto criará o arquivo _mysql2psql.yml_ com a estrutura de configuração necessária. Edite este arquivo para ficar assim:

    
    mysql:
     hostname: localhost
     port: 3306
     socket: /var/run/mysqld/mysqld.sock
     username: root
     password: root
     database: app_development
    destination:
     postgres:
      hostname: localhost
      port: 5432
      username: postgres
      password: postgres
      database: app_development


Agora crie o banco de destino no Postgres:

    
    $ sudo psql -h localhost -U postgres -W
    # CREATE DATABASE app_development;


Saia do client com _\q_ e então rode novamente:

    
    $ mysql2psql


Se as configurações estiverem corretas, a estrutura do banco e os dados serão migrados. O output será parecido com:

    
    Creating table comments...
    Created table comments
    Creating table schema_migrations...
    Created table schema_migrations
    Creating table users...
    Created table users
    
    Counting rows of comments... 
    Rows counted
    Loading comments...
    5 rows loaded in 0min 0s
    
    Counting rows of schema_migrations... 
    Rows counted
    Loading schema_migrations...
    20 rows loaded in 0min 0s
    
    Counting rows of users... 
    Rows counted
    Loading users...
    418 rows loaded in 0min 0s
    
    Indexing table comments...
    Indexed table comments
    Indexing table schema_migrations...
    Indexed table schema_migrations
    Indexing table users...
    Indexed table users
    
    Table creation 0 min, loading 1 min, indexing 0 min, total 1 min




## Configurando a app


Para finalizar, remova a gem do _mysql_, adicione a do Postgres no _Gemfile_ e configure o _database.yml_ com as informações do Postgres:

    
    # Gemfile
    gem 'pg'



    
    # database.yml
    development:
      adapter: postgresql
      host: localhost
      database: app_development
      username: postgres
      password: postgres


Basta rodar _bundle install_ e iniciar sua aplicação usando PostgreSQL!

Se precisar ou preferir rodar uma migração no contexto da sua app, dê uma olhada na gem [mysql-to-postgres](https://github.com/maxlapshin/mysql2postgres), do mesmo autor.

Caso tenha alguma dúvida ou sugestão, deixe seu comentário.
