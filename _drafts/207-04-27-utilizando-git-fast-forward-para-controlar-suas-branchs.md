---
layout: post
title: Utilizando GIT NO FAST FORWARD para controlar suas branchs
description: "Utilizando GIT NO FAST FORWARD para controlar suas branchs"
modified: 2016-04-17T15:27:45-03:00
tags: [GIT, Branch, No Fast Forward]
image:
  url: https://images.pexels.com/photos/2740/nature-animal-fur-dangerous.jpg?w=940&h=650&auto=compress&cs=tinysrgb
---
Motando uma estrutura de branchs com a ajuda do No Fast Forward.

Recentemente discutimos no trabalho um padrão para utilização de branchs que
se adequasse às nossas necessidades internas. Chegamos à uma proposta
parecida com o **Gitflow**, porém com algumas mudanças para tornar mais fácil
o trabalho dos QAs, e também de integração contínua, em nossos projetos.

A principal mudança é que a branch **release** nunca morre. Ela não começa
a partir da **dev**(como no Gitflow), mas sim a partir do commit inicial, assim
como a **master** e a própria dev. Essas três branchs possuem algumas
características importantes: são **fixas**, nunca morrem e nunca são commitadas
diretamente. Ou seja, possuem apenas merges.

Resumidamente, neste modelo, a branch **master** irá conter **somente** merges
de **releases** já testadas, prontas para a produção, e refletem todas as
versões disponibilizadas publicamente, devidamente _taggeadas_. O
desenvolvimento de novas features éfeito em branchs que saem da **dev**, com o padrão
_feature/nome-da-feature_, assim como no Gitflow. Já os **Bugs** são corrigidos
a partir da **release** e mergeados duas vezes: na própria **release** e
também em **dev**. Em casos extremos onde bugs são encontrados na **master**,
criamos branchs a partir da master com o padrão _hotfix/codigo-do-bug_, que
também são duplamente mergeados: de volta na **master** e também na **dev**.

Vejamos um exemplo do padrão descrito acima:

![padrão](https://s19.postimg.org/vmk7vzt9v/Token_Flow.png)

Temos três principais motivações para a utilização desse sistema, sendo eles
o controle das versões já publicadas, o controle de versões a serem testadas
pelo QA junto com a integração contínua e por último a representação
histórica do trabalho desenvolvido.

Primeiramente, queriamos ter um maior controle das versões que já foram
publicadas na loja. Antigamente, as branches se emaranhavam de maneira confusa
e era difícil achar dentro tantos commits aquele que foi utilizado para gerar a
build para a loja. Por sorte, utilizavamos na maior parte do tempo tags, mas
mesmo assim as taggs se confundiam com outras versões. Em sua nova proposta, a
**master** manteria apenas merges de versões prontas para a produção, e que
representam versões de release testadas. É muito mais facil localizar as versões
dessa maneira.

Segundo, tinhamos um grande problema em mãos: os QAs tinham que testar em branches
que estavam sempre em desenvolvimento, que as vezes podiam conter trabalhos
ainda não finalizados e features indesejadas. Além disso, testes unitários não
eram feitos automaticamente antes que alterações fossem mergeadas.
Foi justamente para resolver esses dois problemas que definimos que a branch
**release** fosse fixa. Assim poderiamos deixar nossa integração contínua
apontando para release, que iria rodar os testes unitários a cada merge.
Isso também facilitaria os testes de integração, uma vez que os QAs estariam
sempre apontando para uma branch própria para testes, utilizando apenas features
desejadas. Outra vantagem de possuir uma branch fixa para release é que podemos
resolver bugs específicos de cada versão. Por exemplo, suponha que temos as
features 1, 2, 3 e 4 em **dev** mas criamos uma release com as features 1,2 e 3 apenas.
Os QAs  iriam testar essa release, e se caso houvessem bugs, eles poderiam ser corrigidos
diretamente para aquela versão, não precisando se misturar à feature 4 que está
em dev para depois retornar à release.

Finalmente, assim como vimos anteriormente, as branchs podiam se tornar bem
confusas e ficar difícil de se ter uma representação histórica clara do que
realmente aconteceu no repositório. Vejamos um exemplo de onde a representação
histórica poderia ser melhor e, inclusive, nos ajudar a resolver possíveis
problemas de merge:

![branchs](https://s19.postimg.org/tkosev1mb/Screen_Shot_2017-04-27_at_19.04.43.png)

No exemplo acima, podemos ver que features se misturam com bugs, além de tags em
branchs diferentes. Os commits que começam com [MOB-xxxx] são commits de
correção de bugs, enquanto que os demais são features ou merges. Através do
commit "Merge branch 'development' into features" podemos ver que as branches
_development_ e _features_ andaram juntas até o "Ajustes de layout de recibo",
onde alguém em _development_ resolveu lançar uma release (Build#13). Porém, um bug
foi encontrado e corrigido na própria feature, o que forçou que uma nova versão
fosse lançada.



![nova versão](https://s19.postimg.org/5sfh3bzlf/download.jpg)
