---
author: rogerbarreto
comments: true
date: 2009-12-22 16:24:09+00:00
layout: post
slug: importando-e-exportando-suas-gems-com-rubygems-snapshot
title: Importando e exportando suas gems com Rubygems Snapshot
wordpress_id: 618
categories:
- projetos
- quick tips
- ruby
tags:
- gem
- plugin
- quick tips
- ruby
- rubygems
---

Final de ano está rendendo.
O [rubygems_snapshot](http://github.com/rogerleite/rubygems_snapshot) nasceu da necessidade de "migrar" as gems instaladas de uma máquina para outra, aliado ao [rvm](http://rvm.beginrescueend.com/) (veja este [post-guia-rápido](http://www.nuxlli.com.br/2009/11/24/para-tudo-instale-o-rvm-antes/)), permite mudar e/ou criar diferentes ambientes em minutos. Assim você pode fugir do famoso "gem hell".

Veja como é difícil usar:

Instalação:

    
    sudo gem install rubygems_snapshot


Para **exportar** as gems instaladas:

    
    gem snapshot export projeto-exemplo.yml


Supondo que esteja em outra máquina, para **importar** as gems, use:

    
    [sudo] gem snapshot import projeto-exemplo.yml




## Afinal, o que tem de legal nisso?


Vamos supor que você acaba de entrar numa nova equipe e tem que montar o ambiente de desenvolvimento (por sinal, um ambiente complicado de configurar). O gem snapshot aliada ao rvm, foi feito para facilitar isto, vamos a um exemplo rápido:

Com o rvm, você pode criar um "novo ambiente":

    
    rvm use 1.8.7%projeto_exemplo



    
    gem list


Deve retornar vazio.

    
    gem install rubygems_snapshot



    
    gem snapshot import projeto-exemplo.yml


Instalará as gems necessárias para o projeto e pronto!


## ToDo:


Esta é uma versão bem básica, onde o "import" somente lê as gem e version e manda instalar sem requerir dependências. Está previsto de colocar um aviso no final das gems que deram erro, geralmente devido a dependências de "build nativos", mas por enquanto estamos usando aqui na equipe com sucesso.


## Como faço para criar um rubygems plugin também?


Bom, logo de cara posso te garantir que não é difícil (apesar da pouca documentação na internet), mas deixarei os detalhes para um outro post. Por enquanto, a minha recomendação é: clone o projeto, analise os dois rb do projeto :D e crie o seu!

Caso algum corajoso for usar, estou a disposição para ajudar, é só deixar um comentário aê!
Valeu e sucesso!
