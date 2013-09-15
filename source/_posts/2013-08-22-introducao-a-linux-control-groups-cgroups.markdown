---
author: tonyfabeen
comments: true
date: 2013-08-22 17:32:23+00:00
layout: post
slug: introducao-a-linux-control-groups-cgroups
title: Introdução a Linux Control Groups (CGroups)
wordpress_id: 1425
categories:
- web
---

Em tempos **Metodologias Àgeis**, iniciativas como **[DevOps](http://en.wikipedia.org/wiki/DevOps)**, adoção de **Cloud Computing** e derivados **(SaaS, IaaS e PaaS)**, aplicações que demorariam meses, senão anos para estar na **www**, hoje em questão de dias, e por que não horas, é possível estar disponíveis ao usuário final.

Com a necessidade de ter os aplicativos de forma mais rápida em produção, a adoção e criação de **[PaaS (Platform as A Service)](http://en.wikipedia.org/wiki/Platform_as_a_service)** tem sido a nova "_onda do verão_" e tecnologias como **[LXC](http://lxc.sourceforge.net/)**, **[Docker](http://www.docker.io/) **e **[CGroups](http://en.wikipedia.org/wiki/Cgroups)** atuam como o cerne dessa "wave".


### O que são CGroups ?


**CGroups** é uma feature do Kernel que provê mecanismos para organização de Processos em forma de grupos e limita recursos de máquina como Consumo de CPU, memória e I/O para estes.

Curioso pra ver como funciona ?


### Situação de Exemplo


Para este exemplo teremos duas aplicações _[Sinatra](https://gist.github.com/tonyfabeen/6310137)_ e nosso objetivo será dedicar um grupo para cada aplicação limitando o consumo de memória para cada uma elas.

Para rodar o exemplo estarei utilizando um _Ubuntu 12.04 64 bits._


#### Pré-Requisitos


Antes de mais nada precisamos instalar algumas dependências:

    
    <code>sudo apt-get install cgroup-bin libcgroup1
    </code>


Com a instalação dos pacotes acima veremos que um novo filesystem foi montado em **/sys/fs/cgroup **.

    
    <code>ls -al /sys/fs/cgroup
    
    drwxr-xr-x 7 root root 140 Aug  6 09:38 .
    drwxr-xr-x 6 root root   0 Aug  6 09:37 ..
    drwxr-xr-x 6 root root   0 Aug  6 09:38 cpu
    drwxr-xr-x 6 root root   0 Aug  6 09:38 cpuacct
    drwxr-xr-x 6 root root   0 Aug  6 09:38 devices
    drwxr-xr-x 6 root root   0 Aug  6 09:38 freezer
    drwxr-xr-x 6 root root   0 Aug  6 09:38 memory
    
    </code>


CGroups estão organizados por subsistemas conhecidos também como "resource controllers" responsáveis por gerenciar memória, cpu, dispositivos, entre outras coisas. Na organização acima cada diretório representa um **Resource Controller**.


#### CGConfig Service


Para gerenciar CGroups iremos utilizar a utilitário _cgconfig_ instalado como o pacote _libcgroup1_. É interessante checar se o serviço está rodando antes de continuar :

    
    <code>$ sudo service cgconfig status</code>


Caso não esteja inicie o serviço

    
    <code>$ sudo service cgconfig start</code>


Existem duas formas de configurar CGroups com cgconfig, diretamente no arquivo de configuração **/etc/cgconfig.conf'** ou via linha de comando, que será o meio que iremos utilizar.


#### Criando Grupos


Para criar um grupo, utilizamos o comando **cgcreate** passando como argumento quais controllers estarão associados a ele.

    
    <code>sudo cgcreate -g cpu,cpuacct,devices,memory,freezer:/sinatra1
    sudo cgcreate -g cpu,cpuacct,devices,memory,freezer:/sinatra2
    </code>


O argumento **/sinatra*** indica o caminho relativo do grupo dentro de cada Resource Controller. Ex : **/sys/fs/cgroup/<resource_controller>/<path>**


### Executando programas em um Grupo


Para executar determinado processo em um grupo utilizamos o comando **cgexec** passando como argumentos quais controllers estarão associados ao processo e o caminho do grupo que ele estará associado.

    
    <code>sudo cgexec -g *:/sinatra1 sh -c 'cd <path_to_sinatra1> && exec rackup -p 4567 -D'
    sudo cgexec -g *:/sinatra2 sh -c 'cd <path_to_sinatra2> && exec rackup -p 4568 -D'
    </code>


O asterisco **(*)** acima significa que o processo estará associado a todos os controllers.

Para checar a hierarquia criada:

    
    <code>ps xawf -eo pid,cgroup,args | grep ruby
     1476              \_  5:freezer:              \_ grep --color=auto ruby
     1418  5:freezer:/sinatra1?4:memo /usr/bin/ruby1.9.1 /usr/local/bin/rackup -p 4567 -D
     1454  5:freezer:/sinatra2?4:memo /usr/bin/ruby1.9.1 /usr/local/bin/rackup -p 4568 -D
    </code>


Para setar os valores em determinado controller utilizamos o comando **cgset**. No caso abaixo estamos limitando o consumo de memória para o grupo **sinatra1** em **256MB** e para o grupo **sinatra2** em **128MB**.

    
    <code>sudo cgset -r memory.limit_in_bytes='256M' sinatra1
    sudo cgset -r memory.limit_in_bytes='128M' sinatra2
    </code>


Para checar a alteração :

    
    <code>cat /sys/fs/cgroup/memory/sinatra1/memory.limit_in_bytes
    cat /sys/fs/cgroup/memory/sinatra2/memory.limit_in_bytes
    </code>




### Conclusão


O intuito deste artigo foi demonstrar um dos possíveis usos de CGroups. Caso a aplicação **sinatra1** cair por estouro de memória ou alguma outra falha que não seja a destruição da máquina, a aplicação **sinatra2 **continuará funcionando.

Há mais a se explorar, poderíamos inserir limitação de I/O, consumo de banda, entre outras coisas. Poderíamos até criar nossa própria implementação de LXC, mas isso é assunto para um próximo encontro.

Os links abaixo exploram mais detalhes sobre o assunto :



	
  * [https://www.kernel.org/doc/Documentation/cgroups/cgroups.txt](https://www.kernel.org/doc/Documentation/cgroups/cgroups.txt)

	
  * [http://linux.oracle.com/documentation/EL6/Red_Hat_Enterprise_Linux-6-Resource_Management_Guide-en-US.pdf](http://linux.oracle.com/documentation/EL6/Red_Hat_Enterprise_Linux-6-Resource_Management_Guide-en-US.pdf)


Divirtam-se !
