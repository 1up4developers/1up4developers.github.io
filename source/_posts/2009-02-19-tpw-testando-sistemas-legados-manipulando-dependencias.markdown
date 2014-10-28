---
author: rodrigopanachi
comments: true
date: 2009-02-19 15:48:35+00:00
layout: post
slug: tpw-testando-sistemas-legados-manipulando-dependencias
title: 'TPW - Testando sistemas legados: manipulando dependências'
wordpress_id: 328
categories:
- quick tips
- real world
- tutorial
tags:
- agilidade
- mocks
- pragmatismo
- testes unitários
---

Pela definição de [Michael Feather](http://www.amazon.com/Working-Effectively-Legacy-Robert-Martin/dp/0131177052), **código legado é código sem testes**! Não importa se o código foi escrito semana passada ou alguns anos atrás. Qualquer manutenção será de difícil entendimento por outra pessoa e não haverá garantias de seu funcionamento. Uma vez que não há "controle", é mais difícil rastrear as alterações; pior do que uma nova funcionalidade que não funciona, é uma funcionalidade antiga que começa a falhar. Este é um risco que um desenvolvedor não pode correr!


### Não altere código legado até que seja possível testá-lo


Um dos problemas mais comuns em sistemas legados é a interdependência de classes, ou seja, o [alto acoplamento](http://en.wikipedia.org/wiki/Coupling_(computer_science)), que sempre está ligado com a [baixa coesão](http://en.wikipedia.org/wiki/Cohesion_(computer_science)). Se estes termos são difíceis de entender, pense em acoplamento como sendo o grau com que as classes referenciam umas as outras e coesão o quanto uma classe está focada em realizar suas responsabilidades.

Para que seja possível testar o comportamento de uma classe "acoplada", o comportamento de suas dependências precisa ser [simulado](http://en.wikipedia.org/wiki/Method_stub). Isto normalmente é feito através de objetos falsos, ou [mocks](http://en.wikipedia.org/wiki/Mock_object), que são injetados na instância da classe em questão. Este padrão é conhecido como [Inversão de Controle ](http://pt.wikipedia.org/wiki/Invers%C3%A3o_de_controle)e [Injeção de dependência](http://pt.wikipedia.org/wiki/Inje%C3%A7%C3%A3o_de_depend%C3%AAncia), onde o controle sobre as dependências da classe são delegados à outro objeto, ou normalmente um [container de objetos](http://pt.wikipedia.org/wiki/Spring_Framework), responsável por [injetar](http://misko.hevery.com/2009/01/14/when-to-use-dependency-injection/) as dependências nas instâncias das classes. Simples, não?! Mas isso será detalhado em outro post...

Voltando para os testes, no [post anterior](http://1up4dev.org/2009/02/tpw-testando-sistemas-legados-automatizando-build/) começamos a organizar o projeto automatizando o build e centralizando a execução dos testes para evitar que fiquem "soltos" pelo código. Agora vamos nos concentrar em escrever os casos de teste, [refatorando](http://pt.wikipedia.org/wiki/Refatora%C3%A7%C3%A3o) o necessário para lidar com as dependências das classes.

Partindo da premissa que o projeto não possuí nenhum framework de inversão de controle, utilizaremos um certo "padrão" que permite manipular as dependências de uma classe por meio de herança, sem alterar seu comportamento original. **A idéia é resolver as dependências da classe através de getters protegidos, que podem ser sobrescritos em uma classe filha no momento do teste**. Isso permite que, na classe estendida, o método sobrescrito retorne um objeto mock, por exemplo, com o comportamento esperado para o teste.

Vamos tomar como exemplo, uma classe simples com algumas dependências e responsável por encapsular algumas regras de negócio referentes à _Estoque_.


{% codeblock lang:java %}
public class EstoqueLogic {
    public boolean verificaDisponibilidade(Produto produto, Integer quantidade) {
        EstoqueDAO dao = new EstoqueDAO();
        Estoque estoque = dao.localizaProduto(produto.getCodigo());
        return estoque.getQuantidade() >= quantidade;
    }
}
{% endcodeblock %}


Da forma como esta classe foi escrita, é impossível testar a regra de _disponibilidade_ independentemente, pois depende do objeto _EstoqueDAO_ para localizar as informações necessárias para o método. Mas com um pequeno refactoring, a responsabilidade de resolver a dependência _EstoqueDAO_ passa a ser responsabilidade da própria classe _Estoque_:


{% codeblock lang:java %}
public class EstoqueLogic {
    public boolean verificaDisponibilidade(Produto produto, Integer quantidade) {
        EstoqueDAO dao = getEstoqueDAO();
        Estoque estoque = dao.localizaProduto(produto.getCodigo());
        return estoque.getQuantidade() >= quantidade;
    }
    protected EstoqueDAO getEstoqueDAO() {
        return new EstoqueDAO();
    }
}
{% endcodeblock %}


Desta forma, é possível "injetar" um objeto que simule a dependência do _EstoqueDAO_ estendendo a classe e sobrescrevendo o método _getEstoqueDAO()_ para retornar a instância desejada. O teste ficaria mais ou menos assim:


{% codeblock lang:java %}
public class EstoqueLogicTest {

    public void testVerificandoDisponibilidadeDeUmProduto() {

        //Criando o objeto EstoqueDAO mock, simulando o comportamento desejado
        final EstoqueDAO estoqueDAOMock = new EstoqueDAO() {
            @Override
            public Estoque localizaProduto(String codigo) {
                Produto produto = new Produto();
                produto.setCodigo(codigo);
                Estoque estoque = new Estoque();
                estoque.setProduto(produto);
                estoque.setQuantidade(5);
                return estoque;
            }
        };

        //Sobrescrevendo o método getEstoqueDAO para retornar o Mock
        EstoqueLogic logic = new EstoqueLogic() {
            @Override
            protected EstoqueDAO getEstoqueDAO() {
                return estoqueDAOMock;
            }
        };

        //Definindo o teste e executando
        Produto produto = new Produto();
        produto.setCodigo("123456");

        boolean estaDisponivel = logic.verificaDisponibilidade(produto, 10);
        assertTrue(estaDisponivel);

        boolean naoEstaDisponivel = logic.verificaDisponibilidade(produto, 2);
        assertTrue(naoEstaDisponivel);
    }
}
{% endcodeblock %}


O método_ getEstoqueDAO() _da classe _EstoqueLogic_ foi sobrescrito para retornar o objeto _estoqueDAOMock_ com as informações necessárias para o teste, ou seja, o comportamento das dependências foi simulado, possibilitando que o teste ficasse concentrado apenas da classe _Estoque_.


### Elimine as dependências e teste onde os bugs estão!


Este padrão fornece apenas uma maneira de lidar com as dependências das classes para escrever testes. A dica aqui é manter o [foco](http://1up4dev.org/2008/11/foco-no-problema/): defina apenas testes para as funcionalidades que estiver alterando e que exercitem pontos críticos e/ou regras de negócio. Então, refatore apenas as classes necessárias para simular e validar o fluxo destes testes, nem que seja apenas seus "[contratos](http://www.fragmental.com.br/wiki/index.php/Contratos_Nulos)". Não há necessidade de escrever testes muito granulares nem alterar todas as classes de um sistema legado.

No caso de classes com muitas dependências, o melhor é refatorá-la, separar as responsabilidades e testá-las individualmente. Para classes com dependências de Utils e/ou muitas chamadas à métodos estáticos, uma estratégia parecida pode ser utilizada. Mas estes são assuntos para os próximos posts. Acompanhem!
