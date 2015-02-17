---
layout: post
title: "Funções JavaScript - Como Funciona O Escopo"
modified:
categories: 
excerpt:
tags: []
image:
  feature:
date: 2015-02-17T16:53:21-02:00
---

#Funções

Estou lendo o livro Segredos do Ninja JavaScript e resolvi escrever um pouco sobre o tópico que estou estudando: funções. 

Funções podem ser consideradas a base da linguagem JavaScript. Por ser uma linguagem de aspecto funcional, para fazermos o bom uso de seus recursos, é imprecindível que tenhamos um bom conhecimento sobre a mecânica por trás das funções. 

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

Mas se fizermos este teste no browser poderemos verificar que o valor de saída será a string 'teste'. Como foi dito anteriormente, o JavaScript não delimita o escopo por blocos mas sim baseado na função. 

As seguintes regras são utilizadas para determinar o escopo:

- Declarações de variáveis estão no escopo do ponto onde são declaradas até o final da função em que se encontram, independente do aninhamento dos blocos.

- Funções nomeadas fazem parte do escopo dentro de toda função onde são declaradas, independente do aninhamento dos blocos.

Abaixo podemos verificar estes comportamentos com alguns exemplos:

###Escopo de Variáveis

{% highlight javaScript %}
function escopoDeVariaveis() {
  if(typeof dia === 'number') {
    console.log("ANTES da declaração a variável dia ESTÁ no escopo.");
  } else {
    console.log("ANTES da declaração a variável dia NÃO ESTÁ no escopo.");
  }

  var dia = 1;

  if(typeof dia === 'number') {
    console.log("DEPOIS da declaração a variável dia ESTÁ no escopo.");
  } else {
    console.log("DEPOIS da declaração a variável dia NÃO ESTÁ no escopo.");
  }		  
  
}

escopoDeVariaveis();

{% endhighlight %}

###Escopo de Funções

{% highlight javaScript %}
function escopoDeFuncoes() {
  if (typeof helloWorld === 'function') {
    console.log("ANTES da declaração a função helloWorld ESTÁ no escopo.");
  } else {
    console.log("ANTES da declaração a função helloWorld NÃO ESTÁ no escopo.");
  }

  function helloWorld() {	console.log('Hello World'); }

  if (typeof helloWorld === 'function') {
    console.log("DEPOIS da declaração a função helloWorld ESTÁ no escopo.");
  } else {
    console.log("DEPOIS da declaração a função helloWorld NÃO ESTÁ no escopo.");
  }
}

escopoDeFuncoes();

{% endhighlight %}

Bom por hoje é isso. Em breve irei escrever mais sobre esse assunto. Dúvidas e sugestões são bem vindas.
