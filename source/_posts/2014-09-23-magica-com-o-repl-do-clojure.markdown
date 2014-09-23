---
layout: post
title: "Fazendo mágica com o REPL do Clojure"
date: 2014-09-23 18:24
author: 'pbalduino'
comments: true
categories:
- clojure
- lisp
- quick tips
tags:
- quick tips
- clojure
- lisp
- leiningen
- repl
---
# Fazendo mágica com o REPL do Clojure

Caso você não esteja íntimo com o REPL do Clojure, recomendo a leitura do [texto anterior](http://1up4dev.org/2014/04/alguns-truques-com-o-repl-do-clojure/) para aprender o básico.

As pessoas me perguntam se um REPL é equivalente ao IRB do Ruby ou o modo interativo do Python. No artigo anterior eu comentei que o REPL é bem mais do que isso, mas ainda não fui convincente o bastante.

Nesse artigo eu pretendo demonstrar como podemos modificar a aplicação enquanto ela é executada, sem necessidade de reiniciá-la ou de esperar para que a compilação termine.

Primeiro, vamos instalar uma ferramenta chamada __Leiningen__, que pode ser facilmente instalada a partir de http://leiningen.org/#install.

O __Leiningen__ é uma ferramenta que automatiza uma série de processos cotidianos do desenvolvimento de uma aplicação, além de também cuidar das dependências da aplicação. Pense nele como um primo bonito do __Maven__ ou do __Gradle__.

Primeiro, vamos criar uma aplicação usando o __Leiningen__.

No terminal, digite:

    lein new misterm

Se for a primeira vez que você utiliza o __Leiningen__, algumas dependências serão baixadas para a sua máquina.

    $ lein new misterm
    Generating a project called misterm based on the 'default' template.
    The default template is intended for library projects, not applications.
    To see other templates (app, plugin, etc), try `lein help new`.

Será criado um diretório chamado `misterm`, onde teremos nossa aplicação, e dentro desse diretório, edite um arquivo `project.clj`.

    ; O seu arquivo project.clj vai ter essa cara
    (defproject misterm "0.1.0-SNAPSHOT"
      :description "FIXME: write description"
      :url "http://example.com/FIXME"
      :license {:name "Eclipse Public License"
                :url "http://www.eclipse.org/legal/epl-v10.html"}
      :dependencies [[org.clojure/clojure "1.6.0"]])

O conteúdo do arquivo `project.clj` nada mais é do que um código Clojure válido, sendo `defproject` uma **macro** que armazena as configurações do projeto em algum estado global. Vou deixar para explicar macros em outro artigo.

Para que a mágica funcione, vamos adicionar o `nREPL` na nossa aplicação. `nREPL` é uma biblioteca que permite que o REPL do Clojure seja acessado remotamente, normalmente através de uma porta TCP. O próprio REPL do Clojure disponibilizado pelo Leiningen já vem com o nREPL, como vamos ver mais para frente.

Para criarmos um projeto executável, precisamos informar no projeto em qual __namespace__ estará o __entrypoint__ da aplicação. Pense num __namespace__ como um __package__ do Java, apesar de existirem grandes diferenças entre ambos. Para a nossa explicação, considerar um como o outro basta.

Vamos editar o arquivo, adicionando a nova dependência dentro do vetor `:dependencies` e também a opção `:main misterm.core` antes do parêntese final.

O final do arquivo vai ficar assim:

    ; final do arquivo project.clj
      :dependencies [[org.clojure/clojure "1.6.0"]
                     [org.clojure/tools.nrepl "0.2.5"]]
      :main misterm.core)

