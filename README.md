# viernes-geek-java8

A principal motivação pelo qual criei este repositório é para nos atentarmos para as novidades da release 8 do Java e espero que possamos fazer bom proveito e que seja um início de grandes descobertas.

**Disclaimer:** A lista de novidades é imensa, [lista de novidades](http://www.oracle.com/technetwork/java/javase/8-whats-new-2157071.html), e a idéia inicial é abordar as novidades da linguagem que acredito ser as mais relevantes e que vão mudar a forma pelo qual codamos.

Em meados de 2015, a Oracle lançou o Java 8, que chegou com  muitas novidades e muito barulho que eu não via desde o lançamento do Java 5 (around 2004).

Esta release contém um tanto bom de funcionalidades, melhorias, correções de bugs e  features, improvements, bugs fixes e tunings para melhorar a eficiência e execução JAVA. E aqui nós iremos abordar as novidades da linguagem.


### Default Method

Agora é possível ter métodos concretos em interfaces. Basta utilizar o modificador **default**. Eles serão ‘herdados’ por todos que implementarem essa interface.

Com este recurso, pode se evoluir uma interface sem quebrar compatibilidade. É uma técnica já bem conhecida no C#, Scala e outras linguagens. 

Há muitos métodos default que foram adicionados a API de coleções, como Collection.removeIf, Map.getOrDefault e até mesmo no Comparator, como o Comparator.reversed, que devolve um novo comparador que ordena ao contrário.


### Functional Interface

Interface Funcional é uma interface que contém apenas um método abstrato. Este tipo de interface é usado em situações específicas, sendo comum a instanciação é através de uma classe anônima. 

```
@FunctionalInterface
public interface Runnable {
   public abstract void run();
}
```

Esta funcionalidade foi criada para suportar a utilização de Lambdas (que veremos no próximo tópico abaixo :D).

### Lambda Expression


Lambda Expression foi a maneira escolhida para que a linguagem Java finalmente possua “nuances” bem leves de programação funcional.
Sobre a definição de “programação funcional”, sempre existem controvérvisas, mas o meu brother Wikipedia afirma o seguinte:

“...Em ciência da computação, programação funcional é um paradigma de programação que trata a computação como uma avaliação de funções matemáticas e que evita estados ou dados mutáveis. Ela enfatiza a aplicação de funções, em contraste da [programação imperativa](https://www.wikiwand.com/pt/Programa%C3%A7%C3%A3o_imperativa), que enfatiza mudanças no estado do programa...”

Em resumo, utilizar lambda vai permitir passar funções/comportamentos, como argumentos em uma chamada de método. É um paradigma realmente diferente do que programadores Java estão acostumados, que desde então só escrevem métodos que aceitam objetos como parâmetro, e não outros métodos!

A novidade chegou um pouco atrasada no Java. Outras linguagens como Scala, C#, Python e até Javascript já fazem isso há tempos. 

Há quem diga que o uso de lambdas compromete a legibilidade do código. 

Em minha opinião existem pelo menos dois bons argumentos a favor das expressões lambda: 

1. O primeiro é o Paralelismo. Até então em Java, não havia uma maneira simples de escrever código para percorrer grandes coleções de objetos em paralelo. Várias APIs foram desenvolvidas possibilitando o processamento paralelo utilizando o Lambda. 

2. O segundo é fazer com que a linguagem bem fique menos verbosa :)

**Method Reference**

É comum um lambda simplesmente invocar um único método. 
Para deixar a coisa mais exuta, um lambda também pode ser escrito como forma de method reference.

Em vez de s -> s.length() fazemos simplesmente String::length, ficando implícito que queremos, para a String passada como argumento, que o length seja invocado.

### Stream

Stream é a API que chega como uma forma mais funcional de trabalhar com as coleções em Java. Para criar um Stream com os elementos de nossa lista só precisamos chamar o método defaut .stream() presente na interface Collection (e em outros lugares!). 

Esta API possui diversos métodos, como o filter, map e reduce, que recebem uma interface funcional como parâmetro, nos possibilitando tirar proveito dos novos recursos de lambda e method reference.

Os streams são diferentes das classes gerais da API Collections, pois não possuem armazenamento, podendo ser infinitos ou lidos sob demanda. São em essência processados funcionalmente e podem ser gerados sob demanda. Por exemplo, um Stream pode ser criado para representar números primos, com cada novo elemento produzindo o próximo número na sequência.

````
palavras.stream()
  .filter(s -> s.length() < 6)
  .forEach(System.out::println);
````

### Optional


Optional é uma classe que encapsula vários métodos úteis para tratar blocos de código crítico que necessitam ser checados. O Javadoc da mesma é bem objetivo: 

*"A container object which may or may not contain a non-null value. If a value is present, isPresent() will return true and get() will return the value."*

Esta classe nos permite uma elegante e efetiva alternativa para evitar o tão conhecido "NullPointerException".

````
public void printName(String name) {
	String filteredName = Optional.ofNullable(name).orElse("String nula");	
	System.out.println(filteredName);
}


````
````

//o método ifPresent recebe uma expressão lambda como parâmetro, 
//que consome o valor em optional, se este for presente

optional.ifPresent((valor)-> System.out.println(valor.get().toUpperCase()));

````
### Date

Uma nova API de datas disponível no pacote java.time e um dos principais conceitos dessa nova API é a separação de como dados temporais são interpretados em duas categorias: a dos computadores e a dos humanos.

####Datas para computadores

````
Instant agora = Instant.now();
System.out.println(agora); 
//2014-04-08T10:02:52.036Z (formato ISO-8601)
````
Podemos usar um Instant, por exemplo, para medir o tempo de execução de um algoritmo.

````
Instant inicio = Instant.now();
rodaAlgoritmo();
Instant fim = Instant.now();
 
Duration duracao = Duration.between(inicio, fim);
long duracaoEmMilissegundos = duracao.toMillis();

````

####Datas para humanos

````
LocalDate hoje = LocalDate.now();
System.out.println(hoje); //2014-04-08 (formato ISO-8601)

LocalDate emissaoRG = LocalDate.of(2000, 1, 15);

LocalDate homemNoEspaco = LocalDate.of(1961, Month.APRIL, 12);
LocalDate homemNaLua = LocalDate.of(1969, Month.MAY, 25);
 
Period periodo = Period.between(homemNoEspaco, homemNaLua);
 
System.out.printf("%s anos, %s mês e %s dias", 
  periodo.getYears() , periodo.getMonths(), periodo.getDays());
  //8 anos, 1 mês e 13 dias

````

## Não é o fim, tem muito mais novidades do Java 8

Somente reforçando meu disclaimer inicial, existem muitas outras novidades bem legais como "Accumulators and Adders" e "Nashorn" e recomendo a continuação dos estudos.

Espero que tenham gostado das pinceladas de Java 8.

Valeu!!!


## Referências

[http://blog.caelum.com.br/o-minimo-que-voce-deve-saber-de-java-8](http://blog.caelum.com.br/o-minimo-que-voce-deve-saber-de-java-8)

[https://www.thoughtworks.com/insights/blog/composition-vs-inheritance-how-choose](https://www.thoughtworks.com/insights/blog/composition-vs-inheritance-how-choose)

[http://pt.stackoverflow.com/questions/11162/o-que-s%C3%A3o-interfaces-funcionais](http://pt.stackoverflow.com/questions/11162/o-que-s%C3%A3o-interfaces-funcionais)

[http://pt.stackoverflow.com/questions/269/quais-as-vantagens-das-express%C3%B5es-lambda-presentes-no-java-8](http://pt.stackoverflow.com/questions/269/quais-as-vantagens-das-express%C3%B5es-lambda-presentes-no-java-8)

[http://www.infoq.com/br/articles/java8-desmistificando-lambdas](http://www.infoq.com/br/articles/java8-desmistificando-lambdas)

[http://www.drdobbs.com/jvm/lambda-expressions-in-java-8/240166764](http://www.drdobbs.com/jvm/lambda-expressions-in-java-8/240166764)

[http://effectivecraft.blogspot.com.br/2014/05/entendendo-classe-optional-no-java-8.html](http://effectivecraft.blogspot.com.br/2014/05/entendendo-classe-optional-no-java-8.html)

[http://pt.stackoverflow.com/tags/java-8/hot](http://pt.stackoverflow.com/tags/java-8/hot)

[http://effectivecraft.blogspot.com.br/2014/05/entendendo-classe-optional-no-java-8.html](http://effectivecraft.blogspot.com.br/2014/05/entendendo-classe-optional-no-java-8.html)

[http://blog.caelum.com.br/o-eterno-problema-de-calcular-a-diferenca-de-dias-entre-duas-datas-em-java](http://blog.caelum.com.br/o-eterno-problema-de-calcular-a-diferenca-de-dias-entre-duas-datas-em-java/)

[http://www.vidadeturista.com/artigos/gds-global-distribuition-system.html](http://www.vidadeturista.com/artigos/gds-global-distribuition-system.html)


