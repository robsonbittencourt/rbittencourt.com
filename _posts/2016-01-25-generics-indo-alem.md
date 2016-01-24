---
layout: post
title: "Generics indo além - Wildcards, Raw Types e Erasure"
modified:
categories: 
excerpt:
tags: [java, generics]
image:
  feature:
date: 2016-01-23T00:00:00-02:00
---

No [meu último post](http://rbittencourt.com/introducao-a-generics/) apresentei alguns conceitos básicos sobre *Generics*. Vale reforçar que o conteúdo que será apresentado aqui, foi visto no curso da [Pluralsight sobre Generics](https://app.pluralsight.com/library/courses/java-generics/table-of-contents). Não, não estou fazendo propaganda, é que o curso é bom mesmo, recomendo :)

### Wildcards

Antes de explicar o que são *Wildcards* é necessário mostrar alguns detalhes sobre *arrays* em Java. 

#### Variância

Variância é um conceito que se relaciona a como a herança entre classes se comporta na declaração de variáveis. Não vou me aprofundar neste assunto, mas [este link](http://sergiotaborda.javabuilding.com/2015/04/variancia/) traz uma ótima explicação sobre. 

Por hora precisamos saber que *arrays* em Java são covariantes, vamos entender o porquê. 

{% highlight java %}

public class Animal {}

public class Cat extends Animal {}

public class Dog extends Animal {}

Cat[] cats = new Cat[2];
		
Animal[] animals = cats;

{% endhighlight %}

Como pode-se perceber, é possível declarar um *array* com o tipo da classe filha e atribuir à um *array* do tipo da classe pai. Isso é conhecido como covariância, e traz alguns problemas como este:

{% highlight java %}

Cat[] cats = new Cat[2];
		
Animal[] animals = cats;
		
animals[0] = new Dog(); //compila pois Dog é um Animal
		
// out - Exception in thread "main" java.lang.ArrayStoreException:
// com.generics.lab.Dog at 
// com.generics.lab.GenericTest.main(GenericTest.java:12)

{% endhighlight %}

O *array* é um tipo primitivo especial, não possue uma classe nem implementa uma interface, mas possui uma estrutura, e esta sabe o tipo que deve ser possível armazenar, por isso a exceção no exemplo foi lançada. Isso nos leva a perceber que *arrays* não são muito seguros, por isso é recomendado o uso de listas. 

As listas ao contrário dos *arrays* são invariantes e só poder ser "asignadas" a variáveis com o mesmo tipo genérico.

{% highlight java %}

List<Dog> dogs = new ArrayList<>();
List<Animal> animals = dogs; // não compila apesar de Dog ser um Animal
		
List<Dog> otherDogs = dogs; // compila

{% endhighlight %}

Isso torna o uso de listas mais seguro que *arrays*. Caso as listas não fossem invariantes poderiamos fazer algo desse tipo:

{% highlight java %}

List<Dog> dogs = new ArrayList<>();
List<Animal> animals = dogs; // não compila é somente um exemplo
		
animals.add(new Cat());
Dog dog = dogs.get(0); // nada de bom iria acontecer aqui

{% endhighlight %}

#### Upper Bounded Wildcards

Mas e no caso de precisarmos de uma lista com elementos de vários tipos os quais temos domínio. Por exemplo, queremos criar um método que retorne a espécie dos animais de uma lista. Como observamos, se utilizarmos o tipo da classe pai, só poderemos trabalhar com listas deste tipo, o que nos leva a ter que criar uma método para cada classe filha.

{% highlight java %}

// considere que getSpecies é um método da classe Animal

public void printAllDogSpecies(List<Dog> dogs) {
   for (Dog dog : dogs) {
      System.out.println(dog.getSpecies());
   }
}

public void printAllCatSpecies(List<Cat> cats) {
   for (Cat cat : cats) {
      System.out.println(cat.getSpecies());
   }
}

{% endhighlight %}

Cheira mal certo? Se houvessem cem implementações de Animal, teríamos este código replicado uma centena de vezes. Felizmente temos como resolver isso utilizando Wildcards. Veja outro exemplo:

{% highlight java %}

public void printAllSpecies(List<{%raw%}?{%endraw%} extends Animal> animals) {
   for (Animal animal : animals) {
      System.out.println(animal.getSpecies());
   }
}

{% endhighlight %}

Pronto agora temos um método que pode ser utilizado para qualquer classe filha de Animal. A sintaxe que utilizamos diz que pode-se passar no parâmetro qualquer lista onde o tipo genérico seja uma classe que extenda Animal.

Você pode estar se perguntando, qual é a diferença entre utilizar o "? extends" ou o "T extends". O "?" é utilizado onde já é esperado um tipo genérico, por exemplo na interface List. Quando uma classe genérica é criada não podemos utilizar o Wildcard (?) deve-se utilizar o identificador de tipo genérico. Além disso o Wildcard é util como "sintax sugar" na assinatura de métodos.

{% highlight java %}

public static <T extends Animal> void printAllSpecies(List<T> animals) {
   for (Animal animal : animals) {
      System.out.println(animal.getSpecies());
   }
}

// como não utilizamos o T no corpo do método podemos utilizar o Wildcard

public void printAllSpecies(List<{%raw%}?{%endraw%} extends Animal> animals) {
   for (Animal animal : animals) {
      System.out.println(animal.getSpecies());
   }
}

{% endhighlight %}