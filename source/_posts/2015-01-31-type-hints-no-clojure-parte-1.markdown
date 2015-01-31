---
layout: post
title: "Type hints no Clojure - Parte 1"
date: 2015-01-31 21:14
author: 'pbalduino'
comments: true
categories:
- clojure
tags:
- clojure
- programacao
---
## Otimizando com type hints

Por padrão você não informa os tipos dos dados ao Clojure. Internamente o dado vai ser tratado como `Object`, que é a classe base de qualquer objeto Java.

Qualquer método ou propriedade que você invocar desse objeto será localizado e chamado através de _reflection_. No Java, _reflection_ é a capacidade de acessar métodos e membros de um objeto ou classe através de metaprogramação.

Exemplificando, esse código Clojure:

    (defn upper-case [text]
      (.toUpperCase text))
    
    (upper-case "umba umba umba ê")
    ; "UMBA UMBA UMBA Ê"

Vai ser transformado em algo equivalente a isso:

    public Object invoke(Object par01) {
      return (Object)(par01
                        .getClass()
                        .getDeclaredMethod("toUpperCase", null)
                        .invoke(par01));
    }
    
    (new user$upper_case()).invoke("umba umba umba ê");
    // "UMBA UMBA UMBA Ê"

Eu disse _equivalente_ porque o seu código Clojure vai ser compilado diretamente para _bytecode_ ao invés de ser convertido primeiro para código Java.

Como podemos notar, além de termos que invocar três métodos para fazer a chamada de um e termos que fazer _typecasting_ - ou _coerção de tipos_, se preferir - para que tudo seja tratado como `Object`, o próprio processo de _reflection_ é lento por si só.

Note como informamos o nome do método `toUpperCase` como um texto. Com isso o método `getDeclaredMethod` vai pesquisar uma tabela interna daquela objeto comparando cada nome de método até encontrar o que procuramos.

Para ajudar, como o Clojure não tem a mais remota ideia do que queremos transformar em letras maiúsculas, a função aceita qualquer coisa.

    (upper-case 3.14159)
    ; IllegalArgumentException No matching field found: toUpperCase for 
    ; class java.lang.Double

_Ah, então o compilador do Clojure é mal feito?_

Não. Acontece algo muito parecido quando escrevemos código JavaScript, Ruby, Python ou qualquer outra linguagem dinâmica para a JVM. Como você não informou o tipo, o compilador tem que adivinhar ou confiar cegamente no que você está dizendo.

Porém, existe uma forma de diminuir essa trabalheira toda dando dicas ao compilador sobre o tipo de dado que ele deve utilizar. Essas dicas chamam-se _type hints_, ou dicas de tipos.

Mas atenção: que fique claro que estamos dando dicas ao compilador ao invés de declararmos estaticamente o tipo de dados que estamos utilizando.

Podemos reescrever nosso código dessa forma para que o compilador receba nossas dicas:

    (defn upper-case [^String text]
      (.toUpperCase text))
    
    (upper-case "umba umba umba ê")
    ; "UMBA UMBA UMBA Ê"

Se tentarmos passar qualquer coisa diferente de `String`, a JVM vai reclamar na mesma hora.

    (upper-case 3.14159)
    ; ClassCastException java.lang.Double cannot be cast to 
    ; java.lang.String  user/upper-case (NO_SOURCE_FILE:2)

Aí você pensa _ah, tá. era para ter passado `String` e passei `Double`_. Bonito, não?

Mas não é só isso. Ao usar _type hints_ você ainda leva uma otimização de código totalmente de graça.

O _bytecode_ gerado fica equivalente a esse código Java:

    public Object invoke(Object par01) {
      return ((String)par01).toUpperCase();
    }
    
    (new user$upper_case()).invoke("umba umba umba ê");
    // "UMBA UMBA UMBA Ê"

Se você imaginou que o código, além de menor, também ficou mais rápido, acertou.

_Então vou usar isso no meu código inteiro!_

Calma lá. Clojure não deixa de ser uma linguagem dinâmica apenas por ter _type hints_. A contrapartida dessas dicas é que elas poluem o código e, muitas vezes, e dificilmente você vai precisar que todo seu código seja otimizado.

Na próxima parte vamos aprender quando e como otimizar para praticamente todas as situações.

Até lá.

_(Trecho do meu livro "Clojure: Programação Funcional Descomplicada para a JVM", a ser publicado em breve)_