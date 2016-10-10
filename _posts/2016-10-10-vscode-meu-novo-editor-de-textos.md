---
layout: post
title: "VSCode - Meu novo editor de texto"
modified:
categories: 
excerpt:
tags: [vscode, texteditor, produtividade]
image:
  feature:
date: 2016-10-10T12:00:00-02:00
---

Quem trabalha comigo sabe que sou fã do Sublime Text, até já escrevi um post sobre os [melhores atalhos](http://rbittencourt.com/sublime-text-atalhos/). Isso fez com que eu demorasse a dar uma chance para o [Visual Studio Code](https://code.visualstudio.com) (VSCode daqui para frente). Essa semana resolvi testar o tão falado editor de texto da Microsoft, com um certo preconceito ainda, e o Sublime acabou perdendo mais um usuário.

Na primeira interação as coisas parecem um pouco estranhas, fora do lugar. Mas em 10 minutos já estava me impressionando com as facilidades que o editor oferece. Abaixo vou falar sobre os principais destaques na minha opinião.

### Free e Open Source

Você pode não saber (é claro que sabe xD toda hora ele lhe alerta) mas o Sublime Text é um editor pago, ou seja para utilizá-lo dentro da lei você precisa de uma licença. Além disso seu código fonte é fechado.

Aqui entram os dois primeiros destaques que percebi. O VSCode é free e open source, [seu código está no Github](https://github.com/Microsoft/vscode) possuindo uma comunidade bastante ativa ao redor.

Percebe-se analisando o repositório que o editor está em constante evolução. É possível até baixar a [versão do dia atual](https://code.visualstudio.com/insiders) o que eles chamam de Insiders, para dar feedback sobre as novas funcionalidades e possíveis bugs. 

### Debugging

Não sou dev Node, mas venho estudando bastante nos últimos tempos. Para mim sempre foi chato debuggar. No início enchia de *console.log*, depois descobri o debugger do próprio Node, mas ainda não era legal.

Por isso me espantei quando vi o debugger do VSCode.  

<figure>
	<img src="/images/2016-10-10-vscode/1.png" alt="O debugging no VSCode">
</figure>

Como pode-se perceber na imagem, ele lhe dá tudo aquilo que você está acostumado a ter em IDEs, breakpoints, watchers, callstack, facilitando o debugging.

### Git

Outra feature interessante, é a integração com o Git. De forma fácil você consegue visualizar os arquivos modificados em seu repositório. Além disso é possível adicionar e remover da área de stage, reverter e commitar. Ainda prefiro o terminal, para estas tarefas mais corriqueiras, mas se você for meio avesso ao terminal vai gostar. 

<figure>
	<img src="/images/2016-10-10-vscode/2.png" alt="Projeto Git no VSCode">
</figure>

Ainda sobre Git, o maior destaque fica por conta do diff de arquivos. Simplesmente funciona como você espera que ele funcione.

<figure>
	<img src="/images/2016-10-10-vscode/3.png" alt="Diff de mudanças no VSCode">
</figure>

### IntelliSense
O IntelliSense fornece diversas informações sobre o contexto do arquivo que está aberto no VSCode. Por exemplo em um arquivo Javascript, ele lhe dará informações sobre as funções e seus parâmetros, além de *autocomplete*.

<figure>
	<img src="/images/2016-10-10-vscode/6.gif" alt="IntelliSense exibindo as funcções">
</figure>
*Fonte:https://code.visualstudio.com/docs/editor/intellisense*

### Terminal

Em casa tenho o Ubuntu como sistema operacional, por isso utilizo com frequência o terminal. O VSCode permite que você o utilize sem sair do editor. Não precisar alternar de aplicativo melhora a produtividade. Para quem gosta de atalhos, *ctrl+'* abre ou fecha o terminal.

<figure>
	<img src="/images/2016-10-10-vscode/4.png" alt="Utilizando o terminal diretamente no VSCode">
</figure>

### Extensões

Este é um ponto que eu gosto muito no Sublime, seria difícil migrar para o VSCode se ele não fosse extensível, felizmente é. Existem extensões de todos os tipos, debuggers específicos, atalhos para determinadas linguagens ou até que adicionam funcionalidades ao VSCode. Existe um [marketplace na web](https://marketplace.visualstudio.com/VSCode) mas o do próprio editor é suficiente. Você também pode escrever suas próprias extensões e publicá-las no marketplace para que outras pessoas utilizem.

<figure>
	<img src="/images/2016-10-10-vscode/5.png" alt="Extensões no VSCode">
</figure>

### Desvantagens

Até agora a única desvantagem que vi no VSCode em relação ao Sublime é o tempo de inicialização. O Sublime é muito rápido nesse quesito, enquanto o VSCode leva uns 4 segundos para abrir. Não sou aficionado por estes números, ao abrir a primeira vez o editor para mim nem faz diferença este tempo. O problema começa a incomodar na troca constante de projetos. Mas mesmo com este problema, decidi seguir utilizando o VSCode, espero que o melhorem com o tempo.

Conhece outros problemas, ou discorda de algo que eu escrevi, deixe um comentário e vamos conversar.

### Dicas e links

[Repositório oficial](https://github.com/Microsoft/vscode)

[VS Code Tips and Tricks](https://github.com/Microsoft/vscode-tips-and-tricks)

[Visual Studio Code: Increasing Productivity via Key Binding Management](http://www.hongkiat.com/blog/key-binding-management-visual-studio-code/)

[Best of Visual Studio Code: Tips and Tricks](https://www.youtube.com/watch?v=fkM9jCRBwSs)