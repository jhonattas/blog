---
layout: post
title:  "Projeto Cyano"
date:   2017-06-12 12:39:00
categories: [pt, estrutura, nodejs]
image:
  background: witewall_3.png
tags: [estrutura, amazon, aws, nodejs, vuejs, bootstrap, frontend]
comments: true
share: true
---

Ainda no segmento de construir coisas novas, e explicar o intuito e decisões tomadas no processo, nasceu o projeto CYANO. Ele nada mais é que uma SPA construida com [Node.js](https://nodejs.org/) e [Vue.js](https://vuejs.org/).
<br/>

O frontend realiza algumas chamadas das quais a app depende (principalmente no consumo de dados para exibição), para isso se conectando no backend Atlas* (que por sua vez esmiuçarei em detalhes posteriormente). O link para o código fonte pode ser encontrado no final deste post, e as informações para executa-la estarão disponíveis no repositório da mesma. Este projeto está em constante atualização, por ser um experimento meu.


<br/>
<h3>Objetivo</h3>
Construir uma aplicação simples para exibir alguns detalhes simples de projetos que construi ou participei, utilizando dentre alguns recursos Node.js, e abrindo o código-fonte para que possa ser consultado por outras pessoas (desenvolvedores, ou não).


<br/>
<h3>Porque CYANO?</h3>
Bem, o nome Cyano é a versão em inglês da cor Ciano. Vez ou outra eu topava com essa palavra em algum poema, texto ou cancão, e então uma das bandas de que mais gosto (a Fresno) lançou um cd com este nome. O Ciano, apesar de ser uma cor substrativa do vermelho é uma espécie de azul (que é uma cor que sempre gostei demais), como o sketch* inicial deste projeto continha um quantidade muito abundante de azul, decidi nomea-lo assim. Também tem o fato de que Blue (azul do inglês) é na maior parte das vezes relacionado a tristeza ou melancolia, e admito que eu estava em uma fase em que sentia um bocado disso, mas ainda assim sentia e ainda sinto agora que posso criar algo bonito disso.


<br/>
<h3>Tecnologias</h3>
Como citado anteriormente, Cyano possui como base um framework de frontend chamado <u>Vue.js</u>, escolhi o mesmo porque dentre os que eu estava estudando naquele momento (que eram Vue.js, React e Angular2), foi o que me pareceu mais flexivel e com maior acesso (rápido) a comunidade. Pessoalmente ainda utilizo e estudo os outros dois com frequencia, mas para este projeto fiquei tentato a explorar mais do Vue.js, e descobrir de fato porque o número de adptos a ele vem crescendo tanto. E projetos pessoais são para isso não é? <i>Liberdade para experimentar e descobrir mais do que você pode fazer com tecnologias emergentes.</i>


<br/>
<h3>Estrutura</h3>
A SPA construida é bem simples, no momento apresentando uma Home, uma pagina com projetos, um link para o blog (que é onde você se encontra agora), e um link para o email de contato.<br/>
Uma vez que criei a estrutura, a segunda coisa que fiz foi implementar internacionalização na página, para que a mesma operasse em português e em inglês, é um recurso simples (e que admito precisarei ir refinando aos poucos), mas são pequenos detalhes que dão um charme diferente, e um ar de profissionalismo a página.
<br/><br/>
A estrutura dessa SPA inclusive é algo que estou constantemente modificando, a medida que me sinto mais confortável com a estrutura do Vue.js, vou refatorando pedaços de código que escrevi previamente com o objetivo de tornar o código do repositório cada vez mais limpo. Pode ser que ninguém nunca chegue a ver todos aqueles códigos que estão por trás, mas se chegar a isso quero que tenham ao menos um pouco de percepção do programador que sou (mesmo meu foco dos últimos anos não ser Javascript, estou engatinhando nisso ainda) pela lógica utilizada.
<br/><br/>
Para a aparência e uniformidade de algumas telas, utilizei-me do Bootstrap Saas, mas posteriormente vou avaliar se vou mantê-lo.. Tenho feito testes com a Semantic UI, e definitivamente estou encantado com a mesma, se eu não carrega-la para o Cyano, o farei em outro projeto.


<br/>
<h3>Comunicação</h3>
Para carregar dados a aplicação realiza chamadas em ajax para uma API no backend de nome Atlas* (que estou escrevendo em conjunto com a Cyano), no inicio a maior parte dos dados eram estaticos, mas agora com a implementação da área restrita e o amadurecimento inicial da API os dados são dinâmicos.
<br/>Outro ponto importante a ser ressaltado sobre comunicação, é que todas as imagens do qual a aplicação depende estarão sendo migradas para o S3 da Amazon, <u>se é para construir algo top, melhor fazer certinho :)</u> .

<br/>
<hr/>
<b>Github</b>: [https://github.com/jhonattas/cyano](https://github.com/jhonattas/cyano)<br/>