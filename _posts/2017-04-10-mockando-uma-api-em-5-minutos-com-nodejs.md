---
layout: post
title: Criando uma API em 5 minutos com NodeJS
description: "Criando uma API em 5 minutos com NodeJS"
modified: 2016-04-16T15:27:45-03:00
tags: [Nodejs, API, mock, ngrok]
image:
  url: https://s19.postimg.org/53r3kqpyr/blog-post5.jpg
---

# Mockando uma API em 5 minutos com NodeJS

>Esse artigo pode ser um bom ponto de partida para iniciantes em NodeJS :)

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

Se tudo correu bem, você verá uma página escrita "Express - welcome to express".

Nosso objetivo agora é imitar o serviço que desejamos utilizar.
Supondo que o serviço retorne o mesmo json mencionado no começo do post, devemos
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

### Resumindo
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

Um link aleatório será criado na saida do programa:

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

## Explicando a estrutura do programa
Vamos analisar de forma resumida a estrutura do nosso código e focar em alguns
pontos mais importantes.

### bin/www
Partindo pelo primeiro arquivo de nossa estrutura, o arquivo **bin/www**, podemos
ver que, basicamente, ele importa o arquivo **app.js** localizado no root
`var app = require('../app');`, cria um servidor a partir dele `var server = http.createServer(app);` e o coloca para escutar na porta 3000 `server.listen(port);`.

> O arquivo **app.js** só pôde ser importado através do comando _require_ pois, no final do arquivo, declarou: `module.exports = app;`, o que permite que ele seja "exportado".

### public/
Todos os arquivos em public são **públicos** e podem ser acessados diretamente
da url, seguindo o estilo **http://root/pasta/arquivo**. Por exemplo, o arquivo
**style.css** existe dentro da pasta **stylesheets**. Para acessá-lo, podemos
jogar no navegador: http://localhost:3000/stylesheets/style.css.

