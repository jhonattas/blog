---
layout: post
title:  "Projeto Atlas"
date:   2017-06-11 07:21:00
categories: [pt, estrutura, nodejs]
image:
  background: witewall_3.png
tags: [estrutura, amazon, aws, nodejs, passport, mongodb, mysql, jwt, json, api, backend]
comments: true
share: true
---

API integradora escrita em [Node.js](https://nodejs.org/), serve para gerir usuários, imagens das quais aplicações dependem, conexões com banco de dados, etc.
<br/>

A API serve para integrar os serviços principais das quais os apps dependem, como realizar consultas a banco de dados, se conectar a outros serviços (como por exemplo o S3), gerir usuários e níveis de acesso, dentre outras coisas.


<br/>
<h3>Objetivo</h3>
Criar esta API backend tem como objetivo principal separar adequadamente minhas camadas e serviços, com a criação da CYANO para cuidar do frontend, não seria adequado incorporar no projeto toda a camada de backend, até funcionaria se fosse feito mas não ficaria devidamente modularizado, e posteriores substituições ou atualizações de recursos e tecnologias (bem como o próprio deploy da aplicação) teriam grandes chances de se converter em dor de cabeça com o avanço do projeto.


<br/>
<h3>Porque ATLAS?</h3>
Na mitologia grega <a href="https://pt.wikipedia.org/wiki/Atlas_(mitologia)" target="_new">ATLAS</a> é um dos titãs que carrega o mundo, esta API por sua vez tem uma responsabilidade parecida na estrutura que estou dos serviços que estou montando aqui. O ATLAS se conectará com o frontend do domínio principal, e com front-ends como Android, iOS e outros que podem vir.


<br/>
<h3>Tecnologias</h3>
O ATLAS é escrito em Node.js, e possui algumas dependências que podem ser consultadas no repositório da aplicação, bem como FS, MySQL, JWT, e PASSPORT (para autenticações).


<br/>
<h3>Estrutura</h3>
Nada é muito complexo aqui, a estrutura da ATLAS é quase que totalmente baseada no modelo REST, e a maior parte das chamadas retornam um JSON com a informação requisitada. O amadurecimento da mesma é mais focado em rotas, porque a medida que mais apps forem incorporados no sistema, novas rotas irão surgindo para alimentar a demanda de protótipos.


<br/>
<hr/>
<b>Github</b>: [https://github.com/jhonattas/atlas](https://github.com/jhonattas/atlas)<br/>