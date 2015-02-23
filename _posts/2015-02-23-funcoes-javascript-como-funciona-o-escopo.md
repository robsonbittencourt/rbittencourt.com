---
layout: post
title: "Funções JavaScript - Como Funciona O Escopo"
modified:
categories: 
excerpt:
tags: []
image:
  feature:
date: 2015-02-23T12:20:00-02:00
---

#Funções
Estou lendo o livro Segredos do [Ninja JavaScript](http://novatec.com.br/livros/ninja-javascript/), o qual recomendo fortemente a leitura, e resolvi escrever um pouco sobre o tópico que estou estudando: funções. 

Funções podem ser consideradas a base da linguagem JavaScript. Por ser uma linguagem de aspecto funcional, para fazermos o bom uso de seus recursos, é imprescindível que tenhamos um bom conhecimento sobre a mecânica por trás das funções. 

#Escopo

Apesar de ser baseada na linguagem C, JavaScript não delimita o escopo através de blocos (que iniciam e terminam com {}).

Para quem está acostumado com linguagens que delimitam o escopo por blocos, como o Java, irá esperar que o valor de saída seja undefined.

{% highlight javaScript %}
 function escopoTeste() { 
   if(true) {
     var x = 'teste';
   }
   console.log(x);
 }

 escopoTeste();

{% endhighlight %}

Mas se fizermos este teste no browser poderemos verificar que o valor de saída será a string *teste*. Como foi dito anteriormente, o JavaScript não delimita o escopo por blocos mas sim baseado na função. 

As seguintes regras são utilizadas para determinar o escopo:

- Declarações de variáveis estão no escopo do ponto onde são declaradas até o final da função em que se encontram, independente do alinhamento dos blocos.

- Funções nomeadas fazem parte do escopo dentro de toda função onde são declaradas, independente do alinhamento dos blocos.

Abaixo podemos verificar estes comportamentos com alguns exemplos. Execute os códigos abaixo e verifique as saídas.

###Escopo de Variáveis

{% highlight javaScript %}
function escopoDeVariaveis() {
  if(typeof dia === 'number') {
    console.log("ANTES da declaração, a variável dia ESTÁ no escopo.");
  } else {
    console.log("ANTES da declaração, a variável dia NÃO ESTÁ no escopo.");
  }

  var dia = 1;

  if(typeof dia === 'number') {
    console.log("APÓS a declaração, a variável dia ESTÁ no escopo.");
  } else {
    console.log("APÓS a declaração, a variável dia NÃO ESTÁ no escopo.");
  }		  
  
}

escopoDeVariaveis();

{% endhighlight %}

###Escopo de Funções

{% highlight javaScript %}
function escopoDeFuncoes() {
  if (typeof helloWorld === 'function') {
    console.log("ANTES da declaração, a função helloWorld ESTÁ no escopo.");
  } else {
    console.log("ANTES da declaração, a função helloWorld NÃO ESTÁ no escopo.");
  }

  function helloWorld() { console.log('Hello World'); }

  if (typeof helloWorld === 'function') {
    console.log("APÓS a declaração, a função helloWorld ESTÁ no escopo.");
  } else {
    console.log("APÓS a declaração, a função helloWorld NÃO ESTÁ no escopo.");
  }
}

escopoDeFuncoes();

{% endhighlight %}

###Global x Local
Sempre que declaramos uma variável ou função fora de uma função, ela acaba pertencendo ao escopo global. Como seu nome sugere, tudo que pertence ao escopo global, é acessível em qualquer parte daquele código. 

Porém quando criamos variáveis ou funções dentro de outra função, as regras mostradas anteriormente são obedecidas, e dizemos que o escopo é local.

Quando existe um conflito de nomes entre variáveis globais e locais, o JavaScript irá a utilizar a de menor escopo por padrão.

Observe o exemplo abaixo:

{% highlight javaScript %}
var a = 5

function escopoDeVariaveis() {
  var a = 10;
  console.log(a);
}

escopoDeVariaveis();

{% endhighlight %}

Podemos verificar que o valor impresso foi o 10, pois a variável mais próxima deste escopo é a de valor 10.

###Um pouco mais sobre escopo de variáveis

Um comportamento que ainda devemos observar, é que o JavaScript coloca as declarações de variáveis sempre no topo das funções, independentemente de sua localização.

Mas note que somente a declaração é colocada no topo. A atribuição do valor ocorre no ponto em que está localizada no código.

{% highlight javaScript %}
var a = 5

function escopoDeVariaveis() {
  console.log(a);
  
  var a = 10;
  
  console.log(a);
}

escopoDeVariaveis();

{% endhighlight %}

Como você pôde perceber o valor 5 não foi impresso. Isso acontece porque dentro do escopo da função a variável _a_ existe desde o começo, apesar de não possuir valor.

O código acima seria o equivalente a isso:

{% highlight javaScript %}
var a = 5

function escopoDeVariaveis() {
  var a;
  console.log(a);
  
  a = 10;
  
  console.log(a);
}

escopoDeVariaveis();

{% endhighlight %}

Bom por hoje é isso. Em breve irei escrever mais sobre esse assunto. Dúvidas e sugestões são bem vindas.