---
author: rodrigopanachi
comments: true
date: 2013-03-20 14:27:02+00:00
layout: post
slug: pair-programming-remoto-com-screen-e-vim
title: Pair Programming remoto com Screen e Vim
wordpress_id: 1111
categories:
- quick tips
- real world
- tutorial
- web
tags:
- linux
- pair programming
- screen
- vim
---

Limitar a produtividade e colaboração interpessoal ao espaço físico de um escritório parece uma idéia cada vez menos viável no ramo de desenvolvimento de software.

Empresas de TI bem sucedidas como [37signals](http://37signals.com/) e [Github](https://github.com/) apostam em times e colaboradores distribuídos pelo mundo trabalhando remotamente em seus projetos.

Usando as ferramentas e práticas certas é possível [trabalhar remotamente](http://37signals.com/svn/posts/3435-remote-office-not-required-the-new-book-by-37signals-coming-fall-2013) e até mesmo [parear com outro desenvolvedor](http://en.wikipedia.org/wiki/Pair_programming) à distância.


## Requisitos





	
  * ssh

	
  * screen

	
  * vim

	
  * skype


Para compartilhar o mesmo "contexto" remotamente, ambos desenvolvedores deverão ter acesso ao mesmo ambiente de desenvolvimento, via **ssh**. Estando em modo texto, será necessário utilizar um editor compatível, neste caso o **[VIM](http://www.vim.org/)**.  E para compartilhar o terminal em tempo real, utilizaremos o Screen. Para comunicação, basta utilizar o Skype. Simples não!?


## Instalação


Uma vez escolhido o ambiente de desenvolvimento comum (um servidor de homologação, por exemplo), instale o Screen:

    
    $ sudo apt-get install screen


Em seguida, configure as permissões de execução:

    
    $ chmod u+s /usr/bin/screen
    $ chmod 755 /var/run/screen


O Screen roda como um _daemon_, mantendo um _buffer_ da tela. Sendo assim, o primeiro passo é iniciar a sessão do Screen:

    
    $ screen -S nomedasessao


Isso criará uma sessão com o nome "nomedasessao" e será exibido um shell "limpo", o que quer dizer que você já está conectado a esta sessão. Para verificar, execute:

    
    $ screen -ls
    There is a screen on:
            8095.nomedasessao	(19-03-2013 23:32:54)	(Attached)
    1 Socket in /var/run/screen/S-user.


A partir de agora, o buffer desta sessão pode ser compartilhado com outro usuário conectado. Basta que seu par se logue no servidor via ssh e execute:

    
    $ screen -x nomedasessao


Pronto! O que você digitar, seu par vai ver e vice-versa. Assim, basta abrir o VIM e começar a parear remotamente.

Para se desconectar da sessão atual, execute:

    
    # screen -d
    [remote detached from 8095.nomedasessao]




## Usando o Screen


O Screen é simples e poderoso. É possível criar abas (window), dividir a tela (split), rolar a tela (copy mode), etc.

Todos comandos começam com Ctrl + a, em seguida o comando ou atalho. Seguem alguns comandos e atalhos que serão muito úteis do seu dia-a-dia pareando:

**Copy mode (scroll)**
Inicie o modo de cópia com Ctrl-a + [ (colchete para esquerda)
Navegue pela tela com as setas ou pageup/pagedown;
Marque o início da seleção do texto com <espaço> e termine com <espaço> para copiar;
Cole com Ctrl-a + ] (colchete para direita);

**Windows**
Crie uma janela (ou aba) com Ctrl-a + c
Liste as janelas com Ctrl-a + " (aspas)
Altere para a janela com Ctrl-a <numero de 0-9>
Feche (ou mate) a janela atual com Ctrl+a k

**Split**
Divida a tela horizontalmente com Ctrl-a + S
Divida a tela verticalmente com Ctrl-a + V
Mude de split com Ctrl-a + Tab
Feche um split com Ctrl-a + X

Para digitar um comando: Ctrl-a + :

Consulte o [manual do Screen](http://linux.die.net/man/1/screen) para a lista completa de atalhos/comandos.


## Dicas e considerações


Esta é uma abordagem simplista da utilização do Screen. Deixei vários detalhes de fora do post para tentar ser o mais didático possível. Para informações mais completas como configurações, gerenciamento de sessões e usuários, consulte o [manual oficial](http://linux.die.net/man/1/screen).

Existem outras alternativas como o [tmux](http://tmux.sourceforge.net/), mas o conceito envolvido é o mesmo apresentado aqui.

Se você estiver programando em Rails, provavelmente precisará de 3 contextos: console, server e editor (Vim). Recomendo utilizar cada contexto como "window" na mesma sessão do Screen.

Utilize o Skype (ou outro voip de sua preferência) durante todo o tempo em que estiverem pareando e estabeleça intervalos. [Pomodoro](http://www.pomodorotechnique.com/) pode ser uma boa opção.

Dúvidas, críticas ou sugestões nos comentários. Sucesso!


## Referências


[http://www.linux.com/learn/tutorials/442418-using-screen-for-remote-interaction
](http://www.linux.com/learn/tutorials/442418-using-screen-for-remote-interaction)[http://linux.die.net/man/1/screen
](http://linux.die.net/man/1/screen)[http://aperiodic.net/screen/quick_reference](http://aperiodic.net/screen/quick_reference)
