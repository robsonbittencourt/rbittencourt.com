---
layout: post
title: "Generics - Por baixo dos panos"
modified:
categories: 
excerpt:
tags: [java, generics]
image:
  feature:
date: 2016-05-20T12:00:00-02:00
---

Demorei para escrever este último post da série sobre *Generics*, mas saiu. No [post anterior](http://rbittencourt.com/generics-indo-alem-wildcards/) falei sobre *Wildcards* e como eles possibilitam a escrita de códigos mais flexíveis. 

Neste post, iremos descer um pouco o nível e entender alguns aspectos sobre como o mecanismo de *Generics* funciona por baixo dos panos.

### Raw Types

As versões anteriores a 1.5 do Java, não possuíam os recursos de *Generics* que vimos nos posts anteriores. Era comum vermos código como o abaixo nestas versões:

{% highlight java %}

List list = new ArrayList();
list.add("t");
list.add("d");
list.add("f");

String result = (String) list.get(0);

System.out.println(result);

{% endhighlight %}

A plataforma Java é conhecida por sua estabilidade, e largamente utilizada no ramo empresarial. Portanto com o lançamento da *feature* de tipos genéricos, classes que ganharam tipos genéricos deveriam continuar compilando para oferecer esta retrocompatibilidade.

O código abaixo, apesar de não possuir um tipo genérico, compila em versões posteriores a 1.4, porém com um aviso. 

{% highlight java %}

List list = new ArrayList<>();
//List is a raw type. References to generic type List<E> should be parameterized

{% endhighlight %}

Isso possibilita que códigos legados possam migrar para versões recentes do Java. Porém o uso de *Raw Types* é desencorajado pois abre espaço para problemas como este:

{% highlight java %}

List listWithoutType = new ArrayList<>();
listWithoutType.add(1);

List<String> stringList = listWithoutType;

System.out.println(stringList.get(0));

{% endhighlight %}

Vamos entender mais para frente o porque isto ser permitido. Porém com o devido cuidado isso pode ser um ponto positivo, pois permite iteração entre código pré e pós *Generics*.

{% highlight java %}

public static void main(String[] args) {
   List<Integer> numbers = new ArrayList<>();
   numbers.add(1);
   numbers.add(2);
   numbers.add(3);

   int result = sumNumbers(numbers);

   System.out.println(result);
}

public static int sumNumbers(List numbers) {
   int sum = 0;
   Iterator iterator = numbers.iterator();

   while (iterator.hasNext()) {
      sum += (int) iterator.next();
   }

   return sum;
}

{% endhighlight %}

Portanto os *Raw Types* foram importantes para que a compatibilidade entre versões fosse mantida. Mas como o compilador trata essas ocasiões? Vamos para o último e talvez mais complexo tópico desta série: ***Erasure***.

### Erasure

Vou contar uma coisa que talvez lhe pareça estranho a princípio.

***Em Java tipos genéricos não existem em tempo de execução.***

Após compilado, esses tipos são tratados de forma diferente de como os escrevemos. Seguem alguns exemplos do que acontece com os tipos após a compilação.

{% highlight java %}

Compilação: List<String>, List<Integer>, List<List<Integer>> -> Execução: List

Compilação: List<String>[] -> Execução: List

Compilação: <T> -> Execução: Object

Compilação: <T extends Foo> -> Foo

{% endhighlight %}


Está duvidando? Vamos fazer uma verificação mais profunda. Observe a seguinte classe: 

{% highlight java %}

public class Erasure<T, B extends Comparable<B>> {

   public void unbounded(T param) {}

   public void lists(List<String> param, List<List<T>> param2) {}

   public void bounded(B param) {}

}

{% endhighlight %}

Agora utilizando o comando *javap* vamos decompilar o .class gerado.

<figure>
	<img src="/images/2016-05-23-generics/1.png" alt="console aws">
</figure>

Como podemos observar, no campo *descriptor*, os verdadeiros tipos que são interpretados pela JVM, não são genéricos. Os tipos genéricos são removidos, os *unbounded parameters* se tornam *Object*, e os *bounded parameters* se tornam o tipo que estendem, nesse caso *Comparable*. Além disso podemos observar no método *lists*, que um *checkcast* é feito, para garantir que o parâmetro passado é uma *String*. Esse comportamento traz algumas implicações, e é importante conhece-las.

####Sobrecarga de métodos

Por conta do mecanismo de *Erasure*, métodos sobrecarregados, onde a única diferença é um tipo genérico não são válidos. Isso se deve ao fato que o compilador não saberia qual método chamar após a remoção do tipo genérico.

{% highlight java %}

public void doSomething(List<String> params) {}

public void doSomething(List<Integer> params) {}

//Erasure of method doSomething(List<String>) is the same as another method in type TestClass

{% endhighlight %}

#### InstanceOf

Outra consequência do uso de *Erasure* é que não é possível utilizar *instanceOf* com tipos genéricos, pois como vimos eles não existem em tempo de execução.

{% highlight java %}

public boolean equals(Object o) {
   // Cannot perform instanceof check against parameterized type
   // TestClass<T>. Use the form TestClass<?> instead since further generic
   // type information will be erased at runtime

   if (o instanceof TestClass<T>) {
      return true;
   }

   return false;
}

{% endhighlight %}

#### Exceptions

Também não é possível criar exceções genéricas, pois como não podemos utilizar *instanceOf* o *catch* do bloco *try/catch* não conseguiria identificar o tipo da exceção. Em virtude disso o Java nem permite a criação desse tipo de *exception*.

{% highlight java %}

// The generic class UncompilableException<T> may not subclass
// java.lang.Throwable
public class UncompilableException<T> extends Exception {

   public void doSomething() {
      try {
         throw new UncompilableException();
	  } catch (UncompilableException e) {
	  	 e.printStackTrace();
	  }
   }

}

{% endhighlight %}

### Conclusão

Chegamos ao fim desta série sobre *Generics*. Espero que tenha sido útil, e esclarecido suas dúvidas sobre esta grande funcionalidade Java. Se ainda surgirem dúvidas fique a vontade para deixar um comentário.