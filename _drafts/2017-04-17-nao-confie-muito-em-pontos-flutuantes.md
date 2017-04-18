---
layout: post
title: Não confie muito em pontos flutuantes
description: "Não confie muito em pontos flutuantes"
modified: 2016-04-17T15:27:45-03:00
tags: [dinheiro, pontos flutuantes, unidade monetária]
image:
  url: https://s19.postimg.org/udlq97v8z/blog-post3.jpg
---
# Pontos flutuantes não são sempre confiáveis.

Quando estamos escrevendo um código para uma aplicação em produção, devemos
tomar muito cuidado com as linhas que digitamos. É muito importante que saibamos
utilizar corretamente os tipos de dados para que o resultado esperado
seja o que realmente desejamos, e não uma anomalia que pode acabar com seu projeto.

## Cadê meu dinheiro?
Suponha que estejamos escrevendo uma aplicação que lide com dinheiro. Não queremos
que os usuários do sistema percam dinheiro ao transferir ou receber valores,
não é mesmo? Isso seria um erro grave em nosso sistema.

Considere o seguinte cenário: você possui em sua carteira virtual **R$4,30**. Então, recebe **R$6,15** reais de seu pai e mais **R$1,85** de sua mãe. Vamos, inocentemente,
jogar esses valores em números reais (pontos flutuantes) e ver o que acontece:

_javascript:_

```javascript
var soma = 4.3 + 6.15 + 1.85;
console.log(soma);
//Resultado: 12.299999999999999
```

_python:_

```python
soma = 4.3 + 6.15 + 1.85
print(soma)
#Resultado: 12.299999999999999
```

Ué? Estavamos esperando **R$12,30** redondos, não esse número **quebrado**!
Infelizmente, nosso número foi quebrado devido à utilização de pontos flutuantes,
e, se formos simplesmente pegar os dois decimais depois do ponto,
estaremos perdendo 1 centavo. Imagina essa situação ocorrendo em um sistema de
pagamentos, onde milhares de transações são realizadas por dia. Uma grande
quantia de dinheiro seria perdida nessa brincadeira.


## Solução
A saída aqui é utilizar tipos inteiros, ou decimais com precisão de duas casas, para representar esses tipos de valores. Por exemplo, poderíamos tratar nosso dinheiro na base dos centavos e dividí-lo por 100 para exibir seu valor em real:

_javascript:_

```javascript
var soma = 430 + 615 + 185;
console.log(soma/100);
//Resultado: 12.30
```

_python:_

```python
soma = 430 + 615 + 185
print(soma/100)
#Resultado: 12.30
```

Não iremos nos aprofundar em como funcionam os pontos flutuantes, mas saibam que eles dependem também da arquitetura (32/64 bits) e podem as vezes se comportar de formas que não esperamos, como no caso acima.

Ficamos por aqui, não deixem os pontos flutuantes roubarem seu dinheiro.