### routes/
Aqui se encontram todas as "rotas" que irão compor a url de nossa API ou website.
As rotas definem os **endpoints** de nossa aplicação. Em outras palavras, ela
irá definir o que vai acontecer quando um cliente tentar acessar, por exemplo,
o caminho _root_ da API (http://localhost:3000/), ou então o endpoint _/users_ (http://localhost:3000/users).

Você deve ter se perguntado durante o tutorial sobre como, ao adicionar o código
`router.get('/:id', function(req, res) {...}`, conseguimos redirecionar a url
**http://localhost:3000/users/47** para a função que criamos. De onde veio o **/users/**
do caminho??

Podemos ver que existem os arquivos **index.js** e **users.js** na pasta routes.
Agora, pulando rapidamente para o arquivo **app.js** (que iremos explicar melhor
a seguir) podemos ver o seguinte código:

```javascript
var routes = require('./routes/index'); //#1
var users = require('./routes/users'); //#2
.
.
.
app.use('/', routes); //#3
app.use('/users', users); //#4
```

Em **#1** e **#2**, podemos ver que está sendo importado nossos dois arquivos da pasta routes.

Em seguida, **#3**, ele está dizendo _"olha, para o caminho root '/', use o arquivo
routes/index.js como rota"_. Ou seja, para todos os caminhos que partam da url
root (http://localhost:3000/), o programa vai olhar dentro do arquivo
index.js para buscar o caminho correspondente. Por exemplo, se quisermos que nosso
servidor responda na url http://localhost:3000/respondeaqui, teremos que incluir
dentro de **routes/index.js** o seguinte código:

```javascript
/* arquivo: routes/index.js
 * url de resposta: http://localhost:3000/respondeaqui
 */
router.get('/respondeaqui', function(req, res, next) {
  res.send('ok');
});
```

No caso do **#4**, ele está dizendo _"olha, para o caminho /users/, use o arquivo
routes/users.js como rota"_. Ou seja, para todos os caminhos que partam da url
http://localhost:3000/users/, o programa vai olhar o arquivo users.js para buscar
o caminho em questão. Como exemplo, se quisermos que nosso servidor agora responda
na url http://localhost:3000/users/agoraaqui, teremos de incluir no arquivo
**routes/users.js** o seguinte código:

```javascript
/* arquivo: routes/users.js
 * url de resposta: http://localhost:3000/users/aquiagora
 */
router.get('/aquiagora', function(req, res, next) {
  res.send('ok');
});
```

### views/
Talvez você tenha estranhado os arquivos com terminação **.jade** encontrados nesta pasta.
Jade, hoje chamado Pug, é uma das diversas **template engines** disponíveis para Node.
As template engines basicamente substituem variáveis em arquivos de template por
valores atuais e transformam o template, junto com os valores, em arquivos HTML,
que são então enviados para o cliente.

**Mas como elas funcionam na prática, e como enviamos esses arquivos para o cliente?**

Lembra quando acessamos o root da aplicação
e recebemos de volta uma página escrita "Express - welcome to express"?
Vamos ver como esse fluxo funciona.

Primeiramente, o cliente vai acessar um recurso de nossa API. No caso, ele digita
no navegador o caminho root da aplicação, http://localhost:3000/.

Ao fazer isso, estaremos acessando o arquivo **routes/index.js** e buscando pela
função que representa o caminho root (como discutido em **routes**):

```javascript
/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index', { title: 'Express' });
});
```

Quando o comando `res.render(...)` é chamado, ele vai tomar uma string como
argumento e buscar na pasta **views** o arquivo correspondente à string. Nesse caso,
ele estará buscando o arquivo **index.jade** (repare que não precisamos definir a
terminação do arquivo no comando).

Nesse caso, o comando `res.render(...)` possui dois argumentos: o primeiro é a string,
no caso **'index'**, e o segundo argumento é um json, **{ title : 'Express' }**.
Quando um json é passado como segundo argumento, ele será acessível dentro
da view buscada.

Vamos abrir então o arquivo **index.jade**:

```html
extends layout

block content
  h1= title
  p Welcome to #{title}
```

* **extends layout** está importando o arquivo layout.jade para dentro de index.jade.

* **#{title}** é a variável que passamos como segundo argumento em `res.render(...)`,
logo, **#{title}** irá ser substituido por **Express** quando o arquivo for
renderizado para HTML:

```html
<html>
  <head>
    <title>Express</title>
    <link rel="stylesheet" href="/stylesheets/style.css">
  </head>
  <body>
    <h1>Express</h1>
      <p>Welcome to Express</p>
  </body>
</html>
```

Logo após ser renderizado, o arquivo será redirecionado para o cliente, que irá ver
em sua tela do navegador a mensagem "Express - welcome to express".

> **res** em **res.render(...)** representa o **response**, ou a "resposta",
da requisição. Logo, esse comando está dizendo "responda para esse cliente com
o arquivo index.jade renderizado".


### app.js
Este um dos arquivos mais importantes de nossa estrutura. Ele irá tomar conta das configurações iniciais da nossa aplicação.
Primeiramente, ele importa alguns módulos:

```javascript
var express = require('express');
var path = require('path');
...
```

Em seguida, define o caminho padrão para as views e define qual template engine que será utilizada (poderia ser utilizada a 'ejs', por exemplo).

```javascript
// view engine setup
app.set('views', path.join(__dirname, 'views'));
app.set('view engine', 'jade');
```

Os restantes `app.use(...)` irão definir _middlewares_, ou seja, funções intermediárias, para tratar os erros que venham a ocorrer em ambiente de desenvolvimento `if (app.get('env') === 'development') { ... }` ou de produção. Para mais informações sobre middlewares, acesse
a [página](https://expressjs.com/en/api.html#app.use) do express.

 > Os middlewares são ativados em cascata, portanto, ordem importa.

### Fim!
Ficamos por aqui! Espero que tenham entendido um pouquinho sobre o como o Node
em combinação com o Express funcionam e consigam aproveitar desse tutorial de
alguma maneira útil. Até breve :)
