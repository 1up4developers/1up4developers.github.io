---
author: rodrigopanachi
comments: true
date: 2008-11-04 03:49:11+00:00
layout: post
slug: os-guardioes-da-cascata
title: Os guardiões da cascata
wordpress_id: 228
categories:
- cascata
- marketing
- processos
- resenhas
tags:
- devaneios
- processos
---

Se você freqüenta este blog já deve ter percebido que nós não gostamos da maldita [cascata](http://pt.wikipedia.org/wiki/Modelo_em_cascata). Fases bem definidas, detalhamento de requisitos, documentos inúteis, diagramas UML, papéis... tudo muito lindo na teoria. Eu fico até emocionado quando leio a [documentação](http://www.wthreex.com/rup/) do [RUP](http://pt.wikipedia.org/wiki/Rational_Unified_Process). Mas infelizmente a maioria dos profissionais de TI precisam são obrigados a trabalhar nestes ambientes cascateiros, enfrentando chefes sem noção, colegas com [síndrome do funcionário público](http://www.novacorja.org/), [prazos sem sentido](http://desciclopedia.org/wiki/POG#Prazos_de_um_projeto_POG), entre outras [pérolas](http://1up4dev.org/2008/09/contos-do-programador-pragmatico/) da [área](http://1up4dev.org/2008/10/a-perpetuacao-da-especie/).

O principal apelo de um processo cascateiro são suas fases e papéis bem definidos, onde cada membro da "equipe" é responsável por uma determinada tarefa que é executada em uma seqüência previamente definida. Dentre os [papéis](http://www.wthreex.com/rup/process/workers/ovu_works.htm), pode-se facilmente identificar os especialistas daquela tarefa, que defendem sua necessidade execução com unhas e dentes. Para ilustrar, resolvi chamá-los de [guardiões](http://pt.wikipedia.org/wiki/Guardi%C3%B5es_do_Universo), seja da tecnologia ou da atividade em questão. Um guardião protege sua fase, tarefa e interesses, defendendo-os para que o "processo" não seja quebrado. Desta forma, <sarcasmo> a "equipe" atinge seu objetivo: o software! </sarcasmo> Seguem alguns exemplos desses guardiões cascateiros:

**O guardião do banco de dados: "_Não rodarás nenhum script na base alheia_"**
Começo por este por ser o mais comum dos guardiões. Ele trata o banco de dados como um filho, mesmo que seja um adolescente que não obedeça inteiramente à 3ª regra normal. São vistos como semi-deuses, capazes de transcrever o modelo de negócio da empresa em uma linguagem de alto nível, impossível de ser compreendida por simples programadores. Protejem as tabelas com a própria vida e qualquer alteração na base de dados é motivo para um duelo até a morte! Utilizam um padrão para nomenclatura de campos que somente é conhecido pelo clã dos DBAs. Geralmente são seguidores do Oráculo, o senhor de todos os bancos de dados.

**O guardião do projeto: _"Guia-te pelo teu Gantt e serás recompensado"_**
Este guardião está presente em todos os projetos, garantindo que a palavra do [Gantt](http://pt.wikipedia.org/wiki/Diagrama_de_Gantt) seja cumprida, protegendo o escopo do projeto com a própria vida (ou a vida de algum programador). Adicionalmente atua como roteador de atividades: recebe os requisitos pelo email, encaminha para um recurso disponível (programador) que estima o esforço e define uma data de entrega, devolvendo para o guardião que atualiza seu [Project](http://blog.aspercom.com.br/2007/11/15/ganttchartnaofunciona/).

**O guardião do framework: _"Venerarás o Struts e nada te faltarás"_**
O framework é o objeto de adoração deste guardião, nenhum outro framework é tão bom quanto o que ele venera. Ele provê solução para todos seus problemas simplesmente escrevendo um bloco de XML aqui, outro ali, mudando aquela linha acolá e estendendo uma classe X implementando aquela interface Z. Qualquer evolução do framework em questão não passa de uma tentativa frustrada de "reinventar a roda".

**O guardião da arquitetura: _"Não usarás a instância do teu objeto em vão"_**
Uma variação interessante de guardião, que neutraliza seu oponente através de técnicas de tortura e perturbação mental, inundando as sessões de brainstorm com uma enxurrada de DTO's, VO's, Facades, EJB's entre outros patterns que fazem a cabeça dos programadores entrar em conflito, até que seus órgãos faleçam (ou simplesmente se demitam). Geralmente são cúmplices dos guardiões do projeto, conspirando para a dominação do Gannt.

**O guardião do root: _"Teu processo não executarás no meu bash"_**
A jóia mais preciosa da empresa: a senha do root. Seu guardião é o mais honrado dos seres, sendo uma espécie de [Frodo](http://pt.wikipedia.org/wiki/O_Senhor_dos_An%C3%A9is), protegendo-a com a própria vida pois uma vez em mãos erradas pode ser usada para a destruição da humanidade (ou apenas para reiniciar aquela instância do Tomcat travado em produção). Aquele que desafia este guardião perde o direito de executar seus processos como administrador local e fica vagando pelo filesystem eternamente.

**O guardião dos guardiões: _"Tua TI é um mal necessário"_**
Também conhecido como diretor, presidente, CEO, dono, investidor, sócio, etc. É o guardião das decisões, aquele que protege sua riqueza acima de tudo, economizando nos salários, contratando funcionários despreparados e investindo rios de dinheiro em consultorias e licenças de software para garantir seus investimentos.

Enfim, são guardiões dos próprios interesses. A "equipe" é apenas uma palavra que usam em discursos mas nunca aplicaram o conceito na prática!
