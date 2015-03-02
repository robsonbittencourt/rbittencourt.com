---
layout: post
title: "Hospendando seu Blog Na AWS por Centavos"
modified:
categories: 
excerpt:
tags: []
image:
  feature:
date: 2015-02-12T09:00:00-02:00
---

###O Primeiro Post

A ideia deste primeiro post é começar a demonstrar como montei este blog, para que possa servir como um tutorial para quem quiser começar o seu próprio.

O processo pode ser dividido em 3 partes: criação, versionamento, e hospedagem. Creio que para os dois primeiros já existe bastante material, então resolvi escrever detalhadamente somente sobre a hospedagem que foi a parte que não encontrei muita coisa em português. 

###Criação 

Para criação do blog utilizei o gerador de sites estáticos [Jekyll](http://jekyllrb.com/). O Jekyll é uma ferramenta que facilita a construção de sites estáticos como por exemplo blog. Seu objetivo é fazer com que o usuário se preocupe quase que exclusivamente com o texto, uma vez que ele já tenha criado seu template.

O Willian Justen escreveu um [ótimo post](http://willianjusten.com.br/perguntas-e-respostas-jekyll/) demonstrando desde a instalação até a criação das páginas utilizando Jekyll.

###Versionamento 

É muito importante que tenhamos um controle sobre as modificações do blog. Para isso utilizei Git + Github para fazer o versionamento dos arquivos. Além disso utilizar o Github possibilita que outras pessoas possam contribuir com o blog.

O Fernando Daciuk tem [diversos posts](http://blog.da2k.com.br/2014/01/19/manter-repositorio-github-forkado-sincronizado-com-o-original/) sobre Git e Github no seu blog. 


###Hospedagem

Para hospedagem vou demonstrar alguns serviços da AWS (Amazon Web Services). A AWS oferece uma enorme gama de serviços utilizando o paradigma de Cloud Computing. A principal característica destes serviços é o modelo pay-as-you-go, onde você só é cobrado pelo tempo que utilizar o serviço, sem contratos.

Irá ser necessário criar uma conta na AWS. Isso pode ser feito [aqui](http://aws.amazon.com/pt/). Ao criar a conta você poderá utilizar [diversos serviços gratuitamente durante 1 ano](http://aws.amazon.com/pt/free/). Com exceção do Route53, os demais serviços apresentados a seguir se enquadram no nível gratuito.

Os serviços que utilizei para hospedar o blog foram: 

[S3 Simple Storage Service](http://aws.amazon.com/pt/s3/) - Armazenamento de arquivos estáticos


[CloudFront](http://aws.amazon.com/pt/cloudfront/) - Serviço de entrega de conteúdo (CDN)


[Route53](http://aws.amazon.com/pt/route53/) - Domínio e DNS


###Armazenando Seus Arquivos No S3
O Simple Storage Service, mais conhecido por S3 é o serviço de armazenamento de arquivos da AWS. É aqui que vamos colocar o conteúdo do blog. Qualquer questão relativa a backup dos arquivos é providenciada pela AWS. Você só precisa se preocupar em fazer o upload dos arquivos. 

No console principal, selecione o S3:

<figure>
	<img src="/images/2015-02-10-hospedando-seu-blog-na-aws-por-centavos/1.png" alt="console aws">
</figure>

Clique no botão *Create Bucket*. O bucket é o lugar onde irão ficar armazenados seus objetos no S3. Após, preencha o nome do bucket com o domínio do seu blog. Escolha a região US Standard pois é a mais barata e clique em *Create*.

<figure>
	<img src="/images/2015-02-10-hospedando-seu-blog-na-aws-por-centavos/2.png" alt="Criando um bucket">
</figure>

Faça upload do conteúdo do seu blog dentro do bucket. Se você já leu o post sobre Jekyll faça upload do conteúdo da pasta _site.

<figure>
	<img src="/images/2015-02-10-hospedando-seu-blog-na-aws-por-centavos/3.png" alt="Upload de arquivos no bucket">
</figure>

Selecione o bucket criado e vá na aba *Properties* e na opção *Permissions*. Clique no botão *Edit bucket policy*

<figure>
	<img src="/images/2015-02-10-hospedando-seu-blog-na-aws-por-centavos/4.png" alt="Permissões do bucket">
</figure>

Adicione o seguinte código:

{% highlight json %}
{
	"Version": "2008-10-17",
	"Statement": [
		{
			"Sid": "Allow Public Access to All Objects",
			"Effect": "Allow",
			"Principal": {
				"AWS": "*"
			},
			"Action": "s3:GetObject",
			"Resource": "arn:aws:s3:::www.meublog.com/*"
		}
	]
}
{% endhighlight %}

Agora no menu *Static Web Hosting* selecione a opção *Enable website hosting* e informe sua página inicial e a página de erro.

<figure>
	<img src="/images/2015-02-10-hospedando-seu-blog-na-aws-por-centavos/5.png" alt="Ativando página estática no bucket">
</figure>

Ao ativar esta opção o S3 já lhe fornece uma URL para o seu bucket. Se você acessá-la já deverá visualizar a página inicial do seu blog.

<figure>
	<img src="/images/2015-02-10-hospedando-seu-blog-na-aws-por-centavos/6.png" alt="URL Bucket S3">
</figure>

Mas não queremos está URL feia certo? Vamos trabalhar nisso então.

###Entregando o Conteúdo Do Seu Blog Rapidamente Com CloudFront

Ao criarmos o bucket no S3 selecionamos a região US Standard. Isso significa que nossos arquivos estão sendo hospedados nos Estados Unidos. Sempre que um leitor de seu blog acessá-lo de fora dos EUA ele vai ter um tempo de carregamento da página maior, em virtude da latência. 

Mas como podemos entregar o conteúdo do nosso blog de forma otimizada para qualquer lugar do mundo? É neste ponto que entra o CloudFront. Este serviço funciona como um [CDN - Content Delivery Network](http://www.wikiwand.com/pt/Content_Delivery_Network). Ele se encarrega de armazenar uma cópia de seus arquivos em diversos lugares do mundo. 

Assim se alguém acessar o seu blog do Brasil, os arquivos serão recuperados dos servidores da AWS localizados no Brasil, reduzindo a latência e entregando uma melhor experiência para o leitor.

O primeiro passo para termos estas vantagens em nosso blog é selecionar a opção CloudFront no console principal, clicar em *Create Distribution* e selecionar a opção Web. Em *Origin Domain Name* selecione o bucket criado no S3. As demais opções não precisam ser alteradas.

<figure>
	<img src="/images/2015-02-10-hospedando-seu-blog-na-aws-por-centavos/7.png" alt="Criando distribuição no CloudFront">
</figure>

No campo *Alternate Domain Names* coloque o domínio do seu blog das duas formas que são mostradas na imagem. Também adicione o objeto raiz da sua distribuição, que será o mesmo que você selecionou no S3, nesse caso o index.html. As demais opções podem permanecer com os valores padrão.

<figure>
	<img src="/images/2015-02-10-hospedando-seu-blog-na-aws-por-centavos/8.png" alt="Criando distribuição no CloudFront">
</figure>

Agora se irmos no menu lateral *Distributions* e selecionarmos a distribuição recém criada, poderemos verificar que ela tem um *Domain Name*. Essa url irá devolver também a página inicial do blog, mas agora utilizando o serviço CloudFront. Mas ainda temos uma URL pouco amigável. Chegou a hora de comprarmos nosso próprio domínio.

###Obtendo Uma URL Amigável Com Route53

Agora que já temos nossos arquivos hospedados e sendo entregues da melhor forma possível, precisamos ter uma URL amigável para que nosso blog seja encontrado facilmente pelos leitores. O primeiro passo é adquirir um domínio, que será o endereço de seu blog na web. Selecione o Route53 no console principal, *Registered Domains*, *Register Domain*. Escolha o nome do seu domínio e a extensão final da URL. Domínios .com custam $12, mas não se assuste com esse valor ele é anual. 

<figure>
	<img src="/images/2015-02-10-hospedando-seu-blog-na-aws-por-centavos/9.png" alt="Registrando um domínio no Route53">
</figure>

Clicando no botão *check* o sistema irá verificar se o domínio escolhido está disponível. Se positivo clique em *continue* até finalizar a compra.

Agora que temos nosso domínio precisamos fazer as configurações de [DNS](http://www.wikiwand.com/pt/Domain_Name_System) para quando o leitor digitar o endereço do nosso blog ele seja redirecionado para a nossa distribuição do CloudFront.

Ainda no Route53 clique na opção *Hosted Zones*, e depois em *Create Hosted Zone*. Em *Domain Name* coloque o endereço do seu blog e clique em *Create*.

<figure>
	<img src="/images/2015-02-10-hospedando-seu-blog-na-aws-por-centavos/10.png" alt="Hosted Zone no Route53">
</figure>

Com a Hosted Zone selecionada clique em *Go to Record Sets*. Clique em *Create Record Set*. Marque a opção *Alias* como Yes, e selecione sua distribuição do CloudFront no campo *Alias Target*.

<figure>
	<img src="/images/2015-02-10-hospedando-seu-blog-na-aws-por-centavos/11.png" alt="Records Sets no Route53">
</figure>

Faça o mesmo procedimento novamente, mas agora preenchendo o campo *Name* com www. Seus Records Sets devem estar parecidos com estes.

<figure>
	<img src="/images/2015-02-10-hospedando-seu-blog-na-aws-por-centavos/12.png" alt="Records Sets no Route53">
</figure>

###Pronto

Agora basta esperar a propagação do DNS, que pode levar até 24 horas e depois começar a escrever em seu blog. Mesmo após passar o período do free trial da AWS o custo de hospedagem do blog será muito baixo. Tirando o domínio que tem um custo um pouco mais elevado, você irá gastar menos de $1 por mês. 

Se interessou pelos serviços da AWS? Fique atento que virão mais posts sobre o assunto em breve. Até mais.
