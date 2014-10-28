---
author: pbalduino
comments: true
date: 2013-07-14 14:35:31+00:00
layout: post
slug: entendendo-lisp-finalmente
title: Entendendo LISP, finalmente.
wordpress_id: 1400
categories:
- lisp
- quick tips
tags:
- clojure
- lisp
---

## A sintaxe invertida



Ao olhar um código LISP pela primeira vez, você se assusta.

Eu me assustei e não havia ninguém para me ajudar a entender.

Que bom que você está lendo isto para entender bem depressa e perder o medo.

Acredite ou não, o LISP não é invertido: as outras linguagens é que são inconsistentes.

Matematicamente falando, funções são expressas dessa forma:


    
    y = f(x)



Para calcularmos o dobro de um número, teríamos:


    
    y = dobro(21)



Note que estamos usando uma notação diferente: primeiro vem o operador dobro e, em seguida, vem o operando, ou parâmetro, 21. Chamamos isso de notação prefixa.

Já para executar um cálculo matemático, usamos a forma abaixo:


    
    y = 21 * 2



Primeiro temos um operador 21, depois temos um operando responsável pela multiplicação e, finalmente, o segundo operando 2. Chamamos essa forma de notação infixa.

Nota: se você é um desenvolvedor Ruby, ignore essa última expressão. Em Ruby o cálculo acima utiliza internamente a notação prefixa onde 21 é um objeto, * é um método (ou uma mensagem, se preferir) e 2 é um parâmetro.

A coisa fica bagunçada quando misturamos as duas formas:


    
    y = dobro(7 * 3)



Na expressão acima misturamos notação prefixa com infixa. Não há problema algum com isso, mas não é um bom exemplo de consistência.

Quando falamos em LISP, o primeiro item de uma lista é um operador e todos os demais são operandos.

Todo operador é uma função, macro ou forma especial, inclusive os operadores matemáticos. Não se preocupe em entender agora o que são macros ou formas especiais. Todo o resto da lista é considerado um valor, parâmetro ou operando.

Imagine agora que o símbolo `+` é uma função. Para calcularmos uma soma usaríamos o seguinte código:


    
    +(1, 2)



Movendo os parênteses e removendo as vírgulas, a nossa soma inicial ficaria:


    
    (+ 1 2)



Sabemos que dobro também é uma função. Para calcular dobro, usaríamos:


    
    (dobro 12)



Percebam que agora temos uma regra que se aplica a todos os casos. Repetindo a expressão acima que mistura as notações infixa e prefixa usando as regras do LISP, teríamos:


    
    (dobro (+ 7 3))



Talvez pela sua origem acadêmica e fortemente influenciada pela matemática, as implementações de LISP levam muito a sério a questão da consistência.



## Os parênteses



Quando eu estava na quarta série, aprendi uma coisa chamada _expressão numérica_, que consistia em resolver um cálculo extenso atacando um pedaço por vez, organizadamente.

Cada pedaço desse cálculo ficava dentro de parênteses, colchetes ou chaves, dependendo do quão aninhado estivesse a expressão. Eu nunca mais vi esse tipo de hierarquia,  mas era um jeito bacana de manter a organização.

Uma expressão numérica tem essa cara:


    
    x = {1 + [3 * (5 + 7)]}



Resolvemos a expressão de dentro para fora:


    
    x = {1 + [3 * (12)]}
    
    x = {1 + [36]}
    
    x = {37}
    
    x = 37


Simples, não?

Agora vamos extrapolar o que aprendemos na quarta série para uma linguagem de programação, trocando chaves e colchetes por parênteses:


    
    x = (1 + (3 * (5 + 7)))



Vamos substituir a nossa conhecida notação infixa pela prefixa.


    
    x = (+ 1 (* 3 (+ 5 7)))



Pronto. Você tem uma expressão numérica com a cara do LISP, resolvendo da forma como a professora ensinou lá na quarta série: primeiro você resolve os parênteses de dentro, depois os próximos, até terminar.

Qualquer LISP que você encontrar pela frente, incluindo o Clojure, funciona exatamente dessa maneira.

Uma vantagem que isso traz é que você não precisa ficar se preocupando com precedência de operadores.

Imagine que você tem o código abaixo:


    
    x = 3 * 2 + 1
    
    y = 1 + 2 * 3



Os valores de x e y serão iguais? Sim, ambas as variáveis contém o número 7, mas para saber disso você precisou ler em algum outro lugar que o operador de multiplicação tem precedência sobre o operador de adição. É algo que você espera que seja assim e age como se realmente fosse.

E o que aconteceria se você estiver usando uma linguagem em que a adição tem precedência sobre a multiplicação? Ou pior ainda: os operadores são executados da esquerda para a direita conforme forem aparecendo.

No primeiro caso, x e y continuariam sendo igual, mas ambos teriam o valor 9. No segundo caso, x seria igual a 7 e y seria igual a 9.

Seria mais fácil se as expressões fossem escritas assim:


    
    x = (3 * 2) + 1
    
    y = 1 + (2 * 3)



Agora está claro para qualquer pessoa o que vai ser executado primeiro, independente do modo como a expressão seja interpretada pela linguagem. Pois saiba que é exatamente assim que um LISP trabalha. Usando a notação prefixa, as expressões acima ficariam:


    
    x = (+ (* 3 2) 1)
    
    y = (+ 1 (* 3 2))



Primeiro será executada a multiplicação, que está nos parênteses mais internos e, em seguida, será executada a adição. Tudo isso sem se preocupar com regras ocultas ou peculiaridades do compilador.

Qualquer código em qualquer dialeto LISP, mesmo com suas características particulares, fica fácil de entender se você lembrar dessas regrinha.
