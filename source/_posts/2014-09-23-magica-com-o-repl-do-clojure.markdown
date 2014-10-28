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

Caso você não esteja íntimo com o REPL do Clojure, recomendo a leitura do [texto anterior](http://1up4dev.org/2014/04/alguns-truques-com-o-repl-do-clojure/) para aprender o básico.

É recomendado também que você leia as mensagens emitidas pela aplicação com a voz do [Cid Moreira](http://pt.wikipedia.org/wiki/Cid_Moreira). Para uma versão em inglês do artigo estou considerando sugerir as vozes do Morgan Freeman ou do Stephen Fry.

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

Agora vamos editar o arquivo `core.clj`, que está dentro de `src/misterm`. Perceba que o nome da aplicação é usado por padrão como parte do nome do __namespace__.

Encontraremos um arquivo assim:

    (ns misterm.core)
    
    (defn foo
      "I don't do a whole lot."
      [x]
      (println x "Hello, World!"))

Aqui está sendo declarado o __namespace__ `misterm.core`, e em seguida uma função `foo` que simplesmente imprime o texto __Hello, World!__.

Vamos modificar a declaração do __namespace__ para que possamos usar o `nREPL` em nossa aplicação.

	(ns misterm.core
		(:require [clojure.tools.nrepl.server :as nrepl]))

Em seguida vamos iniciar o servidor do `nREPL` na porta 12345.

	(defonce server (nrepl/start-server :port 12345))

Estamos usando `defonce` aqui para garantir que, ao acessarmos a aplicação via `nREPL`, o servidor não seja iniciado mais de uma vez. Caso isso aconteça teremos um erro porque a porta 12345 já estará em uso.

Vamos apagar a função `foo` e vamos criar uma chamada `magic`:

	(defn magica []
	  (Thread/sleep 2000)
	  (println "Mostre-nos o seu segredo, Mister M"))

Nossa função simplesmente espera dois segundos e, sem seguida, imprime na tela um texto.

Agora vamos criar a função `-main`, que serve como __entrypoint__ da aplicação, assim como o método `main` de uma aplicação Java.

	(defn -main [& args]
	  (loop []
	    (magica)
	    (recur)))

O argumento `& args` indica que a função `-main` aceita uma quantidade indefinida de parâmetros, ou mesmo nenhum, exatamente como acontece com o método `main(String ... args)` do Java.

Os operadores `loop` e `recur` são usados para simularmos __tail call recursion__ no Clojure, o que é assunto para outro texto. Para o que precisamos, basta saber que criamos um loop infinito com eles. Dentro desse loop infinito estamos invocando a função `magica`.

Tudo pronto, vamos salvar o arquivo e voltar para o terminal. Execute o comando `lein run` para executar a aplicação e a cada dois segundos será exibida a mensagem. Lembre-se de usar a voz do Cid Moreira ao ler.

	$ lein run
	Mostre-nos o seu segredo, Mister M
	Mostre-nos o seu segredo, Mister M
	Mostre-nos o seu segredo, Mister M
	Mostre-nos o seu segredo, Mister M
	Mostre-nos o seu segredo, Mister M

Vamos abrir um novo terminal e executar o comando `lein repl :connect 127.0.0.1:12345`.

	$ lein repl :connect 127.0.0.1:12345
	Connecting to nREPL at 127.0.0.1:12345
	REPL-y 0.3.5, nREPL 0.2.6
	Clojure 1.6.0
	Java HotSpot(TM) 64-Bit Server VM 1.8.0-b132
	    Docs: (doc function-name-here)
	          (find-doc "part-of-name-here")
	  Source: (source function-name-here)
	 Javadoc: (javadoc java-object-or-class-here)
	    Exit: Control+D or (exit) or (quit)
	 Results: Stored in vars *1, *2, *3, an exception in *e

	user=>

O prompt indica que estamos no namespace padrão do Clojure, chamado `user`. Vamos para o namespace onde estão as nossas funções para verificar se estamos mesmo conectados na aplicação.

	(ns misterm.core)

Agora vamos executar a função `magica`. Se tudo foi feito corretamente até agora, vamos esperar dois minutos e então ler a mensagem __Mostre-nos o seu segredo, Mister M__.

	(magica)
	; Mostre-nos o seu segredo, Mister M

Tudo certo. Agora vamos modificar a aplicação enquanto ela está sendo executada.

Dê uma olhada no primeiro terminal. A essa altura a nossa mensagem já encheu a tela.

De volta ao segundo terminal, o que está com o REPL aberto, reescreva o conteúdo da função `magica`:

	(defn magica []
	  (Thread/sleep 1000)
	  (println "Aaaaaahhhhh, Mister M!"))

A função `magica` já existia, mas estamos redefinindo em tempo de execução. Se você tentar fazer isso no IRB, por exemplo, receberá uma mensagem de erro.

Agora olhe novamente no primeiro terminal, aquele que estava com a tela cheia de mensagens.

	Mostre-nos o seu segredo, Mister M
	Mostre-nos o seu segredo, Mister M
	Mostre-nos o seu segredo, Mister M
	Mostre-nos o seu segredo, Mister M
	Mostre-nos o seu segredo, Mister M
	Aaaaaahhhhh, Mister M!
	Aaaaaahhhhh, Mister M!
	Aaaaaahhhhh, Mister M!

A mensagem mudou! Não só isso. Agora ela leva um segundo para ser exibida na tela, ao invés dos dois originais. Nós mudamos a aplicação enquanto ela estava sendo executada.

Claro que toda mágica tem lá seus pontos a serem considerados. Se você reiniciar a aplicação a mensagem original será novamente exibida, já que alteramos a aplicação enquanto ela estava sendo executada, e não seu código fonte.

Esse recurso é muito útil para corrigir erros sem derrubar a aplicação, para desenvolver aplicações de maneira verdadeiramente incremental e iterativa, e também para aumentarmos a velocidade do ciclo de desenvolvimento ao máximo. Pense que dessa forma não precisamos mais derrubar a aplicação, recompilar, executar novamente, aguardar o tempo de carga da JVM, que não é pouco. Numa visão otimista, caso você ganhe dez segundos em cada iteração do seu ciclo de desenvolvimento, ao final do dia você terá economizado muito tempo, além de ter mantido o foco naquilo que realmente importa. É sabido que essas mudanças de contexto, por mais rápidas que sejam, atrapalham a concentração e a produtividade.

Editores como __Emacs__ e __ViM__ têm plugins que se conectam ao `nREPL`, fazendo com que você nem ao menos precise fechar a janela de edição de código para alterar a aplicação. Já o __Lighttable__ oferece um modo chamado __InstantREPL__, onde seu código é avaliado na própria edição, exibindo os valores enquanto você programa.

O REPL é uma ferramenta poderosa não só no Clojure, mas em praticamente todos os dialetos LISP. Experimente fazer isso com a sua linguagem preferida e veja se consegue ter tanto ganho de produtividade.

P.S.: Jon Garret conseguiu inclusive corrigir um bug em um satélite em órbita graças ao uso do REPL. Leia mais [aqui](http://www.flownet.com/gat/jpl-lisp.html).
