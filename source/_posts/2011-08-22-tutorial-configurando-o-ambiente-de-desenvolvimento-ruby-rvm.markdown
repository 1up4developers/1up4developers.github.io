---
author: rodrigopanachi
comments: true
date: 2011-08-22 09:00:19+00:00
layout: post
slug: tutorial-configurando-o-ambiente-de-desenvolvimento-ruby-rvm
title: '[Tutorial] Configurando o ambiente de desenvolvimento Ruby: RVM'
wordpress_id: 1069
categories:
- quick tips
- ruby
- tutorial
- web
---

Se você ainda não trabalha com [Ruby](http://www.ruby-lang.org/pt/) pois acha complicado demais instalá-lo, chega de desculpas! Esse tutorial <del>for dummies</del> ajudará a instalar o Ruby através do [RVM](http://beginrescueend.com/) de forma direta, sem enrolação.

Partindo de uma instalação do [Ubuntu](http://www.ubuntu.com/) 11.04 zerada, começamos com os pré-requisitos para instalar o RVM:

    
    user@ubuntu:~$ sudo apt-get install git-core curl


Basta confirmar a instalação dos pacotes, em seguida executar o comando (extraído do site oficial do RVM):

    
    user@ubuntu:~$ bash < <(curl -s https://rvm.beginrescueend.com/install/rvm)


A seguir, o output completo gerado por esse comando indicará os pacotes e libs necessários para a instalação do Ruby.

    
    (...)
    
    dependencies:
    # For RVM
      rvm: bash curl git
    
    # For Ruby (MRI & ree)  you should install the following OS dependencies:
      ruby: /usr/bin/apt-get install <strong>build-essential bison openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-0 libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf libc6-dev ncurses-dev automake</strong>
    
    (...)
    
    Installation of RVM to /home/user/.rvm/ is complete.


Agora, basta instalar essas libs através do apt-get:

    
    user@ubuntu:~$ sudo apt-get install build-essential bison openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-0 libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf libc6-dev ncurses-dev automake


Com as dependências instaladas, abra um novo console (para re-carregar o RVM), e comece a instalar o Ruby, no caso, o MRI 1.8.7:

    
    user@ubuntu:~$ rvm install 1.8.7


Esta operação levará alguns minutos. Quando concluída, você terá o Ruby 1.8.7 e o [Rubygems](http://rubygems.org/) 1.8.6 instalados. Para testar tudo, execute os seguintes comandos:

    
    user@ubuntu:~$ rvm use --default 1.8.7
    Using /home/user/.rvm/gems/ruby-1.8.7-p352



    
    user@ubuntu:~$ ruby -v
    ruby 1.8.7 (2011-06-30 patchlevel 352) [i686-linux]



    
    user@ubuntu:~$ gem -v
    1.8.6


Pronto! Agora você tem o Ruby e Rubygems instalados e prontos para o uso. Caso queira conferir, o output dos comandos acima está disponível [neste gist](https://gist.github.com/1157378).

Fique ligado no próximo post: Gemsets e Bundler. Qualquer dúvida, use os comentários.
