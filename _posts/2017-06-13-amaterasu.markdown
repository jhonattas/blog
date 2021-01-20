---
layout: post
title:  "Projeto Amaterasu"
date:   2017-06-12 08:20:00
categories: [pt, projects]
image:
  background: witewall_3.png
tags: [estrutura, amazon, aws, ruby on rails, api, backend, projects]
comments: true
share: true
---

Amaterasu é um dos deuses presentes na mitologia japonesa, outro fato que me levou a escolher este nome foi uma das habilidades especial de um personagem no anime Naruto.


<h3>Objetivo</h3>
Esta api tem um objetivo estremamente claro dentro da aplicação, ela foi escrita para gerir uma camada entre o extrato gerado pela VISA em tempo real, e a aplicação final do cliente. Era necessário que toda transação realizada dentro de um contexto especifico fosse tratada de uma maneira "especial", exemplo: Uma compra de um grupo especifico / pré-definido deveria gerar pontos baseando-se em tal evento.


<br/>
<h3>Porque AMATERASU?</h3>
Neste caso o nome é puramente estético, e o utilizei para batismo por conta de uma habilidade de personagens do anime Naruto (do qual sou um bocado fã), mas sei também que existe conexão com a mitologia oriental.


<br/>
<h3>Tecnologias</h3>
Está api é escrita em Ruby, e utiliza quase que completamente uma gema chamada <u>Shouryuken</u> para executar seus serviços. A gema Shouryuken se conecta com uma fila de dados recebidos pelo serviço SQS da Amazon, e os absorve para dentro do banco de dados, para que posteriormente sejam consumidos pelas APIs que alimentam os aplicativos legados, podendo então exibir o extrato atualizado, a contagem de pontos, ou até mesmo as operações que podem ser realizadas naquele dia (com seu custo atualizado).


<br/>
<hr/>
<b>Github</b>: Não disponível<br/>