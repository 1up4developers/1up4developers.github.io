---
layout: post
title: "Extraindo dados da Internet com Clojure"
date: 2014-10-28 12:10
author: 'pbalduino'
comments: true
categories:
- clojure
- lisp
- cookbook
tags:
- cookbook
- clojure
- lisp
- enlive
---
# Extraindo dados da Internet com Clojure

## O problema

A Internet é um repositórios de dados gigantesco e frequentemente precisamos extrair algo que nos interessa de maneira automatizada. O grande problema é que esses dados normalmente são apresentados de forma não estruturada, e precisamos utilizar uma técnica chamada _scrapping_, que consiste em abrir uma página, carregar o HTML e navegar dentro desse código para extrair o que precisamos.

Com o uso das ferramentas certas isso não é complicado, mas pode ser trabalhoso e, uma vez que a página que você estiver lendo altere alguma coisa em sua estrutura, você terá que adaptar seu código às mudanças.

Vamos apresentar um exemplo simples, mas que vai te dar uma boa base de como extrair dados de uma página utilizando Clojure.

## Quem escreveu mais livros na Casa do Código?

A Casa do Código é uma editora brasileira especializada em livros para desenvolvedores de software, empreendedores e webdesigners. Seus autores são profissionais conhecidos em suas respectivas áreas e precisamos saber quais deles escreveram mais livros.

Para isso, vamos abrir a página inicial da editora em http://www.casadocodigo.com.br/, que contém links para todos os livros publicados até o momento. Com esses links em mãos, vamos entrar em cada um deles e extrair os nomes dos autores para em seguida agrupá-los e apresentarmos o resultado.

### Como fazer

Vamos criar um projeto chamado `autores`, definir o namespace inicial e adicionar a biblioteca _Enlive_, que vai nos permitir extrair os dados que queremos de dentro do código HTML. O nosso arquivo project.clj vai ficar parecido com o exemplo abaixo:

    (defproject autores "0.1.0-SNAPSHOT"
      ; informações de licença e descrição do projeto

      :dependencies [[org.clojure/clojure "1.6.0"]
                     [enlive "1.1.5"]]
      :main autores.core)

Enlive é uma biblioteca criada por Christophe Grand, coautor de Clojure Programming, que permite que você gere código HTML escrevendo em Clojure, e permite também que você extraia textos de um arquivo HTML já existente.

Para utilizarmos o Enlive em nosso código, vamos referenciar as funções na nossa declaração de namespace. Vamos também adicionar o _Pretty Print_ para exibir o resultado formatado e as bibliotecas do _clojure.string_ para manipularmos o texto. Também vamos precisar importar a classe `java.net.URL` para tratarmos o endereço do site. Nosso código então começa assim:

    (ns autores.core
      (:require [clojure.pprint :as pp]
                [net.cgrand.enlive-html :as en]
                [clojure.string :as str])
      (:import  [java.net URL]))

O primeiro passo é varrer a página inicial da editora e extrair os links dos livros. Abrindo o código fonte da página, percebemos que ela tem a seguinte estrutura, que aqui está devidamente resumida para fins de apresentação:

    <html>
    <head>
      <!-- titulo, meta, etc -->
    </head>
    <body>
      <nav>
        <!-- menu do topo -->
      </nav>
      <section>
        <!-- links e imagens com os livros -->
      </section>
      <footer>
        <!-- menu do rodapé -->
      </footer>
    </body>
    </html>

Visualmente, a página tem a aparência da imagem abaixo:

![/images/uploads/10/cdc-full.png](/images/cdc-parts.png)

Felizmente a página está bem estruturada e todos os links que nos interessam estão dentro da área `section`, o que vai nos poupar trabalho.

Vamos criar uma função chamada `get-links`, que vai receber a URL do site em formato texto e vamos extrair todo o código HTML usando a função `html-resource` do Enlive.

    (defn- get-links [url]
      (en/html-resource (URL. url)))

Perceba que a função `html-resource` exige que você converta a URL de texto para um objeto `java.net.URL`, para só então o passarmos como parâmetro para o Enlive. 

