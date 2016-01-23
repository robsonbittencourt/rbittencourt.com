---
layout: post
title: "Introdução à Generics"
modified:
categories: 
excerpt:
tags: [java, generics]
image:
  feature:
date: 2016-01-22T12:00:00-02:00
---

Há alguns dias fiz um curso da [Pluralsight sobre Generics](https://app.pluralsight.com/library/courses/java-generics/table-of-contents). O curso é muito bom, se você tem oportunidade de faze-lo aproveite. Decidi compilar algumas das lições que aprendi, aqui no blog. Vou dividir em dois posts para não ficar muito maçante. Este será uma introdução e no próximo irei apresentar algumas funcionalidades mais avançadas.

### Generics

*Generics* é uma funcionalidade introduzida na versão 5 do Java. Seu objetivo é fornecer ao desenvolvedor a capacidade de escrever código que seja reutilizável e ao mesmo tempo com a segurança da vericação de tipos em tempo de compilação. As APIs da própria linguagem utilizam largamente estas funcionalidades, como é o caso das conhecidas interfaces *List* e *Map* por exemplo.

### Por que utilizar?

Nas versões anteriores a 5 onde não havia a funcionalidade de *Generics*, códigos como abaixo eram comuns:

{% highlight java %}

String result = (String) list.get(0);
System.out.println(result);

{% endhighlight %}

Ao recuperar valores de uma lista, sempre era necessário realizar o *cast* para um determinado tipo. Isso devido ao fato que a interface *List* recebia um *Object* como parâmetro em seu método *add*. Como em Java todos os tipos extendem *Object*, qualquer tipo poderia ser inserido em uma lista. Em um primeiro momento nenhum problema. O código compila corretamente, mas se aumentarmos um pouco nosso cenário poderemos observar situações deste tipo:

{% highlight java %}

List list = new ArrayList();
list.add(1);
list.add("2");
list.add("3");

String result = (String) list.get(0);

System.out.println(result);

out - Exception in thread "main" java.lang.ClassCastException:
 java.lang.Integer cannot be cast to java.lang.String

{% endhighlight %}

Ou seja, não é possível ter segurança de qual tipo será retornado pela lista em tempo de compilação. Erros poderão ser percebidos somente em tempo de execução. Com o uso de *Generics* podemos resolver este problema. 

### Classes e Interfaces

A interface *List* se tornou genérica apartir do Java 5, e o problema mostrado anteriormente pode ser evitado. Mas vou criar minha própria (e simples) classe de lista para exemplificar melhor a explicação.

{% highlight java %}

public class MyAwesomeList<T> {
	
   private Object[] elements = new Object[0];

   public T get(int index) {
      return (T) elements[index];
   }

   public void add(T element) {
      int position = elements.length + 1;
	
      elements = Arrays.copyOf(elements, position);
	
      elements[position] = element;
   }
}

{% endhighlight %}

Uma classe é considerada genérica, quando possui ao lado de seu nome um identificador como este: **\<T\>**. No exemplo a letra T representa um tipo que será inserido ao instanciar a classe *MyAwesomeList*. A letra T é apenas uma convenção para *type*, qualquer identificador pode ser utilizado.

Ao longo do código da classe este tipo estará disponível e poderá ser utilizado na criação de variáveis, retorno de métodos (get), ou ainda em parâmetro de métodos (add). Isso faz com que minha classe possa ser utilizada com qualquer tipo, mas somente um tipo nos elementos, uma vez que a lista seja instânciada. Alguns exemplos:

{% highlight java %}

MyAwesomeList<String> list = new MyAwesomeList<>();

list.add(1);
list.add("2");

//Não compila. 
//The method add(String) in the type MyAwesomeList<String>
//is not applicable for the arguments (int)

{% endhighlight %}

{% highlight java %}

MyAwesomeList<String> list = new MyAwesomeList<>();
list.add("1");
list.add("2");

//Não há necessidade de cast, pois o compilador sabe que é uma String
String result = list.get(0);

System.out.println(result);

{% endhighlight %}

### Generics em Métodos

Não são somente as classes e interfaces que possuem a flexibilidade dos *Generics*, também podemos criar métodos genéricos. A sintaxe é um pouco diferente:

{% highlight java %}

public <T> T min(List<T> values, Comparator<T> comparator) {
   if (values.isEmpty()) {
      throw new IllegalArgumentException("List is empty, cannot find minimum");
   }
	
   T lowestElement = values.get(0);
	
   for(int i = 1; i < values.size(); i++) {
      T element = values.get(i);
		
      if (comparator.compare(element, lowestElement) < 0) {
         lowestElement = element;
      }
   }
	
   return lowestElement;
}

{% endhighlight %}

Como pode-se perceber a sintaxe é um pouco diferente. Assim como nas classes genéricas, temos que ter a declaração do tipo. Ela fica entre os modificadores de acesso do método e seu tipo de retorno. O tipo pode ser usado como argumento e nas variáveis internas do método, assim como no retorno.

O retorno no exemplo pode causar um pouco de confusão, mas para deixar claro o primeiro T (**\<T\>**) está declarando o tipo genérico do método, enquanto o segundo é seu retorno. Ou seja estamos dizendo que o mesmo tipo que passarmos para invocar o método, será o tipo que irá retornar.

É importante ressaltar que quando utilizado em métodos, o tipo genérico pertence ao escopo daquele método. Ou seja, não é possível utilizar o tipo em outros métodos. Se a classe também é genérica, e possui um identificador de tipo igual ao do método, o do método vai sobreescrever o da classe.

No próximo post irei apresentar outros assuntos importantes sobre *Generics*: *Wildcards*, *RawTypes* e *Erasure*. 
