---
author: rogerbarreto
comments: true
date: 2009-12-17 10:30:15+00:00
layout: post
slug: ruby-rubygems-e-load_path-ou-como-funciona-o-require-de-gems
title: Ruby, Rubygems e $LOAD_PATH ou Como funciona o require de gems
wordpress_id: 589
categories:
- quick tips
- ruby
tags:
- dicas
- gem
- load_path
- ruby
- rubygems
---

Na madrugada passada, andei "brincando" com o fonte do [Rubygems](http://rubyforge.org/projects/rubygems/). Logo de cara posso te dizer que não consegui fazer o que queria, e pra amenizar o sentimento de "perda de tempo", resolvi postar alguns truques aprendidos.


## Baixei o fonte do rubygems, como faço pra rodá-lo sem alterar o meu sistema?


Foi a primeira pergunta que fiz. Percebi que com o google não iria encontrar a resposta, mas consegui uma dica importante: $LOAD_PATH.

    
    $ irb



    
    irb(main):001:0> $LOAD_PATH


No meu Ubuntu, obtive:

    
    ["/usr/local/lib/site_ruby/1.8", "/usr/local/lib/site_ruby/1.8/i486-linux", "/usr/local/lib/site_ruby/1.8/i386-linux", "/usr/local/lib/site_ruby", "/usr/lib/ruby/vendor_ruby/1.8", "/usr/lib/ruby/vendor_ruby/1.8/i486-linux", "/usr/lib/ruby/vendor_ruby", "/usr/lib/ruby/1.8", "/usr/lib/ruby/1.8/i486-linux", "/usr/lib/ruby/1.8/i386-linux", "."]



    
    $ ls /usr/local/lib/site_ruby/1.8/


Exatamente nesta pasta que se encontra o rubygems.rb. Bingo!
Para rodar o fonte do rubygems, só é necessário adicionar ao $LOAD_PATH a pasta lib do projeto. Dado que estou na raiz do projeto rubygems baixado, execute:

    
    ~/rubygems$ ruby -I $PWD/lib ./bin/gem -v


O paramêtro -I permite adicionar diretório ao $LOAD_PATH. Simples e prático. Primeiro problema resolvido, comecei a programar.


## Afinal, como funciona o "require de gems"?


Bom, já sabemos que o require "rubygems" fuciona pois encontra-se no $LOAD_PATH do ruby, no caso do meu Ubuntu em "/usr/local/lib/site_ruby/1.8".

Basicamente (e muito), o Rubygems faz duas coisas no Kernel do Ruby.



	
  * Adiciona o metodo Kernel#gem.

	
  * Faz um [Monkey Patch](http://en.wikipedia.org/wiki/Monkey_patch) no Kernel#require




### Kernel#gem


Permite "acionar" uma versão específica de gem. Note que este acionar, traduz-se para, adicionar a lib da gem no $LOAD_PATH. Segue um trecho do comentário do Kernel#gem:


##




# Use Kernel#gem to activate a specific version of +gem_name+.




#




# +version_requirements+ is a list of version requirements that the




# specified gem must match, most commonly "= example.version.number".  See




# Gem::Requirement for how to specify a version requirement.




#




# If you will be activating the latest version of a gem, there is no need to




# call Kernel#gem, Kernel#require will do the right thing for you.




#




# Kernel#gem returns true if the gem was activated, otherwise false.  If the




# gem could not be found, didn't match the version requirements, or a





# different version was already activated, an exception will be raised.
[...]


### Kernel#require


No final do rubygems.rb encontramos:

if RUBY_VERSION < '1.9' then

require 'rubygems/custom_require'

end

Não consegui descobrir o que acontece com o ruby 1.9, mas no 1.8, o monkey patch executa os seguintes passos:



	
  * Chama o "original" require;

	
  * Em caso de LoadError;

	
    * Executa o "Gem.searcher.find(path)";

	
    * Se _true_

	
      * Chama o activate (novamente traduz-se para adiciona a gem no $LOAD_PATH)

	
      * Executa o "original" require novamente;










### Exemplos com o IRB


Para finalizar legal e comprovar tudo isso, fiz alguns testes:

    
    $ gem list json




*** LOCAL GEMS ***




json (1.2.0, 1.1.9)




json_pure (1.2.0)







    
    $ irb



    
    irb(main):001:0> $LOAD_PATH




=> ["/usr/local/lib/site_ruby/1.8", "/usr/local/lib/site_ruby/1.8/i486-linux", "/usr/local/lib/site_ruby/1.8/i386-linux", "/usr/local/lib/site_ruby", "/usr/lib/ruby/vendor_ruby/1.8", "/usr/lib/ruby/vendor_ruby/1.8/i486-linux", "/usr/lib/ruby/vendor_ruby", "/usr/lib/ruby/1.8", "/usr/lib/ruby/1.8/i486-linux", "/usr/lib/ruby/1.8/i386-linux", "."]




    
    irb(main):004:0> require "json"




LoadError: no such file to load -- json




from (irb):4:in `require'




from (irb):4




from :0




    
    irb(main):005:0> gem "json", "= 1.2.0"




NoMethodError: undefined method `gem' for main:Object





from (irb):5
from :0

O require "json" por si só, carrega a versão mais atual da gem.

    
    irb(main):006:0> require "rubygems"


=> true

    
    irb(main):007:0> require "json"


=> true

    
    irb(main):008:0> JSON::VERSION


=> "1.2.0"

    
    irb(main):009:0> $LOAD_PATH


=> ["/usr/lib/ruby/gems/1.8/gems/gemcutter-0.1.8/lib", "/usr/lib/ruby/gems/1.8/gems/json-1.2.0/bin", "/usr/lib/ruby/gems/1.8/gems/json-1.2.0/ext/json/ext", "/usr/lib/ruby/gems/1.8/gems/json-1.2.0/ext", "/usr/lib/ruby/gems/1.8/gems/json-1.2.0/lib", "/usr/local/lib/site_ruby/1.8", "/usr/local/lib/site_ruby/1.8/i486-linux", "/usr/local/lib/site_ruby/1.8/i386-linux", "/usr/local/lib/site_ruby", "/usr/lib/ruby/vendor_ruby/1.8", "/usr/lib/ruby/vendor_ruby/1.8/i486-linux", "/usr/lib/ruby/vendor_ruby", "/usr/lib/ruby/1.8", "/usr/lib/ruby/1.8/i486-linux", "/usr/lib/ruby/1.8/i386-linux", "."]

    
    irb(main):010:0> quit


Após o require "json", as pastas foram adicionadas no $LOAD_PATH.

    
    "/usr/lib/ruby/gems/1.8/gems/json-1.2.0/bin", "/usr/lib/ruby/gems/1.8/gems/json-1.2.0/ext/json/ext", "/usr/lib/ruby/gems/1.8/gems/json-1.2.0/ext", "/usr/lib/ruby/gems/1.8/gems/json-1.2.0/lib"


Agora olhe que **interessante** este **último teste**:

    
    $ irb
    
    irb(main):001:0> require "rubygems"


=> true

    
    irb(main):002:0> $LOAD_PATH


=> ["/usr/lib/ruby/gems/1.8/gems/gemcutter-0.1.8/lib", "/usr/local/lib/site_ruby/1.8", "/usr/local/lib/site_ruby/1.8/i486-linux", "/usr/local/lib/site_ruby/1.8/i386-linux", "/usr/local/lib/site_ruby", "/usr/lib/ruby/vendor_ruby/1.8", "/usr/lib/ruby/vendor_ruby/1.8/i486-linux", "/usr/lib/ruby/vendor_ruby", "/usr/lib/ruby/1.8", "/usr/lib/ruby/1.8/i486-linux", "/usr/lib/ruby/1.8/i386-linux", "."]

    
    irb(main):003:0> gem "json", "= 1.1.9"


=> true

    
    irb(main):004:0> $LOAD_PATH


=> ["/usr/lib/ruby/gems/1.8/gems/gemcutter-0.1.8/lib", "/usr/lib/ruby/gems/1.8/gems/json-1.1.9/bin", "/usr/lib/ruby/gems/1.8/gems/json-1.1.9/ext/json/ext", "/usr/lib/ruby/gems/1.8/gems/json-1.1.9/ext", "/usr/lib/ruby/gems/1.8/gems/json-1.1.9/lib", "/usr/local/lib/site_ruby/1.8", "/usr/local/lib/site_ruby/1.8/i486-linux", "/usr/local/lib/site_ruby/1.8/i386-linux", "/usr/local/lib/site_ruby", "/usr/lib/ruby/vendor_ruby/1.8", "/usr/lib/ruby/vendor_ruby/1.8/i486-linux", "/usr/lib/ruby/vendor_ruby", "/usr/lib/ruby/1.8", "/usr/lib/ruby/1.8/i486-linux", "/usr/lib/ruby/1.8/i386-linux", "."]

    
    irb(main):005:0> JSON


NameError: uninitialized constant JSON
from (irb):5

    
    irb(main):006:0> require "json"


=> true

    
    irb(main):007:0> JSON::VERSION


=> "1.1.9"

    
    irb(main):008:0> quit


Note que após o gem "json", "= 1.1.9" ... a versao 1.1.9 foi adicionada no $LOAD_PATH mas não foi carregada. Ao executar o require "json", como este já estava no $LOAD_PATH, a versão 1.1.9 é usada.

Espero que com estas explicações, você use com mais segurança o rubygems.
