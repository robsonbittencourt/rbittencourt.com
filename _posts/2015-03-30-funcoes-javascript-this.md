---
layout: post
title: "Funções JavaScript - This"
modified:
categories: 
excerpt:
tags: [javascript, funcoes]
image:
  feature:
date: 2015-03-30T12:00:00-03:00
---

### O tal This
Se você é um desenvolvedor de linguagens orientadas a objeto como Java e C#, já deve estar acostumado com o conceito de *this*. Nestas linguagens, *this* é o contexto de execução. Por exemplo quando executamos um método em Java, podemos utilizar referências a *this*, que equivalem ao objeto que esta realizando a invocação. Isso permite que métodos iguais retornem informações diferentes de acordo com quem faz a chamada.

Chegando no mundo JavaScript, vamos perceber que o *this* é tratado de forma totalmente distinta. Aqui *this* não é determinado por quem chama a função e sim pela forma como ela é invocada. Existem quatro maneiras de invocar uma função, e vamos conhece-las agora: *Invocação como Função*, *Invocação como Método*, *Invocação como Construtor* e *Invocação por apply/call*.

### Invocação como Função
Como assim invocação como função? É óbvio que uma função é invocada como uma função, qualquer um deduziria isso. Na verdade dizemos que uma função é invocada como função quando ela não é invocada de nenhuma das outras três maneiras: método, construtor, apply/call.

Este tipo de invocação ocorre quando a função é invocada utilizando o operador () e a expressão para qual o operador é aplicado não referência a função como uma propriedade de um objeto. Quando utilizamos este tipo de invocação o contexto da função é o contexto global (window).

{% highlight javaScript %}

function printBalance() {return this;}

if(printBalance() === window) {
  console.log('Window is the context!');
};

var depositValue = function(){};
depositValue();

{% endhighlight %}

### Invocação como Método
Quando uma função é atribuída a uma propriedade de um objeto, e a invocação ocorre através desta propriedade, então a função é invocada como um método deste objeto. Aqui as coisas começam a ficar interessantes.

Quando invocamos uma função como método, o contexto da função é o objeto ao qual o método pertence. Assim como em outras linguagens orientadas a objetos, o objeto utilizado para invocar a função é passado como referência implícita para a mesma, possibilitando a sua utilização dentro da função.

{% highlight javaScript %}

var account = {
  clientName: 'Mike'
};

account.getClientName = function(){
  console.log(this.clientName);
};

account.getClientName();

{% endhighlight %}

### Invocação como Construtor
Invocação como construtor é uma forma de se criar objetos. Na verdade todas as funções podem ser invocadas desta forma, basta chamá-las utilizando a palavra-chave *new*. 


{% highlight javaScript %}

new printBalance();

{% endhighlight %}

Quando utilizamos a palavra-chave *new* três coisas acontecem:

- Um novo objeto vazio é criado.

- Esse objeto é transmitido ao construtor como parâmetro *this* e torna-se o contexto da função do construtor.

- Quando não há um valor de retorno explícito o novo objeto é retornado como valor do construtor.

Como você deve ter percebido, isso faz com que nem todas funções sejam interessantes de serem invocadas utilizando *new*. Os construtores na verdade têm como objetivo criar um objeto, configurá-lo e retorná-lo. Qualquer coisa diferente disso não irá trazer resultados muito bons. Um bom exemplo da utilização dos construtores para criação de objetos:

{% highlight javaScript %}

function Account() {
  this.balance = 100;
}

var account1 = new Account();
console.log(account1.balance);

{% endhighlight %}

Por ter sua função bem definida, existem padrões utilizados na nomenclatura dos construtores. Enquanto os métodos são nomeados com verbos e iniciam com a primeira letra minúsculas, os construtores recebem no nome um substantivo que indica o objeto que será construído e iniciam com uma letra maiúscula.

### Invocação por Apply/Call
Até agora vimos diferentes formas de invocar funções, onde *this* é determinado de acordo com a forma escolhida. Mas e se quiséssemos definir *this* da nossa forma? Sim podemos fazer isso utilizando um dos dois métodos que todas funções possuem: *apply* e *call*.

Quando utilizamos estes métodos podemos definir quem será o contexto da função e ainda passar uma lista de argumentos. A diferença entre os dois é que enquanto *apply* recebe um array de argumentos, *call* recebe os mesmos diretamente na lista de argumentos. 

{% highlight javaScript %}

function deposit() {
  var balance = 0;
  for (var n = 0; n < arguments.length; n++) {
    balance += arguments[n];
  }
  this.balance = balance;
}

var account1 = {};
var account2 = {};

deposit.apply(account1, [1,2,3,4]);
deposit.call(account2, 1,2,3,4);

console.log(account1.balance);
console.log(account2.balance);

{% endhighlight %}

Os dois métodos possuem o mesmo comportamento, você pode escolher o que mais lhe fizer sentido no momento. Se possuir um array, ou os argumentos são uma coleção utilize *apply*, caso contrário utilize *call*.

### Recapitulando
Como podemos observar, JavaScript fornece diversas formas de invocarmos funções, e estas formas definem quem será o contexto da função.

- Quando invocada como uma função simples, o contexto é o objeto global (window).

- Quando invocada como um método, o contexto é o objeto ao qual o método pertence.

- Quando invocada como construtor, é um objeto recém alocado.

- E por fim quando invocada pelos métodos *apply()* e *call()*, o contexto pode ser o que quisermos.
