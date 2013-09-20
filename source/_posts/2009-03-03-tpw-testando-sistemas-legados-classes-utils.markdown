---
author: rodrigopanachi
comments: true
date: 2009-03-03 14:11:00+00:00
layout: post
slug: tpw-testando-sistemas-legados-classes-utils
title: 'TPW - Testando sistemas legados: classes Utils'
wordpress_id: 330
categories:
- quick tips
- tutorial
tags:
- ddd
- java
- mocks
- pragmatic waterfall
- testes unitários
---

Aproveitando o gancho do [post anterior](http://1up4dev.org/2009/02/tpw-testando-sistemas-legados-manipulando-dependencias/) sobre manipulação de dependências, decidi dedicar um post apenas sobre este tema, pois acredito ser de grande ajuda para todos desenvolvedores que precisam manter código legado.

Em projetos legados é comum encontrarmos classes Util (aka Helpers) espalhadas por todo o código, fazendo desde coisas simples como formatar datas ou números, até coisas mágicas como cache de objetos, operações com [reflection](http://en.wikibooks.org/wiki/Java_Programming/Reflection/Overview), escrita de logs, etc. Mesmo que tenham sua "utilidade", são um [terror](http://en.wikipedia.org/wiki/Separation_of_concerns) quando falamos de testes! Além de ser um [forte indício](http://en.wikipedia.org/wiki/God_object) de um [design fraco](http://en.wikipedia.org/wiki/Single_responsibility_principle), as chamadas a seus métodos, geralmente estáticos, geram dependências nas classes que a utilizam.

Este é a estrutura comum (bem simplificada) de uma classe Util com métodos estáticos:


{% codeblock lang:java %}
public class MagicUtil {
    public static String getConstanteSecreta() {
        return "VALOR_SECRETO_AMBIENTE";
    }
}
{% endcodeblock %}


Neste caso, uma boa estratégia para os testes seria encapsular a chamada da classe Util em um método _protected_, para que seja sobrescrito na classe de teste, assumindo o comportamento desejado. Porém, se a classe Util for largamente referenciada no projeto (o que é comum) seria preciso _refatorar_ todas a classes que a utilizam para escrever um teste completo.

A estratégia proposta é [refatorar](http://pt.wikipedia.org/wiki/Refatora%C3%A7%C3%A3o) a classe Util, aplicando o padrão [Singleton](http://pt.wikipedia.org/wiki/Singleton) e transformando os métodos estáticos em métodos de instância, porém mantendo as assinaturas estáticas, que devem referenciar os métodos da instância. Uma vez que a Util pode ser instanciada (mesmo que internamente), é possível manipular seu comportamento através da injeção de um objeto _Mock_, por exemplo_:_


{% codeblock lang:java %}
public class MagicUtil {
    private static MagicUtil instance = new MagicUtil();
    protected static MagicUtil getInstance() {
        return instance;
    }
    protected static void setInstance(MagicUtil obj) {
        instance = obj;
    }
    public String getConstanteSecretaInstancia() {
        return "VALOR_SECRETO_AMBIENTE";
    }
    public static String getConstanteSecreta() {
        return getInstance().getConstanteSecretaInstancia();
    }
}
{% endcodeblock %}


Assim, a Util continua com o mesmo comportamento e seu **contrato** foi mantido. Agora, no seu teste, basta escrever o mock (estendendo a classe e sobrescrevendo os métodos) e injetá-lo na instância interna da Util:


{% codeblock lang:java %}
public class ClasseTest {
    @Test
    public void testaMetodoDependenteDeMagicUtil() {
        new MagicUtil() {
            {
                setInstance(this);
            }
            public String getConstanteSecretaInstancia() {
                return "MEU_VALOR_MOCK";
            }
        };
        Assert.assertEquals("MEU_VALOR_MOCK", MagicUtil.getConstanteSecreta());
    }
}
{% endcodeblock %}


Neste caso a classe anônima (que estende a Util) passa sua própria instância (this) para o método protegido _setInstance()_. Note que a chamada do método (estático) da Util continua igual ao da classe original, sem o refactoring.

Nos projetos que preciso manter, esta estratégia tem sido muito útil para resolver os problemas das "teias" de Utils. Porém é um recurso paliativo e [não deve ser utilizado](http://gcirne.wordpress.com/2008/01/14/classes-util/) como o "padrão". O ideal é sempre evitar classes Utils, lembrando que uma classe deve sempre ter comportamentos bem definidos e o [nome já deve indicar](http://domaindrivendesign.org/) sua responsabilidade.
