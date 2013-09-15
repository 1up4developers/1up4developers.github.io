---
author: rodrigopanachi
comments: true
date: 2009-02-12 02:01:31+00:00
layout: post
slug: tpw-testando-sistemas-legados-automatizando-build
title: 'TPW - Testando sistemas legados: automatizando o build'
wordpress_id: 307
categories:
- quick tips
- real world
tags:
- java
- pragmatic waterfall
- testes unitários
---

Imagine o cenário: você caiu de para-quedas naquele projeto que todo mundo na empresa fez gambiarra deu manutenção e agora precisa implementar uma nova funcionalidade. Mesmo que tenha alguma documentação, vai ser inútil neste caso. Então você começa a vasculhar o código e encontra dezenas de classes com nomes parecidos, vários arquivos XML's dos inúmeros frameworks utilizados anteriormente, [TO's, VO's](http://www.fragmental.com.br/wiki/index.php/Evitando_VOs_e_BOs), Actions, Utils... ou seja, um lugar cheio de [janelas quebradas](http://blog.improveit.com.br/articles/2007/01/05/nada-de-janelas-quebradas).

O mais fácil neste caso seria fazer seu trabalho, quebrando mais algumas janelas caso necessário, e cair fora o quanto antes. Mas você, [desenvolvedor ágil](http://agilemanifesto.org/), sabe que além de ser falta de profissionalismo, você corre o risco de cair novamente no mesmo projeto. Então aproveite a oportunidade para fazer uma pequena faxina e preparar o projeto para escrever os testes das novas funcionalidades que irá desenvolver, garantido assim a qualidade, pelo menos, do seu trabalho.

Durante minha carreira fui obrigado tive a oportunidade de dar manutenção em diversos projetos "[frankenstein](http://blog.fragmental.com.br/2006/04/02/2006-o-inicio-da-arquitetura-heterogenea-java-ee-ou-qual-o-melhor-framework-web/)" de onde adquiri certa experiência para poder "organizar a casa" e trabalhar decentemente no código. É claro que não estou falando em escrever testes para o projeto inteiro, mas pelo menos garantir a qualidade do código que desenvolvi ou irei desenvolver.

Há um certo padrão que todo software de qualidade deve seguir. Além dos padrões e boas práticas como organização do código em pacotes vs. responsabilidade, uma coisa essencial para o projeto é sua construção, ou seja, a forma que o software é "entregue". Acredito que este seja o primeiro passo para começar a organizar a bagunça de um projeto.

A construção do projeto deve ser automática e desempenhada por uma ferramenta de build como o [Ant](http://ant.apache.org/index.html) ou [Maven](http://maven.apache.org/), com tarefas (tasks) e objetivos bem definidos. Normalmente, um build do projeto deve:



	
  1. Preparar o código e dependências

	
  2. Compilar os arquivos fontes

	
  3. Executar os testes

	
  4. Empacotar a distribuição


No exemplo abaixo usarei o Ant, que é a ferramenta de build (para Java) mais popular do mercado e muito simples de utilizar. O importante neste processo é ser pragmático: não perca o foco! Você poderia (e futuramente deveria) utilizar o Maven, mas adequá-lo a um sistema legado não é tão simples quanto parece.

Tendo os objetivos do build definidos, basta escrever o script do Ant. Isso deve ser feito no arquivo build.xml, no root do projeto. As tarefas são definidas pelas tags <target> e podem ser dependentes para garantir a sequencia de execução. Em cada target são definidas as ações executadas, como rodar o compilador (javac), copiar um arquivo, rodar os testes, etc. Uma vez que as tarefas estão configuradas, basta executar o build pelo próprio IDE ou linha de comando.

    
    <code><project name="frank" default="package">
        <path id="classpath">
            <pathelement location="build/bin"/>
            <fileset dir="src">
                <include name="*.java"/>
            </fileset>
            <fileset dir="lib">
                <include name="*.jar"/>
            </fileset>
        </path>
        <target name="clean">
             <delete dir="build"/>
              <mkdir dir="build"/>
        </target>
        <target name="compile" depends="clean">
            <javac srcdir="src" destdir="build">
                <classpath refid="classpath"/>
            </javac>
        </target>
        <target name="test" depends="compile">
            <junit>
                <classpath refid="classpath"/>
                <batchtest>
                    <fileset dir="build" includes="*Test"/>
                </batchtest>
            </junit>
        </target>
        <target name="package" depends="compile, test">
            <war destfile="frank.war" webxml="web/WEB-INF/web.xml">
            <classes dir="build"/>
        </target>
    </project>
    </code>


Neste script estão definidas as tarefas necessárias para executar um build, conforme citei anteriormente. Por enquanto isto será o mínimo necessário para dar suporte as próximas etapas da sua missão. Mesmo que o projeto já tenha um build em Ant aproveite para [revisá-lo](http://www.onjava.com/pub/a/onjava/2003/12/17/ant_bestpractices.html). Tarefas bem simples e bem definidas são mais fáceis de entender e manter.

Com o build implementado, você não terá mais que se preocupar com o empacotamento do projeto. Agora comece a direcionar seus esforços para escrever os testes. Deixe que o Ant se encarregará de executá-los para você.

No próximo post vou apresentar alguns padrões e práticas de refactoring utilizados para possibilitar escrever testes que dependem de classes legadas do projeto.
