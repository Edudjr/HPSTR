---
layout: post
title: Montando uma API em 5 minutos com NodeJS
description: "Demo post displaying the various ways of highlighting code in Markdown."
modified: 2016-04-07T15:27:45-04:00
tags: [Nodejs, API, Montando uma API, Montando uma API com Nodejs]
image:
  url: https://s19.postimg.org/xapr6cfsj/blog-post2.jpg
---

# Montando uma API em 5 minutos com NodeJS

As vezes precisamos criar rapidamente uma API para realizar um MOCK de algum
serviço, ou simplesmente precisamos de um _quick start_ para um novo backend.
Também é possível que você deseje apenas aprender Node mas não sabe por onde
começar. Esse post foi feito pensando em vocês :)

Algumas vezes me deparo no trabalho com a seguinte situação: estamos trabalhando
no desenvolvimento do frontend de algum aplicativo, mas precisamos utilizar o
serviço de terceiros para botar a mão na massa. Contudo, o servidor da empresa
de que dependemos está fora do ar ou costuma cair com certa frequencia
por algum motivo.

Por que não montar uma API rapidamente com Node e
mockar o serviço de que precisamos? É uma solução rápida para nosso problema e
não precisaremos lidar com a constante queda do serviço de que dependemos.
Nossa API irá exibir um simples **json** (mostrado abaixo) quando requisitada no
endpoint **localhost:3000/usuarios**.

```json
{
  "userId": 47,
  "userName": "Eduardo Domene Junior",
  "age": 26,
  "address": {
    "streetNumber": "647",
    "streetName": "Rua da Coxinha Dourada"
  }
}
```

# Dica: como expor nosso localhost na internet?
Já ouviram falar de um serviço chamado **ngrok**?
