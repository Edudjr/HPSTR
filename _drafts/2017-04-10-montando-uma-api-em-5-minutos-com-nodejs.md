---
layout: post
title: Montando uma API em 5 minutos com NodeJS
description: "Montando uma API em 5 minutos com NodeJS"
modified: 2016-04-10T15:27:45-04:00
tags: [Nodejs, API, Montando uma API, ngrok]
image:
  url: https://s19.postimg.org/xapr6cfsj/blog-post2.jpg
---

# Montando uma API em 5 minutos com NodeJS

As vezes precisamos criar rapidamente uma API para realizar um MOCK de algum
serviço, ou simplesmente precisamos de um _quick start_ para um novo backend.
Também é possível que você deseje apenas aprender Node mas não sabe por onde
começar. Esse post foi feito pensando em vocês :)

Algumas vezes me deparo com a seguinte situação: estamos trabalhando
no desenvolvimento do frontend de algum aplicativo, mas precisamos utilizar o
serviço de terceiros para botar a mão na massa. Contudo, o servidor da empresa
de que dependemos está fora do ar ou costuma cair com certa frequência
por algum motivo.

Por que não montar uma API rapidamente com Node e
mockar o serviço de que precisamos? É uma solução rápida para nosso problema e
não precisaremos lidar com a constante queda do serviço de que dependemos.
Nossa API irá exibir um simples **json** (mostrado abaixo) quando requisitada no
endpoint **localhost:3000/usuarios/7**.

```json
{
  "userId": 7,
  "userName": "James Bond",
  "age": 37,
  "address": {
    "streetNumber": "30",
    "streetName": "Wellington Square"
  }
}
```

Para isso utilizaremos o framework **expressjs**. De uma maneira bem
simplificada, podemos dizer que o expressjs é uma ferramenta que nos ajuda a
organizar o nosso projeto através da arquitetura MVC, simplificando também o uso
de rotas, requests e views.

## Antes de mais nada
Devemos, obviamente, instalar tudo o que precisamos para rodar
nossa API. Felizmente não precisamos instalar muita coisa, apenas o **Node** e
um módulo chamado **express-generator**.

Primeiramente, [entre no site do NodeJS](https://nodejs.org/en/) e o
instale. Após feito isso, entre no terminal e digite o seguinte comando para
instalar a ferramenta do expressjs que irá nos ajudar a criar a uma estrutura
básica para nossa API:

```bash
$ npm install express-generator -g
```

>npm é uma ferramenta de gerenciamento de pacotes do NodeJS. Ela permite que
você instale diversos **modulos** criados pela comunidade de maneira rápida e
fácil.

_**dica:** caso esteja em uma máquina **windows**, sugiro instalar o **git** para
ter acesso à ferramenta **git bash**, que funciona como um terminal linux._

## Partindo pra parte boa
Agora temos tudo que precisamos para criar a API. Entre no terminal, navegue até
sua pasta de projetos e digite o seguinte comando:

```bash
$ express myapp
```

Isso irá criar uma pasta de projeto chamada **myapp**. Navegue até ela.

```bash
$ cd myapp
```

Rapidamente, iremos olhar a estrutura que nos foi criada para o projeto:

<figure>
	<a href="https://s19.postimg.org/5c4s6ysxf/Screen_Shot_2017-04-10_at_10.00.04.png">
    <img src="https://s19.postimg.org/5c4s6ysxf/Screen_Shot_2017-04-10_at_10.00.04.png" alt="">
  </a>
</figure>

### Explicando cada arquivo
Caso você já tenha familiaridade com NodeJS, pode pular essa parte. Senão,
continue lendo para entender um pouco mais sobre essa estrutura.

Para entendermos rapidamente o que cada pasta significa,

Antes de rodar o projeto devemos instalar as dependências dele através do
npm:

```bash
npm install
```

Isso irá criar uma pasta chamada **node_modules**. Caso esteja utilizando alguma
ferramenta de versionamento, não costumamos versionar essa pasta pois ela irá
armanezar os módulos específicos da maquina local, dependendo de seu sistema
e arquitetura, por isso cada maquina deve rodar `npm install`, criando sua
própria pasta de módulos.


## Dica: como expor nosso localhost na internet?
Já ouviram falar de um serviço chamado **ngrok**?
