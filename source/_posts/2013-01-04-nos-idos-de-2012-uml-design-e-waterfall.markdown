---
author: rramos
comments: true
date: 2013-01-04 15:51:14+00:00
layout: post
slug: nos-idos-de-2012-uml-design-e-waterfall
title: Nos idos de 2012, UML, Design e Waterfall
wordpress_id: 1258
categories:
- projetos
- real world
- waterfall
tags:
- uml
- waterfall
---

Há alguns anos atrás não havia uma referencia forte e consistente sobre os processos de desenvolvimento de software que não fosse Waterfall. Embora movimentos ágeis, processos mais simples e eficazes venham sendo utilizados a muito mais tempo, eles não eram tão evidentes como agora.
Independente do processo ágil discutido [Scrum](http://scrum.org), [Lean](http://www.lean.org), [XP](http://xprogramming.com) e etc, etc, etc... o movimento para remover as velhas e engessadas práticas de desenvolvimento de software cresce vertiginosamente e começa a movimentar grandes empresas, que ainda amarradas e processos internos pesadíssimos, entendem que algo precisa mudar para se conseguir maior flexibilidade e agilidade ao entregar novos serviços e funcionalidade a seus clientes, e obviamente, estar à frente da concorrência.

Em meio a corrida do novo ouro, me encontro em uma sala de treinamento, às vesperas de um novo ano, estudando, discutindo e demonstrando como analisar e modelar sistemas utilizando a mais famosa linguagem de modelagem: a [UML](http://pt.wikipedia.org/wiki/UML).
Nunca consegui traçar uma ligação saudável entre os modelos criados com UML e código funcionando em produção. A idéia principal da UML é a de comunicar aos envolvidos em um projeto o que se planeja implementar; quais os detalhes que norteiam o desenvolvimento de uma solução e que [requisitos funcionais e não funcionais](http://www.batebyte.pr.gov.br/modules/conteudo/conteudo.php?conteudo=1718) devem ser implementados. O problema é que qualquer coisa diferente de código no desenvolvimento de sistemas, está fadada a diferentes interpretações, ao conhecimento e experiência de quem [produz](http://1up4dev.org/2008/11/arquiteto-cascateiro) e consome tais artefatos.

A idéia de times multidisciplinares e autogerenciáveis trazida pelo movimento ágil distoa fortemente do [modelo cascata](http://en.wikipedia.org/wiki/Waterfall_model), que delinea claramente o papel do analista de negócios/requisitos, o arquiteto/designer da solução e os <del>pobres</del> desenvolvedores que terão de seguir à risca todas definições impostas pelos modelos produzidos. E se durante o ciclo ágil os problemas identificados são priorizados para serem endereçados no próximo ciclo, como o processo formal gerencia isso? Hum... daí vem minha maior crítica quanto ao uso de modelos no desenvolvimento de software. Já que se decidiu por engessar o processo, seguí-lo fielmente deveria ser o preço a ser pago para manter tanta parafernalha de artefatos sem valor. Identificado o problema, o fluxo deveria voltar lá no início e corrigir requisitos, modelos, código e testes; mas o mercado não permite tanta demora, as linhas de negócio precisam colocar seu produto na prateleira e o fluxo controladamente perfeito que outrora se desenhou, na vida real não funciona mais.

É inviável manter a "documentação" do sistema em face a uma concorrência e volatilidade de negócios tão vorazes, então eu me pergunto: O que aquelas pessoas estavam fazendo trancadas numa sala, consumindo o tempo a um alto [custo](http://www.stanford.edu/group/fms/fingate/staff/capitalequip/capital_software.html) da sua empresa? Aprendendo a como não fazer? Pode ser. Constatando uma vez mais que embora no papel, no processo, tudo aquilo que a teoria diz é muito bonito e controlado mas não funciona no mundo real? Sim, pode ser também, mas o pior é que passados anos de experiências ruins, projetos fracassados e montanhas de dinheiro jogados no ralo, ainda terão coragem de propor um processo baseado em requisitos → modelagem → desenvolvimento → testes, faseados e interdependentes, ignorando o histórico de dores e prejuízos experimentado.
