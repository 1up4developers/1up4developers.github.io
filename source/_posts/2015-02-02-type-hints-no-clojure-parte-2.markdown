---
layout: post
title: "Type hints no Clojure - Parte 2"
date: 2015-02-02 17:14
author: 'pbalduino'
comments: true
categories:
- clojure
tags:
- clojure
- programacao
---
Parte 1: http://1up4dev.org/2015/01/type-hints-no-clojure-parte-1/

## Quando otimizar

Não adianta sair adicionando _type hints_ no código sem critério. A primeira coisa a ser feita é detectar onde estão os gargalos, os pontos que consomem mais recursos sem trazer resultados imediatos utilizando uma boa ferramenta de ::profiling::.

Uma vez que você localize os pontos lentos da aplicação, adicione a seguinte linha no início do seu código:

    (set! *warn-on-reflection* true)

Agora o compilador vai nos avisar toda vez que for obrigado a usar _reflection_ para invocar um método ou acessar um membro.

Vamos usar a nossa função `upper-case` para detectar os pontos que podem ser otimizados com _type hinting_.

    (set! *warn-on-reflection* true)
    
    (defn upper-case [text]
      (.toUpperCase text))
    ; Reflection warning, NO_SOURCE_PATH:2:3 - reference to field 
    ; toUpperCase can't be resolved.
    ; #'user/upper-case

Essa é a mensagem do compilador nos avisando que não sabe que tipo contém o método `toUpperCase`. Como estamos usando `toUpperCase` em `text`, vamos adicionar o _type hint_ nele.

Perceba que o erro ocorreu durante a compilação da função, e não durante sua execução. Lembre-se que todo código executado já foi compilado em algum momento, incluindo o código que escrevemos no _REPL_.

    (defn upper-case-2 [^String text]
      (.toUpperCase text))
    ; #'user/upper-case-2
    
    (upper-case-2 "umba umba umba ê")
    ; "UMBA UMBA UMBA Ê"

Sem erros nem avisos, resolvemos a questão do _reflection_ na expressão.

Falamos sobre otimização e gargalos mas não mostramos na prática o que isso significa.

Vamos posicionar as duas funções, com e sem _type hint_ e executar uma iteração apresentando o tempo gasto com cada versão. 

    (defn upper-case [texto]
      (.toUpperCase texto))
    ; Reflection warning, NO_SOURCE_PATH:2:3 - reference to field 
    ; toUpperCase can't be resolved.
    ; #'user/upper-case
    
    (defn upper-case-2 [^String texto]
      (.toUpperCase texto))
    ; #'user/upper-case-2
    
    (time (dotimes [_ 100000] (upper-case "mandarina")))
    ; "Elapsed time: 327.720765 msecs"
    
    (time (dotimes [_ 100000] (upper-case-2 "mandarina")))
    ; "Elapsed time: 55.403849 msecs"

Nesse nosso exemplo simples o processamento ficou de seis a sete vezes mais rápido.

_(Trecho do meu livro "Clojure: Programação Funcional Descomplicada para a JVM", a ser publicado em breve)_