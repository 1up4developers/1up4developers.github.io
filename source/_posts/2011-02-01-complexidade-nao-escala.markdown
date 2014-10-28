---
author: rodrigopanachi
comments: true
date: 2011-02-01 09:00:00+00:00
layout: post
slug: complexidade-nao-escala
title: Complexidade não escala!
wordpress_id: 800
categories:
- questionamento
- real world
- web
tags:
- escalabilidade
- simplicidade
---

Este é um assunto polêmico e que gera muita discussão nos escritórios de TI. Mesmo assim, tendo trabalhado em várias empresas, grandes e pequenas, presenciando vários projetos fracassarem por decisões arquiteturais (as gerenciais não contam nesse contexto), e os mesmos erros sendo cometidos repetidas vezes, sinto-me na obrigação de passar esta experiência e meu ponto de vista.


### Escalabilidade, por definição


De acordo com a [Wikipedia](http://en.wikipedia.org/wiki/Scalability), **escalabilidade** _é uma característica desejável em todo o sistema que indica sua habilidade de manipular uma porção crescente de trabalho de forma uniforme, ou estar preparado para crescer._ Em outras palavras, um sistema deve estar preparado para suportar uma demanda crescente.

Pode ser classificada como:




  * Carga de escalabilidade: um sistema distribuído deve ser fácil para ser expandido e usar sua gama de recursos para acomodar tanto exigências do mesmo sendo elas pouca ou excessiva.


  * Geograficamente escalável: um sistema é geograficamente escalável quando ele mantém sua utilidade e usabilidade, não obstante como são usados os seus recursos.


  * Escalabilidade administrativa: não importa a variação de informação que diferentes organizações necessitam compartilhar em um único sistema distribuído, ele deve permanecer fácil de ser usado e gerenciado.


  * Escalabilidade funcional: capacidade de melhorar o sistema, adicionando novas funcionalidades com mínimo esforço.


Pode ser aplicada como:


  * **Escalabilidade Vertical** (scale up): significa adicionar recursos em um único nó do sistema (mais memória ou um disco rígido mais rápido).


  * **Escalabilidade Horizontal** (scale out): significa adicionar mais nós ao sistema, tais como adicionar um novo computador com uma aplicação para clusterizar o software. Você pode encontrar escalabilidade horizontal nos sabores [Sistemas Distribuídos](http://pt.wikipedia.org/wiki/Computa%C3%A7%C3%A3o_distribu%C3%ADda), [Serviços de Componentes](http://en.wikipedia.org/wiki/Enterprise_JavaBean), [SOA](http://pt.wikipedia.org/wiki/Service-oriented_architecture), [System of Systems](http://en.wikipedia.org/wiki/System_of_systems), etc.


[![Escalabilidade](/images/uploads/2011/01/scale-up-300x150.png)](http://escalabilidade.com)
Do ponto de vista do custo de desenvolvimento, manutenção e suporte, eu defino escalabilidade como: caro (complexo) vs. barato (simples). Porém, o que é caro nem sempre é bom... e vice versa.


### Analogias com o mundo real


Nós gostamos de fazer [analogias com coisas reais](http://en.wikipedia.org/wiki/Extreme_programming_practices#System_metaphor), são ótimas para expressar idéias e facilitar a comunicação.

Call centers, ou centrais de atendimento. Se você mora no Brasil provavelmente já perdeu algumas horas de sua vida pendurado no telefone tentando resolver um problema com sua assinatura de celular, internet, TV, energia elétrica, etc. Mas por que isso acontece?
Provavelmente, essas centrais de atendimento começaram com poucos funcionários, porém foram "projetadas" para crescer conforme a demanda. Basta contratar mais funcionários, dar treinamento e pronto: mais "nós" atendendo as requisições. Entretanto, o processo passa a ser mais complexo e mais burocrático, pois é muita informação para disseminar entre os funcionários. Resultado: alto custo para empresa e pouco resultado para os clientes. A espera ainda existe e quando finalmente somos atendidos, a sensação é de estar falando com um robô, que dificilmente entenderá seu problema e dará uma solução padrão ou insatisfatória para o mesmo.

[![Call Center](/images/uploads/2011/01/call-center-300x225.jpg)](/images/uploads/2011/01/call-center.jpg)

O processo é adequado para escalar a demanda horizontalmente, o que o torna complexo e não garante a qualidade. Vale lembrar que o [software](http://pt.wikipedia.org/wiki/Sistemas_computacionais) apenas automatiza ou apóia a realização dos processos reais. Se o processo é complexo, o software será tão complexo quanto (e em alguns casos até mais complexo).

Um exemplo diferente: o [Fusca](http://pt.wikipedia.org/wiki/Volkswagen_Fusca). É um dos carros mais simples que existem, não precisa nem de refrigeração a água. Costuma-se dizer que pode ser consertado com um alicate e um rolo de arame. Seu custo de fabricação seria mínimo se comparado com o custo de um carro popular moderno. Ok, não oferece a mesma segurança, velocidade e conforto de um carro moderno, mas ambos conseguem transportar 5 passageiros ao destino desejado.

[![Fusca](/images/uploads/2011/01/fuka_herbie_oval-300x210.jpg)](/images/uploads/2011/01/fuka_herbie_oval.jpg)

Mas o que um Fusca têm a ver com escalabilidade? Bem, imagine um congestionamento na véspera de um feriado, por exemplo. Tanto o Fusca quanto o carro moderno ficarão parados na mesma fila de farofeiros. Toda tecnologia, complexidade e custo da fabricação do carro moderno não o livra de um congestionamento. Não há projeto que consiga prever ou contornar esse problema.

[![Twitter](/images/uploads/2011/01/baleia1-300x225.jpg)](/images/uploads/2011/01/baleia1.jpg)

Voltando para nossa realidade, em certos casos o custo não paga o benefício. Mesmo um sistema bem planejado, distribuído, separado em camadas e serviços independentes, pode não suportar a demanda, seja de carga ou de funcionalidade. O mesmo aconteceria com um sistema mais simples arquiteturalmente, com certeza. Porém, o custo e velocidade de desenvolvimento e manutenção seria bem menor.



### O que fazer então?



Sistemas precisam evoluir de acordo com a necessidade. [Projetar um sistema](http://merbist.com/2011/01/31/designing-for-scalability/) prevendo todos os possíveis "casos de uso" bem como suas hipotéticas cargas (ou acessos) é uma forte característica do _waterfall_. Sistemas mais simples arquiteturalmente evoluem mais facilmente, em outras palavras, escalam mais facilmente:





  * [**Keep it simple, stupid**](http://pt.wikipedia.org/wiki/Keep_It_Simple)! Soluções simples geralmente custam menos e trazem retorno mais rápido. Qualquer funcionalidade ou recurso que não agregue valor deve ser descartada. Simples assim!


  * **Simplifique requisitos**. Clientes _sem_ software costumam _viajar_ nos requisitos. Converse, escute e entenda exatamente seus [PROBLEMAS](http://1up4dev.org/2008/11/foco-no-problema/), então [negocie](http://pt.wikipedia.org/wiki/YAGNI) e sugira uma SOLUÇÃO simples e rápida. E a [entregue](http://pragprog.com/titles/prj/ship-it) logo!


  * **Busque soluções prontas**. As necessidades dos clientes, em muitos casos, são parecidas. Provavelmente existe alguma solução pronta que você pode entregar para seu cliente ao invés de desenvolver um novo [software que todos conhecem] do zero.


  * **Otimize sua infra-estrutura**. Procure a melhor forma de deploy da aplicação. Qual hardware é mais adequado, qual sistema operacional e arquitetura. Faça tunning do webserver (workers, pool, memória), módulos e load balance. Utilize uma versão otimizada do interpretador, como o [Ruby Enterprise Edition](http://www.rubyenterpriseedition.com/), [JRuby](http://www.jruby.org/) ou [Rubinius](http://rubini.us/).


  * **Monitore sua aplicação**. Uma vez que esteja em produção, monitore-a em busca de gargalos e possíveis pontos de otimização. Existem várias ferramentas para ajudá-lo nessa tarefa, como o [Hoptoad](http://hoptoadapp.com/), [NewRelic](http://newrelic.com/), ou até mesmo o [Rubinius Profiler](http://rubini.us/doc/en/tools/profiler/).


  * **Otimize sua aplicação**. O log é seu melhor amigo no desenvolvimento. Não ignore queries demoradas ou que geram [N+1](http://rails-bestpractices.com/posts/29-fix-n-1-queries), otimize-as! Faça _profiling_, analise e otimize o código. Utilize _caching_ para serviços externos e para páginas. Caching pode ser a melhor alternativa para turbinar sua aplicação, mas use-o com sabedoria. Bancos de dados não devem ser um problema, por isso saiba escolher a melhor distribuição para seu caso. Mesmo que [NoSQL](http://nosql-database.org/) esteja na moda, não quer dizer que é a melhor solução para sua aplicação (a menos que seja necessário para o negócio).


  * **Simplifique o desenvolvimento**. Fameworks ajudam bastante, mas use-os com sabedoria para não inflar o _stack_ da aplicação. Em certas situações, vale mais a pena desenvolver algo simples do zero do que utilizar algo pronto, pesado e que precise ser adaptado para sua necessidade. Escreva menos: quanto menor seu código, mais rápido será sua execução. Camadas são perigosas: evite rodeios e escreva código que realmente interessa. E lembre-se: sempre existe uma ferramenta certa para cada problema.





### SIMPLIFIQUE!



Tenha sempre a palavra [**SIMPLES**](http://zenhabits.net/simple-living-manifesto-72-ideas-to-simplify-your-life/) gravada na sua mente. Se uma coisa exigir muito esforço para ser feita, talvez nem valha a pena. Volte para o início, pense novamente e busque uma ALTERNATIVA mais rápida/fácil/barata. Quanto melhor for sua técnica, melhor serão suas escolhas. [Pratique constantemente](http://blip.tv/file/2733212). 
