---
author: pbalduino
comments: true
date: 2011-04-08 16:24:49+00:00
layout: post
slug: o-desenvolvedor-preguicoso
title: O desenvolvedor preguiçoso
wordpress_id: 925
categories:
- questionamento
---

Durante o almoço eu estava conversando com um colega e citei que, se tivesse uma empresa, gostaria de contratar um programador/desenvolvedor/engenheiro que fosse preguiçoso. Por meio minuto ele deve ter imaginado que eu estava louco, ou que queria abrir transformar minha empresa imaginária numa repartição pública.

Vou tentar ser breve na explicação.

O primeiro ponto é que uma pessoa preguiçosa, dentro desse contexto, é completamente diferente de uma pessoa acomodada, encostada ou vagabunda. Uma pessoa preguiçosa, novamente nesse contexto, é aquela que se incomoda de fazer várias vezes ao dia a mesma tarefa, e encontra meios de automatizar isso.

Um caso real: eu trabalhei numa projeto em que era necessário compilar os fontes, passar os binários resultantes (executáveis, DLLs ou JARs) por um software proprietário para que esses fossem convertidos num formato que a plataforma proprietária entenda, e então utilizar um outro software para jogar esse binário convertido para dentro do dispositivo.

O segundo passo, a conversão dos binários, era freqüentemente esquecido, fazendo com que um binário desatualizado fosse instalado no dispositivo, desperdiçando tempo e estressando os envolvidos.

Um colega, preguiçoso, criou um passo novo que foi adicionado ao [Maven](http://maven.apache.org/) para que essa geração fosse automatizada, reduzindo todo processo à compilação e instalação. Não fosse o software de instalação tão fechado, a coisa toda seria realizada num único processo.

O mesmo acontece com a utilização de testes automatizados. Poderíamos muito bem executar passo a passo todos os itens de uma planilha gigantesca cada vez que eu gerasse uma nova versão do sistema. Ao invés disso, escrevemos alguns testes automatizados (705 da última vez vi) que levam trinta segundos para executar toda vez que você altera o código. A cada nova funcionalidade, uma nova série de testes é escrita, fazendo com que a coisa seja menos trabalhosa para todos os envolvidos.

Automatizar tarefas repetitivas aumenta a produtividade, reduz drasticamente a possibilidade de que erros idiotas aconteçam e ainda te dá tempo para investir no que realmente interessa.

E você, está sendo preguiçoso ou acomodado?
