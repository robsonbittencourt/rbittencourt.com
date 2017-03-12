---
layout: post
title: "Faça o build do seu projeto com Travis CI"
modified:
categories: 
excerpt:
tags: [travis-ci, integração-continua, ferramentas]
image:
  feature:
date: 2017-03-13T12:00:00-02:00
---

Irei começar uma série de três posts mostrando algumas ferramentas gratuitas que temos disponíveis para projetos open-source. Este post irá mostrar a ferramenta de integração contínua Travis CI. Os próximos dois serão sobre qualidade de código e deploy de aplicações.

##Travis CI

Travis CI (daqui para frente chamado de apensas Travis) é um serviço muito interessante que possibilita executarmos tasks como build, testes e deploy nossas aplicações. Na verdade ele não se limita a isso, é possível criar scripts para fazer qualquer coisa que seria possível em um script bash.

Além disso ele se integra com o seu código no GitHub. Uma vez configurado, a cada commit um build é disparado pelo Travis de forma automática. Este processo de execução do build e testes a cada commit é conhecido como Integração Contínua. Não escrevi sobre ainda aqui no blog, mas você pode dar uma conferida [neste post da Caelum](http://blog.caelum.com.br/integracao-continua/).

Mas e quanto custa tudo isso? Nada para projetos open-source, totalmente de graça. Segundo o site: *"Testing your open source project is 10000% free. Seriously. Always. We like to think of it as our way of giving back to a community that gives us so much as well."* Se você ou sua empresa possui projetos fechados, eles oferecem serviços pagos e a privacidade necessária.

##Iniciando

Criar uma conta no Travic CI é muito fácil. Use seu próprio acesso do Github, dê as permissões necessárias e pronto, todos seus projetos já estarão aparecendo na listagem.

<figure>
	<img src="/images/2017-03-13-travis/1.png" alt="Lista de projetos no Travis">
</figure>

Caso algum não esteja é só clicar em Sync account que ele irá sincronizar novamente com o Github. Escolha um projeto que você deseja habilitar o build e ligue o botão correspondente.

Ao clicar na engrenagem do projeto, a tela de configurações é aberta. Aqui você pode realizar algumas configurações, como por exemplo, se todos os commits devem gerar um commit ou somente pull requests. Além disso é nesta tela que podemos criar variáveis de ambiente para utilizar nos builds. Entrarei em mais detalhes em um próximo post quando precisarmos. A princípio as configurações padrão são suficientes para prosseguirmos.

<figure>
	<img src="/images/2017-03-13-travis/2.png" alt="Tela de configurações">
</figure>

## O arquivo .travis.yml
Você pode estar se perguntando: como o Travis vai saber fazer o build do meu projeto e rodar meus testes? Simples, vamos dizer para ele fazer isso através de um arquivo de configuração que por padrão deve se chamar .travis.yml.
Neste arquivo vamos detalhar os passos que o Travis deve realizar para fazer o nosso build. *Eu sabia que tinha alguma pegadinha, agora vou ter que fazer várias configurações complicadas!*. 

Se enganou amiguinho. O Travis já utiliza várias configurações default, como comandos de build e testes para diversas linguagens. Vou mostrar um exemplo para realizar o build e execução dos testes de um projeto Java que utiliza Maven.

{% highlight yaml %}

language: 
  - java

{% endhighlight %}

Sim somente isso. Por padrão ele vai executar as tasks do Maven de build e testes. Claro que se o seu build tiver características diferentes outras configurações serão necessárias. Por exemplo, no caso anterior se quisermos definir uma versão específica do Java devemos adicionar a opção:

{% highlight yaml %}

jdk:
  - oraclejdk8

{% endhighlight %}

Porém notem que tudo é muito declarativo e simples. Existem diversas opções para várias linguagens diferentes. Você pode encontrá-las [aqui.](https://docs.travis-ci.com/user/customizing-the-build)

## Build

Agora que temos o arquivo basta fazer o commit dele no repositório e o Travis irá identificar a mudança e iniciar o build automaticamente. 

<figure>
	<img src="/images/2017-03-13-travis/3.png" alt="Tela mostrando a saída do build">
</figure>


Caso nosso build tivesse falhado, um e-mail teria sido enviado notificando. Um ponto interessante, é que o build sempre é executado em um ambiente isolado utilizando um container Docker. Dessa forma podemos ter a garantia que o ambiente de build sempre é idêntico e que não guarda nenhum tipo de estado. 

## Concluindo
Travis é uma ótima ferramenta para utilizarmos em nossos projetos open-source. Ele traz mais garantia e segurança para o nosso projeto, fornecendo feedback frequente sobre o estado do projeto. Esse feedback pode ser inclusive exibido para outras pessoas através de uma badge com o status do build. Se você clicar nela, já será exibido um link para que você cole no README do seu projeto, mostrando o status do seu projeto para o mundo. 

Na sequência estarei mostrando mais algumas ferramentas e voltaremos a incrementar nosso build com o Travis. O código completo pode ser encontrado [nesta demo no GitHub](https://github.com/robsonbittencourt/app-tools-examples)

Ficou com dúvidas? Escreva nos comentários. Até a próxima.