Para evitar que o encadeamento de muitas funções torne o código difícil de ler, vamos alterá-lo para utilizar o operador `->` e vamos guardar o resultado em um _binding_ chamado `links`. Perceba que essa abordagem permite que possamos adicionar funções no final da lista de argumentos de `->` sem diminuir a legibilidade do código.

    (defn- get-links [url]
      (let [links (-> url
                      URL.
                      en/html-resource)]
        (pp/pprint links)))

Vamos então usar a função `select`, também do Enlive, que recebe como argumento um _vector_ com as tags que você deseja extrair. Nós queremos somente os links, formados pela tag `<a></a>`, que estão contidas entre `<section>` e `</section>`.

    (defn- get-links [url]
      (let [links (-> url
                      URL. 
                      en/html-resource
                      (en/select [:body :section :a]))]))

A função `select` vai nos retornar uma lista contendo um _map_ para cada elemento HTML que obedecer aos nossos requisitos. Cada _map_ tem um item `:attrs` que contém os atributos da tag HTML, incluindo a página para a qual o link está apontando. Ainda dentro da função `get-links`, vamos converter essa lista de mapas em uma lista que contenha apenas os endereços e para isso vamos usar a função `map`, que recebe como parâmetros a função que vai transformar cada item da lista e a lista a ser transformada. Para fins de didática, vamos suprimir o código que está dentro de `let`, para só no final da explicação mostrarmos a função completa.

      (map #((% :attrs) :href) 
           links)

Agora nossa função está retornando uma lista de links como na listagem abaixo, devidamente resumida:

    ("/products/livro-programador-apaixonado"
     "/products/livro-aspnet-mvc"
     "/products/livro-jpa-eficaz"
     "/products/livro-photoshop"
     "/products/colecao-frameworks-java"
     ...
     "/products/livro-ciencia-da-computacao-com-jogos"
     "/products/vale-presente")

Os links são relativos à URL da página inicial, então vamos adicionar o endereço que que foi passado para a função `get-links` para torná-los absolutos. Note que nessa lista existem links que não são de livros, mas de coleções e do vale presente. Podemos eliminá-los usando a função `filter`, que recebe como parâmetros uma função que retorna `true` ou `false` de acordo com cada item da lista, e a lista a ser filtrada que é passada como terceiro parâmetro. Os itens que fizerem a função retornar `true` ficam, e os demais não são incluidos.

    (filter
      #(. % contains "livro")
      (map #(str url ((% :attrs) :href)) 
           links))

E agora nossa função retorna os endereços absolutos de todos os livros da editora, conforme a listagem abaixo:

    ("http://www.casadocodigo.com.br/products/livro-programador-apaixonado"
     "http://www.casadocodigo.com.br/products/livro-aspnet-mvc"
     "http://www.casadocodigo.com.br/products/livro-jpa-eficaz"
     "http://www.casadocodigo.com.br/products/livro-photoshop"
     ...
     "http://www.casadocodigo.com.br/products/livro-ciencia-da-computacao-com-jogos")

Agora que concluímos o primeiro passo da nossa pesquisa, vamos editar o código da nossa função `main` para que a função `get-links` seja chamada:

    (defn -main [& args]
      (pp/pprint
        (get-links "http://www.casadocodigo.com.br")))
             
Vamos criar agora uma função chamada `get-author`, que vai receber cada um dos links e, usando as funções do Enlive que já conhecemos, vai extrair o nome do autor, ou dos autores, de cada um dos livros.

Os nomes dos autores ficam dentro de uma tag `span` marcada com a classe `product-author-link`. Mais uma vez o site bem construído nos ajuda na tarefa. Para pesquisarmos uma tag que esteja utilizando determinada classe, vamos separar tag e classe com um ponto final, como se fosse um arquivo CSS. Assim, o nosso `span` com a classe `product-author-link` vai virar `:span.product-author-link`. O código inicial não vai ficar muito diferente do que fizemos em `get-links`:

    (defn- get-author [url]
      (let [authors (-> url
                        URL.
                        en/html-resource
                        (en/select [:span.product-author-link]))]
          (pp/pprint authors)))

