---
author: rogerbarreto
comments: true
date: 2012-06-01 13:14:30+00:00
layout: post
slug: vagrant-virtualizando-o-ambiente-de-trabalho
title: Vagrant, virtualizando o ambiente de trabalho
wordpress_id: 1141
categories:
- projetos
- quick tips
- real world
- web
tags:
- dicas
- gem
- pragmatismo
- real world
- vagrant
- virtualização
---

[Vagrant](http://vagrantup.com/), se você não conhece ou não deu atenção para ele nestes últimos tempos, este post é pra ti mesmo. Trata-se de uma ferramenta que facilita (e muito!) a criação de Máquinas Virtuais usando o Virtual Box por baixo dos panos. E não é só isso! Com o Vagrant fazer _port forward_, compartilhar pastas é só questão de alterar um arquivo de configuração. Continue lendo que eu detalho melhor tudo isto.


## Prós e Contra<del>s</del>


Vários pontos se destacam no uso do Vagrant:




  * Centraliza as _dependências de ambiente_ do projeto. Sabe aquele projeto legado que só roda com rubygems 1.4.2 e mongo 1.1, com o Sol alinhado aos anéis de Saturno, então, você pode deixar tudo isso num _box_ do Vagrant.


  * Documenta as _dependências de ambiente_, caso use algum _Provisioner_.


  * Facilita a integração de novos desenvolvedores na equipe, independente do SO que utiliza.


  * Mantém a sua máquina local "limpa". Você não precisa instalar o Mysql, Postgree, Redis, Memcache etc. para cada projeto que roda.


Agora vem o contra.


  * Se você trabalha com projetos simples ou até mesmo com poucos projetos, você pode sentir que está usando um canhão para matar mosca.




## Instalação


Você precisa do [VirtualBox](https://www.virtualbox.org/) (versões 4.0.x ou 4.1.x). Já o Vagrant, o jeito mais fácil é instalar via rubygems, ou seja, um "gem install vagrant" e pronto! Caso ache melhor instalar via .dmg, .deb etc., você pode baixar em [http://downloads.vagrantup.com](http://downloads.vagrantup.com).


## Exemplo de Uso


O [Getting Started](http://vagrantup.com/v1/docs/getting-started/index.html) do Vagrant é bem completo e tem também o [Rails Cast](http://railscasts.com/episodes/292-virtual-machines-with-vagrant), mas segue um resumão.

Supondo que o vagrant está instalado. Vamos adicionar uma máquina:


    $ vagrant box add lucid32 http://files.vagrantup.com/lucid32.box


Dentro da pasta do projeto, você tem criar o Vagrantfile e para isso execute:


    $ vagrant init lucid32  #ja especificando o box lucid32 baixado


Vamos subir a VM:


    $ vagrant up


Para acessar a VM:


    $ vagrant ssh


Repare que dentro da VM, na pasta "/vagrant" estará montado o diretório do seu projeto, onde está o Vagrantfile. Supondo que seja um projeto Rails, daí em diante você segue todo o fluxo _default_, instalando o _bundler_, dando um _bundle install_ e etc.

Vamos supor que você executou um "rails s" na VM e o projeto subiu na porta 3000. Para acessá-lo, você tem que configurar o forward_port no VagrantFile:


{% codeblock lang:ruby %}
config.vm.forward_port 3000, 4000  # 3000 from VM, available at 4000
{% endcodeblock %}


Dá um restart na VM:


    $ vagrant halt && vagrant up


Subir o projeto novamente. (vagrant ssh e o "rails s" dentro do /vagrant)

Acesse http://localhost:4000.


## Extras e Conclusão


A idéia deste post é explicar rapidamente o que é e como usar o Vagrant, mas com certeza o Vagrant tem muito mais a oferecer. Segue alguns tópicos, que valem posts:

[Provisioning](http://vagrantup.com/v1/docs/provisioners.html) - Existem várias ferramentas que podem te ajudar no _setup_ da sua VM. O Vagrant tem suport a Chef, Puppet e até mesmo Shell script.

[Plugins](http://vagrantup.com/v1/docs/plugins.html) - Você pode mudar ou adicionar funcionalidades ao Vagrant, criando plugins. É claro que sempre vale a pena googlar antes. Por exemplo o [vagrant-snap](https://github.com/t9md/vagrant-snap), que ajuda a tirar e gerenciar snapshots da VM.

[Boxes](http://vagrantup.com/v1/docs/boxes.html) - No exemplo acima, usamos um box de Ubuntu, mas nada impede de você criar ou utilizar outros boxes. Existe o [vagrantbox.es](http://vagrantbox.es/) que pode te ajudar a baixar uma existente, ou você vai ter que se aventurar pelos docs para criar uma zerada.
