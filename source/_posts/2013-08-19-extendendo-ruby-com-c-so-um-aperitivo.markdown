---
author: tonyfabeen
comments: true
date: 2013-08-19 17:29:56+00:00
layout: post
slug: extendendo-ruby-com-c-so-um-aperitivo
title: Extendendo Ruby com C - Só um aperitivo
wordpress_id: 1410
categories:
- web
---

Extender Ruby em C não é complicado. É claro, você deve ao menos ter o conhecimento básico da linguagem C.

Vamos criar uma extensão que retorna uma simples String.

Primeiramente criamos o diretório onde estará nossa extensão :

    
      <code>$ mkdir <your_path>/1up4dev</code>


Crie um arquivo chamado **1up4dev.c **e dentro dele inclua o header "**ruby.h"**

    
      <code>#include <ruby.h></code>


Tudo em Ruby relaciona-se com o tipo **VALUE**. Para nosso exemplo, vamos criar um **VALUE m1up4dev** representando um módulo.

    
    <code>VALUE m1up4dev;</code>


E para representar uma classe, abaixo deste módulo, a qual chamaremos de Talker, criaremos uma **VALUE cTalker**:

    
    <code>VALUE cTalker;</code>


Nossa classe **Talker** precisa fazer algo, vamos adicionar uma simples função que retorna uma String.

    
    <code>static VALUE say_yeah(VALUE self){
      const char *sentence= "YEAH YEAH!";
      return rb_str_new2(sentence);
    }
    
    </code>


Na função **say_yeah**, **VALUE self** representa o objeto associado a função, **sentence** a String de retorno e a função **rb_str_new2** converte o ***char **em uma **Ruby String**.

Para deixar esse código acessível no mundo Ruby, criaremos uma função chamada '**Init_1up4dev**'. Por convenção estas funções sempre começam com o prefixo '**Init_**'.

    
    <code>void Init_1up4dev(){
      m1up4dev = rb_define_module("1up4dev");
      cTalker = rb_define_class_under(m1up4dev, "Talker", rb_cObject);
      rb_define_singleton_method(cTalker, "say_yeah", say_yeah, 0);
    }
    
    </code>


A função '**rb_define_module**' define um módulo no topo da hierarquia. Algo como :

    
    <code>module 1up4dev
    end
    
    </code>


A função '**rb_define_class_under**' define uma classe abaixo de um módulo ou outra classe. Isso irá gerar :

    
    <code>module 1up4dev
      class Talker
    
      end
    end
    
    </code>


A função '**rb_define_singleton_method**' é responsável por criar um método singleton em uma classe ou módulo, neste caso ele estará atrelado a class Talker.

Para rodar nosso exemplo, crie um arquivo chamado '**extconf.rb**' contendo :

    
    <code>
    require 'mkmf'
    create_makefile('1up4dev')
    
    </code>


Executando o script, irá ser gerado um arquivo **Makefile** para executar o build da extensão .

    
    <code>$ ruby extconf.rb
    </code>


Compile e instale a extensão :

    
    <code>$ make && make install
    </code>


Para ver o código funcionando basta digitar o código abaixo em um '**irb**' ou algo do gênero :

    
    <code>$irb(main):001:0> require '1up4dev'
    true
    $irb(main):002:0> 1up4dev::Talker.say_yeah
    "YEAH YEAH!"
    
    </code>


YEAH YEAH !!
