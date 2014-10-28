---
layout: post
title: "Alguns truques com o REPL do Clojure"
date: 2014-04-25 11:21
author: pbalduino
slug: alguns-truques-com-o-repl-do-clojure
comments: true
categories: 
- clojure
- lisp
- quick tips
tags:
- quick tips
- clojure
- lisp
- REPL
---

O <tt>REPL</tt> é uma das ferramentas mais úteis para se programar em Clojure. Se você está chegando do Ruby ou do Python está mais do que acostumado a usar o <tt>IRB</tt> ou o modo interativo do Python. Veremos no decorrer do capítulo que o REPL é bem mais do que um prompt da linguagem, que serve apenas para que instruções sejam testadas.


Existem alguns atalhos e funções auxiliares que tornam o uso do REPL bem mais produtivo. Por mais que escrever diretamente no REPL não seja tão confortável quando no seu editor preferido, algumas vezes isso acaba sendo necessário.

### Qual é mesmo o nome daquela função?

As funções da biblioteca padrão do Clojure vem com um texto explicativo, onde você pode se situar sobre como utilizá-las.

Podemos pesquisar alguma palavra que estiver dentro desses textos para encontrar a função que queremos, mas não lembramos o nome. Para isso, usamos <tt>find-doc</tt>, seguido da palavra ou trecho de texto relacionado ao que queremos.

Vamos supor que eu esteja procurando algo sobre ::sockets::. Basta digitar <tt>(find-doc "socket")</tt> no REPL.

    (find-doc "socket")
    ; -------------------------
    ; clojure.tools.nrepl/connect
    ; ([& {:keys [port host transport-fn], :or {transport-fn
    ;  transport/bencode, host "localhost"}}])
    ;  Connects to a socket-based REPL at the given host (defaults to
    ;  localhost) and port, returning the Transport (by default clojure.

    ; e mais um monte de coisas

No nosso exemplo, encontramos a função <tt>connect</tt>, que está no namespace <tt>clojure.tools.nrepl</tt>.

Se você lembra de alguma parte do nome da função, então pode usar a função <tt>apropos</tt>, passando como parâmetros o trecho do nome ou uma expressão regular. Não se preocupe com expressões regulares agora, pois veremos esse assunto em detalhes mais para frente.

Vamos supor que eu esteja manipulando vetores e não lembre o nome da função, mas saiba que a estrutura chama-se <tt>vector</tt>:

    (apropos "vector")
    ; (vector-of vector vector? vector-zip)

E agora você pode usar a função <tt>doc</tt> para ver a documentação daquela que mais se parecer com o que você estiver procurando:

    (doc vector?)
    ; -------------------------
    ; clojure.core/vector?
    ; ([x])
    ;   Return true if x implements IPersistentVector

Existe uma variação de <tt>apropos</tt> chamada <tt>apropos-better</tt>, que informa também o namespace da função quando ela não estiver dentro do namespace <tt>clojure.core</tt> ou dentro do namespace em que você estiver no momento:

    (apropos-better "vector")
    ; (vector vector-of vector? clojure.zip/vector-zip)

### Um pouco de Bash na sua vida

Quando você usa o REPL por dentro do Leiningen, alguns atalhos já conhecidos pelos usuários de Bash estão disponíveis, mesmo para quem está usando o Leiningen no Windows.

O primeiro deles é a tecla ::TAB::, que exibe os nomes de funções que começam com o que você já digitou.

Por exemplo, vou digitar <tt>map</tt> e pressionar ::TAB::

    (map
    ; map           map-indexed   map?          mapcat        mapv

Outra combinação que agiliza bastante o trabalho é a combinação ::Control L::, ou ::Command L:: se você estiver usando MacOS, que limpa os resultados das expressões anteriores e mantém apenas a expressão que você estiver digitando no momento.

Existe também a combinação ::Control R::, ou ::Command R::, que completa o que você estiver digitando usando o histórico de comandos do REPL. Pressionando essa combinação mais de uma vez vai alternar entre todas as combinações já utilizadas que contenham o texto que você já digitou.

Usar as setas ::para cima:: ou ::para baixo:: permite que você navegue nos comandos utilizados recentemente.

### Recuperando os últimos resultados

Existem também símbolos especiais que guardam os resultados das últimas expressões e exceções. Eles são <tt>*1</tt>, <tt>*2</tt> e <tt>*3</tt> para os valores e <tt>*e</tt> para a última exceção, ou erro, que ocorreu:

    (+ 1 2)
    ; 3

    (* 2 4)
    ; 8

    (/ 8 2)
    ; 4

    (println "Resultados anteriores:" *1 *2 *3)
    ; Resultados anteriores: 4 8 3

    (/ 1 0)
    ; ArithmeticException Divide by zero

    (println "Último erro:" *e)
    ; Último erro: #<ArithmeticException java.lang.ArithmeticException:
    ;   Divide by zero>

### Consultando o código fonte

Algumas vezes é bom ter acesso ao código fonte de determinada função ou macro para que possamos entender melhor como ela funciona. Enquanto eu escrevia este artigo, fiz isso constantemente para descobrir como as coisas funcionam por baixo dos panos.

Infelizmente, nem sempre é simples ir até o site onde o código fonte do Clojure está disponível e procurar o arquivo em que aquela função está definida. 

Pior ainda quando a versão que está lá é diferente da versão que você está usando no momento. E fica ainda pior quando você não tem acesso ao código fonte da biblioteca que estiver utilizando.

Para nos ajudar, existe a macro <tt>source</tt>, que recebe como parâmetro o nome da função, sem aspas, e exibe o respectivo código fonte, quando possível.

Existem casos em que isso não é possível, como quando você tentar ler o fonte de uma forma especial ou de um código que foi compilado utilizando AOT (veremos isso em detalhes mais para frente).

Vamos exibir o código fonte da função <tt>+</tt>, responsável por somar dois ou mais números:

    (source +)
    ; (defn +
    ;   "Returns the sum of nums. (+) returns 0. Does not auto-promote
    ;   longs, will throw on overflow. See also: +'"
    ;   {:inline (nary-inline 'add 'unchecked_add)
    ;    :inline-arities >1?
    ;    :added "1.2"}
    ;   ([] 0)
    ;   ([x] (cast Number x))
    ;   ([x y] (. clojure.lang.Numbers (add x y)))
    ;   ([x y & more]
    ;      (reduce1 + (+ x y) more)))

Note que temos acesso a todos os detalhes internos da função <tt>+</tt>, incluindo sua documentação e mais algumas informações que são úteis para o compilador ou para alguma função que gere documentação automaticamente.

Ao tentarmos ver o código fonte de uma forma especial ou de algum código escrito nativamente em Java, receberemos uma mensagem de que o código fonte não foi encontrado:

    (source Thread/sleep)
    ; Source not found

Há muito mais recursos no REPL do Clojure, inclusive no que diz respeito a integração com o seu editor preferido, mas vou deixar isso para outro artigo.