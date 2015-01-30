---
layout: page
title: Como contribuir?
excerpt: "So Simple is a responsive Jekyll theme for your words and images."
modified: 2014-08-08T19:44:38.564948-04:00
---

Meu objetivo inicial ao criar este blog era compartilhar alguns assuntos que eu estivesse estudando no momento. Mas acabei amadurecendo um pouco a ideia, e resolvi torná-lo um blog colaborativo e de código aberto.

O que isso significa? Que qualquer pessoa que tiver interesse pode escrever um post aqui sobre algum assunto relacionado com desenvolvimento de software.

Além disso se você encontrar um erro em algum texto, clique no botão **Edite este post** na barra a esquerda, que redireciona para o Github onde é possível fazer a correção.

##Mas não sei sobre o que escrever

Os assuntos não precisam ser diretamente relacionados à software em si. Você manda bem em design ou técnicas gerenciais? Pode escrever sobre isso. Com certeza existe algo que você saiba e que possa compartilhar.

Você pode dar uma olhada [nesse repositório](https://github.com/LFeh/poste-mais), onde uma galera está com uma iniciativa muito legal de fazer um post por dia utilizando a hashtag **#1postperday**. Lá são sugeridos assuntos, e quem quiser escolhe um para escrever um post.


##Ahh mas isso que eu sei é muito simples, todo mundo sabe

Acredito que todo repasse de conhecimento é válido. O que é trivial para você pode ser a dúvida de muitos. Se seu post ajudar uma pessoa a resolver um problema, ou despertar o interesse por aprender algo novo o objetivo foi alcançado.


##Ok me convenceu. Como faço para escrever aqui?

Irá ser necessário um pouco de conhecimento em Jekyll, Git e Github flow. Não conhece nada sobre isso e não está afim de aprender agora. Sem problemas. Mande um email com seu texto para **robson.luizv@gmail.com** que eu edito e coloco no blog para você ;)

##Um pequeno passo a passo

Este mini tutorial é bem superficial. Pretendo escrever posts sobre estes assuntos em breve. Quando estiverem prontos irei arrumar aqui para que fique mais fácil o entendimento.

Se você seguir os links irá conseguir (e irá aprender algumas coisas legais), ou se já souber trabalhar com estas ferramentas será mais fácil ainda.

- Instalar o Ruby
- Instalar o bundle

{% highlight bash %}
gem install bundle
{% endhighlight %}

- Instalar o Git 
- Fazer um [fork](http://tableless.com.br/contribuindo-em-projetos-open-source-com-o-github/) do [projeto do blog](https://github.com/robsonbittencourt/rbittencourt.com/fork) no Github
- Instalar as dependências

{% highlight bash %}
#no diretório do projeto
bundle install
{% endhighlight %}

- Altere o arquivo /_data/authors.yml adicionando suas informações seguindo o exemplo
- Crie um novo post

{% highlight bash %}
#crie um novo arquivo na pasta _posts ou execute o comando:
octopress new post "Post Title"
{% endhighlight %}

- Escreva seu texto utilizando [markdown](https://help.github.com/articles/markdown-basics/)
- Faça um [build do projeto](http://jekyllrb.com/docs/usage/) e veja as alterações
- Faça um [commit e abra um pull request](http://tableless.com.br/contribuindo-em-projetos-open-source-com-o-github/)