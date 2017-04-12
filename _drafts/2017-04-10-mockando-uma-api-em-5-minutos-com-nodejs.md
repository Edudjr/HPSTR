---
layout: post
title: Mockando uma API em 5 minutos com NodeJS
description: "Mockando uma API em 5 minutos com NodeJS"
modified: 2016-04-10T15:27:45-04:00
tags: [Nodejs, API, mock, ngrok]
image:
  url: https://s19.postimg.org/xapr6cfsj/blog-post2.jpg
---

# Mockando uma API em 5 minutos com NodeJS

>Esse artigo pode ser um bom ponto de partida para iniciantes em NodeJS :)

As vezes precisamos criar rapidamente uma API para realizar um MOCK de algum
serviço, ou simplesmente precisamos de um _quick start_ para um novo backend.

Algumas vezes me deparo com a seguinte situação: estamos trabalhando
no desenvolvimento do frontend de algum aplicativo, mas precisamos utilizar o
serviço de terceiros para botar a mão na massa. Contudo, o servidor da empresa
de que dependemos está fora do ar ou costuma cair com certa frequência
por algum motivo.

Por que não montar uma API rapidamente com Node e
mockar o serviço de que precisamos? É uma solução rápida para nosso problema e
não precisaremos lidar com a constante queda do serviço de que dependemos.
Nossa API irá exibir um simples **json** (mostrado abaixo) quando requisitada no
endpoint **localhost:3000/users/47**.

```json
{
  "userId": 47,
  "userName": "Agente 47",
  "age": 27,
  "address": {
    "streetNumber": "3000",
    "streetName": "Wellington Square Street"
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

> **npm** é uma ferramenta de gerenciamento de pacotes do Node. Ela permite que você instale diversos **módulos** de maneira rápida e fácil.

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

_Obs: O objetivo deste pequeno tutorial é mostrar uma maneira rápida de se criar uma API funcional, por isso o código será explicado resumidamente no final do artigo._


Devemos agora instalar todas as dependências do nosso projeto:

```bash
$ npm install
```
> O comando **npm install** irá instalar todas as dependências listadas no arquivo **package.json**. Isso irá criar uma pasta chamada **node_modules**. Caso esteja utilizando alguma
ferramenta de versionamento, não costumamos versionar essa pasta pois ela irá
gerar os módulos dependendo de seu sistema
e arquitetura, por isso cada máquina deve rodar npm install, criando sua
própria pasta de módulos.

Após as dependências serem instaladas, basicamente já temos um servidor funcionando. Apenas digite o comando abaixo no terminal e em seguida abra seu navegador no endereço http://localhost:3000.

```bash
$ DEBUG=server:* npm start
```
> O comando `npm start` irá procurar pelo script **start** dentro do arquivo **package.json**. Ou seja, `DEBUG=server:* npm start` e `DEBUG=server:* node ./bin/www` são equivalentes. No fim, o comando `node` é o que irá inicializar nossa aplicação.
O comando `DEBUG=server:*` serve apenas para printar na tela eventuais logs
gerados pelo módulo **debug**, automaticamente instalado nesse projeto.

Se tudo correu bem, você verá uma página escrita "Express".

Nosso objetivo agora é _mimicar_ o serviço que desejamos utilizar.
Supondo que o serviço retorno o mesmo json mencionado no começo do post, devemos
então colocar o seguinte código dentro do arquivo **routes/users.js**, logo antes de `module.exports = router;`:

```javascript
router.get('/:id', function(req, res) {
  var id = req.params.id;
  var json = {
    userId: id,
    userName: "Agente "+id,
    age: 27,
    address: {
      streetNumber: "3000",
      streetName: "Wellington Square Street"
    }
  }
  res.json(json);
});
```

Agora, abra novamente o terminal, mate a aplicação com `ctrl+c` e rode-a novamente
com `DEBUG=server:* npm start`. Precisamos fazer isso para cada modificação no
código.

>DICA: caso esteja cansado de _restartar_ a aplicação toda vez que fizer alguma mudança,
 simplesmente instale globalmente o módulo **nodemon**: `$ npm install -g nodemon` e
 dentro do arquivo **package.json** troque "node" por "nodemon" no script de start.
 Esse módulo irá reiniciar automaticamente nossa aplicação sempre que algum arquivo
 for salvo.
 ```json
 "scripts": {
   "start": "nodemon ./bin/www"
 }
 ```

Agora abra novamente o navegador e coloque na url: http://localhost:3000/users/47.
Pronto, o resultado é o mesmo que estavamos esperando no início do artigo! Mude
o número no final da URL para perceber que o retorno também é alterado, assim
podemos _mockar_ o serviço e verificar que ele está chamando diversos usuários e
produzindo o resultado no frontend.

## Resumindo
Por fim, só precisamos de **dois** comandos e algumas linhas de código
para gerar uma API funcional que pode resolver aquele problema do servidor que
**vive caindo** ou **demora séculos** para responder. Partindo desse princípio,
é possível _mockar_ diversos serviços, incluindo outros requests como post, put
ou delete. Entre no site do [express](https://expressjs.com/en/api.html) para mais
detalhes.

## Dica: como expor nosso localhost na internet?
E se por um acaso estivermos trabalhando em alterações rápidas no nosso localhost
e precisarmos mostrar essas alterações para alguem de fora da nossa rede, ou seja, pela internet?

**ngrok** for the rescue!

[Ngrok](https://ngrok.com) é uma ferramenta que expõe nosso localhost na internet.
Para usá-la, faça o download do instalador para o seu sistema, e digite no terminal:
```bash
$ ngrok http 3000
```

Um link randômico será criado na saida do programa:
```bash
Tunnel Status                 online
Version                       2.0/2.0
Web Interface                 http://127.0.0.1:4040
Forwarding                    http://92832de0.ngrok.io -> localhost:80
Forwarding                    https://92832de0.ngrok.io -> localhost:80

Connnections                  ttl     opn     rt1     rt5     p50     p90
                              0       0       0.00    0.00    0.00    0.00
```
No caso, nosso link gerado é o `http://92832de0.ngrok.io`.

### Explicando a estrutura
Para entendermos rapidamente o que cada pasta significa, vamos analisar