Vamos usar o link do primeiro livro para testar. O resultado traz toda a informação da tag `span` dentro de um map que está dentro de uma lista, e não só o nome do autor.

    (get-author "http://www.casadocodigo.com.br/products/livro-programador-apaixonado")

    ({:tag :span,
      :attrs {:class "product-author-link"},
      :content
      ("\n          \n            Chad Fowler\n          \n        ")})

Novamente vamos suprimir o código que está dentro do `let` para não poluir o texto. Vamos selecionar o _map_ que está dentro da lista usando a função `first` e, em seguida, utilizar somente o valor que está na chave `:content`:

    ((first authors) :content)

O valor que estava em `:content` também é uma lista contendo o nome do autor. Além do nome do autor, o texto está poluído com quebras de linhas, simbolizado por `\n` e espaços em branco. Vamos utilizar a função `replace` do namespace `clojure.string` para remover esses caracteres indesejados. Essa função recebe como parâmetros o texto original, uma expressão regular indicando o que deve ser alterado e, por último, o texto a ser utilizado na alteração.

Vamos utilizar o operador `->` para facilitar a leitura:

    (-> ((first authors) :content)
        first
        (str/replace #"(\n|  )" ""))

Ao executar a função, temos o nome do autor limpo e sem caracteres indesejados:

    (get-author "http://www.casadocodigo.com.br/products/livro-programador-apaixonado")
    => "Chad Fowler"

Tudo certo quando estamos lidando com um livro escrito por apenas um autor, mas quando temos coautorias a nossa função já não faz o que é esperado. Por exemplo, no livro de ASP.NET MVC, nossa função retorna o autor como `Fabrício Sanchez e Márcio Fábio Althmann`, e no livro de Lógica de Programação temos `Paulo Silveira, Adriano Almeida`. Tanto `,` como `e` são usados para separar os nomes dos autores. 
Vamos fazer a nossa função retornar um _vector_ com os nomes dos autores separados utilizando a função `split`, do namespace `clojure.string`. Essa função recebe como argumentos o texto a ser dividido e uma expressão regular indicando onde o texto deve ser dividido: 

    (-> ((first authors) :content)
        first
        (str/replace #"(\n|  )" "")
        (str/split #"(, | e )"))

Agora temos os nomes dos autores separados corretamente dentro um _vector_, como no exemplo abaixo:

    (get-author "http://www.casadocodigo.com.br/products/livro-programacao")
    => ["Paulo Silveira" "Adriano Almeida"]

Voltando à nossa função `-main`, vamos fazer com que a função `get-author` pega os autores de cada um dos links retornados pela função `get-links`. Vamos usar o operador `->>` para deixar mais fácil de entender as transformações que estamos fazendo nos dados. A diferença para o `->` é que o operador `->>` passa o resultado da expressão como último parâmetro da expressão seguinte, enquanto o operador `->` passa como primeiro. Isso é necessário para podermos passar a lista retornada por `get-links` para a função `map` da expressão seguinte:

    (defn -main [& args]
      (pp/pprint
        (->> "http://www.casadocodigo.com.br"
             get-links
             (map get-author))))

A nossa função `-main` retornou uma lista contendo vários _vector_ com os nomes dos autores. Um livro com mais de um autor vai retornar um _vector_ com mais de um nome.

    (["Chad Fowler"]
     ["Fabrício Sanchez" "Márcio Fábio Althmann"]
     ...
     ["Mauricio Tollin" "Rodrigo Gomes" "Anderson Leite"]
     ...
     ["André Backes"]
     ["Bruno Feijó" "Esteban Clua" "Flávio S. Correa da Silva"])

Vamos utilizar a função `flatten` para converter essa lista de _vectors_ de diversos tamanhos em uma lista de uma dimensão. Isso vai nos permitir calcular quantas vezes cada nome aparece na lista por meio da função `frequencies`. Como cada vez que o nome aparece é um livro escrito por aquele autor, podemos entender que `frequencies` vai atribuir o número de livros que aquele autor tem publicado pela Casa do Código:

    (defn -main [& args]
      (pp/pprint
        (->> "http://www.casadocodigo.com.br"
             get-links
             (map get-author)
             flatten
             frequencies)))

Agora temos uma lista não ordenada com o nome do autor e a quantidade de livros publicados:

    {"Caio Ribeiro Pereira" 1,
     "Fabrício Sanchez" 1,
     "Alexandre Saudate" 2,
     "Chad Fowler" 1,
     ...
     "Gabriel Schade Cardoso" 1,
     "Guilherme Moreira" 1}

Nosso próximo passo é ordenar essa lista, deixando os autores com maior número de livros no topo. Para isso vamos usar a função `sort`, que permite que você informe uma função para selecionar o critério de ordenação, que no nosso caso vai ser a função `last`, que retorna o último item de uma lista, e também a função que vai ser usada para comparar um item com outro e definir quem vem primeiro, que nosso caso vai ser `>`. No caso do `last`, convém deixar claro que vamos pegar o último item do que vai ser comparado. No caso de `"Caio Ribeiro Pereira" 1`, o último item é o número 1. Se quisessemos ordenar por nome, usariámos a função `first`.

Em seguida vamos dividir essa lista em _n_ grupos, de acordo com a quantidade de livros publicados usando a função `partition-by`. No nosso exemplo teremos um grupo com autores que tenham dois livros publicados e outro com autores com apenas um livro. Como nos interessa apenas os autores que mais publicaram livros, vamos usar a função `first` para selecionarmos apenas o primeiro grupo. Com isso nosso código ficará assim:

    (defn -main [& args]
      (pp/pprint
        (->> "http://www.casadocodigo.com.br"
             get-links
             (map get-author)
             flatten
             frequencies
             (sort-by last >)
             (partition-by last)
             first)))

Agora temos o resultado abaixo:

    (["Alexandre Saudate" 2]
     ["Sérgio Lopes" 2]
     ["Paulo Silveira" 2]
     ["Tárcio Zemel" 2]
     ["Anderson Leite" 2]
     ["Mauricio Aniche" 2]
     ["Gilliard Cordeiro" 2]
     ["Hébert Coelho" 2]
     ["Eduardo Guerra" 2]
     ["Rafael Steil" 2])

Agora que temos a lista dos maiores autores, vamos otimizar um o nosso código adicionando uma dose de processamento paralelo. Note que a função `get-links` retorna uma lista de endereços que é passada como parâmetro para a função `map`, que passa endereço por endereço para a função `get-author`, sequencialmente. Se trocarmos `map` por `pmap`, serão criadas threads que executarão `get-author` em paralelo, melhorando o tempo total do nosso código. A quantidade de thread criadas por `pmap` está diretamente ligada ao número de núcleos ou processadores que sua máquina tiver.

Porém, o uso de `pmap` pode trazer um problema: é comum que a aplicação fique congelada por até um minuto após a execução do `pmap` por conta de questões internas de timeout de threads e configurações do pool utilizado pelo Clojure. Para resolver isso, devemos solicitar ao Clojure que finalize todas as thread que não estiverem sendo usadas, liberando-as para que o programa possa ser encerrado. Para isso usamos a função `shutdown-agents` na última linha da função `-main`.

Agora temos o nosso código funcionando alguns segundos mais rápido e trazendo os autores que mais publicaram livros pela Casa do Código.

### Código completo

    (ns autores.core
      (:require [clojure.pprint :as pp]
                [net.cgrand.enlive-html :as en]
                [clojure.string :as str])
      (:import  [java.net URL]))

    (defn- get-links [url]
      (let [links (-> url
                      URL. 
                      en/html-resource
                      (en/select [:body :section :a]))]
          (filter
            #(. % contains "livro")
            (map #(str url ((% :attrs) :href)) 
                 links))))

    (defn- get-author [url]
      (let [authors (-> url
                        URL.
                        en/html-resource
                        (en/select [:span.product-author-link]))]
        (str/split
          (str/replace
            (first
              ((first authors) :content))
            #"(\n|  )" "")
          #"(, | e )")))

    (defn -main [& args]
      (println "Os autores com mais publicações na Casa do Código:")
      (pp/pprint
        (->> "http://www.casadocodigo.com.br"
             get-links
             (pmap get-author)
             flatten
             frequencies
             (sort-by last >)
             (partition-by last)
             first))
      (shutdown-agents))
