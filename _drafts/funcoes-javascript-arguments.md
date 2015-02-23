---
layout: post
title: "Funções JavaScript - Arguments"
modified:
categories: 
excerpt:
tags: []
image:
  feature:
date: 2015-02-17T18:49:52-02:00
---

###Assinaturas de métodos

Quando estamos trabalhando com linguagens como Java ou C#, estamos acostumados a ter que obedecer as assinaturas dos métodos. Estas assinaturas nos informam quais parâmetros determinado método deve receber para executar corretamente.

Quando não informamos algum parâmetro, ou ainda, passamos parâmetros além dos que estão na assinatura, o compilador nos informa que temos um problema e este método não irá compilar. Veja este exemplo em Java:

{% highlight java %}
public void imprimePalavra(String palavra) {
  System.out.println(palavra);
}

imprimePalavra("teste"); //OK compila sem problemas

imprimePalavra("teste", "outroTeste"); //O método não é aplicável para os argumentos String, String

imprimePalavra(); //O método não é aplicável para os argumentos()

{% endhighlight %}

###Sintaxe de Funções
Porém, a linguagem JavaScript trata este assunto de maneira totalmente distinta. Antes de mostrar as diferenças, vamos ver qual é a sintaxe de declaração de uma função JavaScript.

{% highlight javaScript %}
function myFunction(value1, value2) {
  return value1 + value2;
}

{% endhighlight %}

- A palavra-chave function.

- Um nome que é opcional.

- Uma lista separada por vírgulas de nomes de parâmetros entre parênteses. A lista pode ser vazia, mas os parênteses sempre devem estar presentes.

- O corpo da função, com as instruções entre chaves. O corpo pode estar vazio, mas as chaves sempre devem estar presentes.

###Argumentos Variáveis
Como dito anteriormente, o JavaScript trata de uma forma diferente os argumentos de uma função. Podemos invocar uma função passando qualquer número de parâmetros, não importando a assinatura da função, e nenhum erro será gerado.

As seguintes regras são seguidas ao invocar uma função:

- Invocar a função passando o mesmo número de argumentos que a assinatura declara: o primeiro argumento será atribuído ao primeiro parâmetro, o segundo ao segundo e assim sucessivamente.

- Invocar a função com menos parâmetros do que os informados na assinatura: a regra acima será seguida com os parâmetros informados, e os que não forem informados terão seus valores *undefined* dentro da função.

- Invocar a função com parâmetros além dos informados na assinatura: Os atributos serão atribuídos aos parâmetros seguindo a primeira regra. Os atributos em excesso, são ignorados e não tem seus valores atribuídos a nenhum parâmetro.

###Arguments
No tópico anterior você deve ter se perguntado: Porque é possível passar argumentos adicionais se eles não são atribuídos a nenhum parâmetro? A resposta para isso é o parâmetro _arguments_.

Toda vez que invocamos uma função, o JavaScript passa de forma implícita o argumento *arguments*. Este argumento representa uma lista que contém todos argumentos que foram passados para a função.

O exemplo abaixo deixará isso mais claro:

{% highlight javaScript %}
function imprimeValores() {
  console.log(arguments[0]);
}

imprimeValores('hello');

{% endhighlight %}

Podemos invocar a função passando um parâmetro apesar de sua assinatura não requerer nenhum. E ainda conseguimos utilizar seu valor dentro da função.

Devemos tomar alguns cuidados ao utilizar *arguments*. Apesar de possuir um atributo *length*, ser possível recuperar seus valores com sintaxe de um array, e até iterá-lo em um laço for, *arguments* não é um array. Tentar utilizar os métodos de array resultará em erros.

Por enquanto ficamos por aqui. No próximo post falarei sobre o outro parâmetro que é passado de forma implícita em todas funções, o *this*. 