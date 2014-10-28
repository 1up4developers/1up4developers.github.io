---
author: rogerbarreto
comments: true
date: 2009-01-15 10:32:52+00:00
layout: post
slug: tpw-dicas-para-a-qualidade-do-software
title: TPW - Dicas para a qualidade do Software
wordpress_id: 280
categories:
- real world
- web
tags:
- dicas
- pragmatic waterfall
- real world
---

Desenvolvedores em geral sabem como é chato quando uma tela que fez ou alterou dá erro durante uma homologação ou até produção. No caso de um sistema Web, a raiva aumenta ainda mais se a causa for incompatibilidade de navegadores.

Antes de começar a apresentar o "the best of my rotina", é bom deixar bem claro o meu cenário:



	
  * Trabalho sozinho no projeto, sou humano e faço pair programming com a cpu, que eu tenho certeza que é [pogger](http://desciclo.pedia.ws/wiki/POG)!

	
  * O meu cliente, o que requisita correções e/ou novas funcionalidades, é um cliente interno e eu tenho acesso direto a ele.

	
  * O sistema não tem testes unitários, testes de integração e etc.

	
  * Muito código foi copiado, não somente na camada de negócio, mas também nas views. As páginas por aqui, também são conhecidas como _business-view_.

	
  * Tem mais complicações, mais acho que já tem o suficiente para entenderem o meu cenário.


O que já fizemos para começar a arrumar a casa:

	
  * Implantamos um bug tracker, conhecido como [Redmine](http://www.redmine.org/). Sabe aquele velho problema de planilha pra lá e pra cá e ninguém nunca sabia quem, e o que estava fazendo? Pois bem, este problema está quase resolvido aqui (quase porque ainda tem projeto faltando para migrar).

	
  * Implantamos um servidor de integração continua, com o [Hudson](https://hudson.dev.java.net/). Apesar de eu não ter testes, havia um sério problema aqui para implantar novas versões em homologação e produção, e agora com o Hudson centralizando o build, ele mesmo já disponibiliza o war.

	
  * Por sinal, montar e começar a usar o war foi outro trampo também.


Sabendo que o meu contexto é diferente do seu, sinta-se livre para adaptar qualquer coisa. Tentei deixar as dicas o mais genérico possível.


### 1. Trabalhe com tarefas Curtas


Tarefas curtas são muito mais fáceis para desenvolver, testar e se livrar! Por exemplo, se tem que desenvolver aquele formulário com vinte telas, não tenha dúvida, quebre isto em pequenas funcionalidades. Para identificar as partes "quebráveis", leve em conta o que a torna funcional, ainda no exemplo anterior, se das vinte telas, você fizer a primeira e a última são suficientes para um cadastro básico, está ai a sua primeira tarefa. Muitas pessoas não dão importância para isso, mas saber "extrair" as tarefas certas de um novo desenvolvimento ou manutenção, traz um leque de vantagens:



	
  * Testar uma funcionalidade curta é muito mais rápido e fácil do que testar uma gigantesca;

	
  * Fica mais dificil perder o foco.

	
  * Os prazos ficam mais "coerentes".

	
  * Você pode retirar ou incluir novas funcionalidades, a "negociação" com o cliente fica mais simples.




### 2. Monte e use um roteiro de teste


Pra quem usa [TDD](http://en.wikipedia.org/wiki/Test-driven_development), este passo pode pular. Pra quem trabalha com legados e ainda não tem testes (meu caso), eu costumo fazer um roteiro de testes antes de implementar a funcionalidade curta. Este roteiro costuma ser enxuto e abrangente, isto permite que eu descanse meu cerebro enquanto testo, pois com ele, acaba se tornando um processo mecânico. Vou colocar um exemplo recente de roteiro que usei:

**Teste de Primeiro Acesso**



	
  * Limpar a base, apagar os registros da tabela xpto e suas dependências.

	
  * Limpar os cookies.

	
  * Acessar o index.jsp da Aplicação (esta página simula um login via cookie).

	
  * A tela inicial só permite cadastrar os dados do perfil, os links "xxx" e "zzz" não ficam disponíveis.

	
  * Após o Teste de Restrições ao Editar o Perfil, verificar que os links "xxx" e "zzz" estão disponíveis.

	
  * Clicar em salvar novamente para verificar se o problema de "unique id" não ocorre.


Estes roteiros se tornaram um costume, eu perco pouquissimo tempo pra fazer e apesar de parecer besta, ele me ajuda muito na hora de executar um teste de sanidade por exemplo. O ideal seria transformar este roteiro num teste unitário ou até mesmo num script via Selenium, mas devido a estrutura (ou a falta dela?!) eu ainda não consegui esta automação tão sonhada.
Apesar deste roteiro ser descartável e somente para ajudá-lo, uma idéia legal que venho fazendo é colocá-lo como comentário caso use um bug tracker.


### 3. Teste seu sistema num ambiente isolado


Estes últimos dois anos tenho usado [linux](http://www.ubuntu.com/) no desktop para desenvolvimento. Infelizmente, todos nós sabemos que todo sistema web tem que ser testado nos IE*s e cia. É claro que durante o inicio do desenvolvimento, uso meu firefox local mesmo, mas assim que termino a funcionalidade, uso uma máquina virtual para averiguar a compatibilidade com os navegadores mais utilizados.
Usando o [Virtual Box](http://www.virtualbox.org/) e [Multiple IEs](http://tredosoft.com/Multiple_IE) mais o opcional roteiro de testes, consigo validar o sistema numa boa gama de navegadores. Para usar o Multiple IEs não tem segredo, é só atualizar para o IE7 e depois rodar o executável do mesmo.


[![](/images/uploads/2009/01/virtualbox_multiple_ies-300x187.png)](/images/uploads/2009/01/virtualbox_multiple_ies.png)





### 4. Automatize o que for possível


Já ocorreu de você precisar preencher _n_ campos, navegar em _n_ telas e descobrir que você errou o nome de uma variável javascript e ter que fazer tudo de novo? Pois bem, comigo já aconteceu muito, e uma das soluções que venho usando é o [Selenium Ide](http://seleniumhq.org/projects/ide/). Com o plugin do firefox, eu gravo scripts temporários (durante o desenvolvimento da tarefa) que preenche os _n_ campos e navegam nas _n_ telas, assim pelo menos este tempo de navegação eu só perco uma vez.


### Finalizando ...


Todas estas "técnicas" nada mais são do que uma retrospectiva minha, de uma tentativa (frustada por sinal) de Scrum Solo. Aqui estou colocando o que vem dando resultado, e se você achou besteira ou legal, gostaria muito do seu comentário. Sabe aquela [frase da maça e conhecimento](http://www.jlcarneiro.com/macas-ideias-e-conhecimento/), então, minha única expectativa com este post é esta: Novas Idéias!

Valeu e feliz ano novo a todos!
