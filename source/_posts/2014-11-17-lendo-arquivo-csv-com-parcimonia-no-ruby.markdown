---
layout: post
title: "Lendo arquivo CSV com parcimônia no Ruby"
date: 2014-11-17 09:34
author: 'rogerbarreto'
comments: true
categories:
- ruby
- csv
tags:
- ruby
- csv
- programacao
---
Ler e escrever [arquivo csv](http://en.wikipedia.org/wiki/Comma-separated_values) é um mal necessário de muitos sistemas, ainda mais levando em conta que esta integração será feita via Excel, em algum Windows, com quilos de texto com acentos e dados a formatar. Dado este cenário, e que ele provavelmente se repetirá no futuro, deixo aqui um post auto-ajuda para mim mesmo e provavelmente para você que está lendo. :D

Na versão 1.9.3 e superior, o Ruby incluiu a classe [CSV](http://ruby-doc.org/stdlib-1.9.3/libdoc/csv/rdoc/CSV.html) na sua *standard lib*, que facilita o trabalho de ler e/ou escrever arquivos csv. Exemplos em código abaixo.

## Conhecendo o CSV

O modo mais simples e direto para ler um arquivo csv, é usar o `CSV.read` que retorna um Array de Arrays:

{% codeblock lang:ruby %}
require 'csv'
array_students = CSV.read('/tmp/mock_data.csv') # return an Array of Arrays
array_students.each { |row| puts row.inspect }  # => output:
# "[\"id\", \"name\", \"country\", \"birthday\"]"
# "[\"1\", \"Virginia Harvey\", \"GB\", \"01/06/1993\"]"
{% endcodeblock %}

Dentro da classe CSV, existem mais duas classes que facilitam ainda mais o manuseio dos dados.

Caso necessite de mais requinte e sofisticação, o método `CSV.table` retorna uma instância de [CSV::Table](http://ruby-doc.org/stdlib-1.9.3/libdoc/csv/rdoc/CSV/Table.html). Com o table, você tem acesso ao cabeçalho através do `headers` e acesso a cada linha do arquivo com o `each`, que retorna uma instância de [CSV::Row](http://ruby-doc.org/stdlib-1.9.3/libdoc/csv/rdoc/CSV/Row.html).

{% codeblock lang:ruby %}
require 'csv'
table_students = CSV.table('/tmp/mock_data.csv') # => instance of CSV::Table
puts table_students.headers.inspect # => [:id, :name, :country, :birthday]
table_students.each { |row| puts row.inspect } # => output:
# <CSV::Row id:1 name:"Virginia Harvey" country:"GB" birthday:"01/06/1993">
table_students.each { |row| puts row.fetch(:name) } # => output:
# Virginia Harvey
{% endcodeblock %}

Tanto o `read` quanto o `table`, aceitam um hash de options como segundo argumento. Tem uma descrição detalhada na documentação do [método new](http://ruby-doc.org/stdlib-1.9.3/libdoc/csv/rdoc/CSV.html#method-c-new). Exemplo usando options:

{% codeblock lang:ruby %}
require 'csv'
table_students = CSV.table('/tmp/mock_data2.csv', col_sep: ";", skip_blanks: true, converters: [])
table_students.each { |row| puts row.inspect }
{% endcodeblock %}

## CSV converters

[CSV::HeaderConverters](http://www.ruby-doc.org/stdlib-1.9.3/libdoc/csv/rdoc/CSV.html#HeaderConverters) contém um hash de symbol e block que são usados para converter os valores do cabeçalho. Para usá-los, você deve informar qual *converter* deseja aplicar na opção `header_converters`. Acredito que o código abaixo explica melhor.

{% codeblock lang:ruby %}
require 'csv'
puts CSV::HeaderConverters.keys.inspect # => [:downcase, :symbol]

# Add new header converter
CSV::HeaderConverters[:remap] = lambda do |raw_value|
  raw_value = raw_value.to_sym
  case raw_value
  when :country
    :pais
  when :birthday
    :dt_nascimento
  else
    raw_value
  end
end

table_students = CSV.table('mock_data.csv', col_sep: ",", header_converters: :remap)
table_students.each do |row|
  puts [row.fetch(:pais), row.fetch(:dt_nascimento)].inspect # => ["GB", "01/06/1993"]
end
{% endcodeblock %}

No exemplo acima, criei o HeaderConverter "remap" que traduz o cabeçalho country para pais e birthday para dt_nascimento. Por padrão, o `CSV` disponibiliza os converters downcase e symbol, que por sinal são usados quando usamos o método `table` para ler csv.

[CSV::Converters](http://www.ruby-doc.org/stdlib-1.9.3/libdoc/csv/rdoc/CSV.html#Converters) segue o mesmo padrão de symbol e block, a única diferença que este é usado para converter os valores da linha. Vamos ao código.

{% codeblock lang:ruby %}
require 'csv'
require 'date'

puts CSV::Converters.keys.inspect       # => [:integer, :float, :numeric, :date, :date_time, :all]

# Add new converter
CSV::Converters[:nil_to_empty] = lambda do |raw_value|
  raw_value.nil? ? "" : raw_value
end

# Add new converter
CSV::Converters[:brazilian_date] = lambda do |raw_value|
  if raw_value =~ /\d{2}\/\d{2}\/\d{4}/
    Date.strptime(raw_value, "%d/%m/%Y")
  else
    raw_value
  end
end

# Group converters
CSV::Converters[:my_custom_converters] = [:nil_to_empty, :brazilian_date]

table_students = CSV.table('mock_data.csv', col_sep: ",", converters: :my_custom_converters)
table_students.each do |row|
  puts [row.fetch(:country), row.fetch(:birthday)].inspect # => ["GB", #<Date: 1993-06-01 ((2449140j,0s,0n),+0s,2299161j)>]
end
{% endcodeblock %}

No exemplo acima criei dois converters. Um para trocar nil por "" e o outro que converte para Date caso o valor esteja no formato 99/99/9999.

## Encoding hell com Excel

Normalmente o csv é usado como meio de integração Excel <=> Sistema. Acontece que o Excel não se dá muito bem com acentos especiais como ãõáé etc. Isto porque estamos em 2014. Acontece que quando há caracteres especiais, a única abordagem que funcionou foi exportar para Unicode text. Neste formato, o encoding do arquivo é UTF-16LE e separado por tab (\t). Este [post de 2009 da Plataformatec](http://blog.plataformatec.com.br/2009/09/exportando-dados-para-excel-usando-csv-em-um-aplicativo-rails/) explica com mais detalhes este jeitinho do Excel de ser com os dados. A única diferença de 2009 pra hoje, é que podemos passar o encoding como parâmetro ao ler o arquivo, e por sorte evitar o uso do iconv. Vamos ao código:


{% codeblock lang:ruby %}
require 'csv'

table = CSV.table('mock_unicode.txt',
                  col_sep: "\t", # tab as delimiter
                  encoding: "UTF-16LE:UTF-8") # read UTF-16LE and convert to UTF-8
table.each do |row|
  puts row.inspect
end
{% endcodeblock %}

## Evitando o abuso de memória

Ao ler arquivos com `read` ou `table`, o arquivo é colocado em memória, ou seja, ao processar uma planilha de 100mb, o seu processo ruby vai pra um 100mb e pouco. Agora imagina 20 workers e cada um processando uma planilha de 100mb ou mais, facilmente o seu servidor terá um pico de consumo de memória, o no pior cenário vai dar crash no processo. Para evitar este consumo devemos usar o `foreach` do `CSV`.

{% codeblock lang:ruby %}
require 'csv'

CSV.foreach("mock_data.csv", col_sep: ",") do |row|
  puts row.inspect
end

# =>
# ["id", "name", "country", "birthday"]
# ["1", "Virginia Harvey", "GB", "01/06/1993"]
{% endcodeblock %}

Desta maneira a leitura é mais otimizada, pois apenas uma linha por vez é lida. O único problema é que perdemos algumas facilidades do `table`, como os `headers` e a instância do `CSV::Row` por linha. Tentando chegar no modelo ideal, montei uma classe que usa o foreach e mesmo assim tem os `headers` e os `rows`.

{% codeblock lang:ruby %}
require 'csv'

class SheetReader

  attr_reader :headers

  def initialize(filepath)
    # options to read unicode text file
    # options = {
    #   col_sep: "\t",
    #   skip_blanks: true,
    #   encoding: "UTF-16LE:UTF-8",
    #   converters: []
    # }
    options = {col_sep: ",", converters: []}
    @csv_reader = CSV.foreach(filepath, options) # gets a iterator
    @headers    = convert_headers(@csv_reader.next) # read first line
  end

  # yield an instance of http://ruby-doc.org/stdlib-2.1.0/libdoc/csv/rdoc/CSV/Row.html
  def each_row(&block)
    begin
      while true
        raw_row = @csv_reader.next  # raise StopIteration in EOF
        yield CSV::Row.new(headers, raw_row)
      end
    rescue StopIteration
    end
  end

  protected

  # Internal: Convert headers to Array of symbols.
  #
  # raw_headers - Array of Strings.
  #
  # Examples
  #
  #   convert_headers(["ATIVO", "NOME COMERCIAL"])
  #   # => [:ativo, :nome_comercial]
  #
  # Returns Array of symbols.
  def convert_headers(raw_headers)
    raw_headers.compact! # removes nil values
    converter = lambda do |header|
      header_converters = CSV::HeaderConverters.values
      header_converters.inject(header) do |header, converter_proc|
        converter_proc.call(header)
      end
    end

    raw_headers.map { |header| converter.call(header) }
  end
end

reader = SheetReader.new('mock_data.csv')
reader.headers # => [:id, :name, :country, :birthday]
reader.each_row do |row|
  puts row.inspect # => #<CSV::Row id:"1" name:"Virginia Harvey" country:"GB" birthday:"01/06/1993">
end
{% endcodeblock %}

Por último, uma observação importante: todo este código acima foi rodado no ruby 2.1.0. Espero que este mini guia de como ler arquivo csv com Ruby te ajude.
Segue alguns links com mais informações:

* http://www.sitepoint.com/guide-ruby-csv-library-part/
* http://www.sitepoint.com/guide-ruby-csv-library-part-2/
* http://technicalpickles.com/posts/parsing-csv-with-ruby/
* http://blog.plataformatec.com.br/2009/09/exportando-dados-para-excel-usando-csv-em-um-aplicativo-rails/

Dúvidas, sugestões ou qualquer outra coisa. Deixe um comentário ou se preferir, mande um [tweety!](https://twitter.com/rogerleite) :D
