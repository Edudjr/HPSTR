---
layout: post
title: Um novo padrão de branching para testar em seus projetos
description: "Um novo padrão de branching para testar em seus projetos"
modified: 2016-05-14T15:27:45-03:00
tags: [GIT, Branch, padrão, versionamento, No Fast Forward]
image:
  url: https://s19.postimg.cc/mxaae08oj/blog-post7.jpg
---
Testando um novo padrão de controle de versionamento com a ajuda do _No Fast Forward_.

Recentemente, discutimos no trabalho um padrão para utilização de branchs que
se adequasse às nossas necessidades internas. Chegamos à uma proposta
parecida com
a do **Gitflow**, porém, com algumas mudanças para tornar mais fácil
o trabalho dos QAs, e também de integração contínua, em nossos projetos.

A principal mudança é que a branch **release** nunca morre. Ela não começa
a partir da **dev** como no Gitflow, mas sim a partir do commit inicial, assim
como a **master** e a própria dev. Essas três branchs possuem algumas
características importantes: elas nunca morrem e nunca são commitadas
diretamente. Ou seja, possuem apenas merges.

Resumidamente, neste modelo, a branch **master** irá conter **somente** merges
de **releases** já testadas, prontas para a produção, e refletem todas as
versões disponibilizadas publicamente, devidamente _taggeadas_. O
desenvolvimento de novas features é feito em branchs que saem da **dev**, com o padrão
_feature/nome-da-feature_, assim como no Gitflow. Já os **Bugs** são corrigidos
a partir da **release** e mergeados duas vezes: na própria **release** e
também em **dev**. Em casos onde bugs são encontrados na **master**,
criamos branchs a partir da master com o padrão _hotfix/codigo-do-bug_, que
também são duplamente mergeados: de volta na **master** e também na **dev**.

Vejamos um exemplo do padrão descrito acima:

![padrão](https://s19.postimg.cc/vmk7vzt9v/Token_Flow.png)

Temos três principais motivações para a utilização desse sistema, sendo eles
o controle das versões já publicadas, o controle de versões a serem testadas
pelo QA junto com a integração contínua e, por último, a representação
histórica do trabalho desenvolvido.

Primeiramente, queriamos ter um maior controle das versões que já foram
publicadas na loja. Em projetos onde não há um padrão de branching a ser seguido, as branches podem se emaranhar de maneira confusa, sendo difícil de achar dentre tantos commits aquele que foi utilizado para gerar a
build para a loja. É possível utilizar tags para marcar as versões de release, mas
mesmo assim as tags podem se confundir com outras versões. Em sua nova proposta, a
**master** manteria apenas merges de versões prontas para a produção e que
representam versões de releases testadas. É muito mais facil localizar as versões
dessa maneira.

Segundo, tinhamos um problema em mãos: os QAs tinham que testar em branches
que estavam sempre em desenvolvimento, que as vezes podiam conter trabalhos
ainda não finalizados e features indesejadas para determinada release. Além disso, testes unitários não
eram feitos automaticamente antes que alterações fossem mergeadas.
Foi justamente para resolver esses dois problemas que definimos que a branch
**release** fosse fixa. Assim poderíamos deixar nossa integração contínua
apontando para release, que iria rodar os testes unitários a cada merge.
Isso também facilitaria os testes de integração, uma vez que os QAs estariam
sempre apontando para uma branch própria para testes, utilizando apenas features
desejadas. Outra vantagem de possuir uma branch fixa para release é que podemos
resolver bugs específicos de cada versão. Por exemplo, suponha que temos as
features 1, 2, 3 e 4 em **dev** mas criamos uma release com as features 1,2 e 3 apenas.
Os QAs  iriam testar essa release, e se caso houvesse bugs, eles poderiam ser corrigidos
diretamente naquela versão, não precisando se misturar à feature 4 que está
em dev para depois retornar à release.

Finalmente, assim como vimos anteriormente, as branchs podiam se tornar bem
confusas e ficar difícil de se ter uma representação histórica clara do que
realmente aconteceu no repositório. É fácil se perder nos commits caso não haja
um padrão no repositório onde várias pessoas contribuem para o mesmo. Bugfixes podem se
confundir com features que podem se confundir com releases, a não ser que
a descrição dos commits seja muito bem detalhada. Utilizando este padrão é fácil
de se identificar as branches principais e cada merge é representado
por um novo commit.


Uma coisa que podemos notar neste novo modelo é que ele possui mais commits. Por outro lado, conseguimos ver claramente o que são releases
públicas, o que são releases internas (de QA), o que são bugs resolvidos e o que
são features. Outra vantagem desse modelo é que conseguimos facilmente remover
qualquer funcionalidade da dev e gerar uma nova release se for preciso, basta
reverter o merge da funcionalidade. Também podemos facilmente reverter uma release,
apenas desfazendo o ultimo merge, voltando ao estado anterior.

### Não se esqueça do Fast Forward
Lembre-se que, quando utilizado o Git na linha de comando, o recurso "fast forward"
é aplicado como padrão! Caso não saiba o que é isso, imagine o seguinte cenário:
você está na branch Master e cria uma nova branch a partir dela, chamada **feature/login**.
Na nova branch, você realiza um commit com algumas alterações.
![branch1](https://s19.postimg.cc/6zwt9be9f/Capture1.png)

Agora, de volta na
Master, você pretende mergear as alterações da nova branch. Se você apenas
digitar `git pull . feature/login` ou `git merge feature/login`, por não haver
nenhum outro commit na Master, o git irá simplesmente igualar a Master com a
feature/login, sem criar um novo commit para o merge.
![branch2](https://s19.postimg.cc/6ofcwjxtf/Capture2.png)

Para que você aplique o modelo discutido acima, é necessário que os commits das 3 branches
principais (master, release e dev) sejam feitos por Pull Requests (que não utiliza Fast Forward) ou então através da
flag "--no-ff", assim:  
`git pull . feature/login --no-ff` ou `git merge feature/login --no-ff`  
Deste modo, um novo commit será criado simbolizando o merge das duas branches.

![branch3](https://s19.postimg.cc/gn0bj178z/Capture3.png)


Espero que este modelo também seja útil para você ou que te inspire a melhorar a
forma como trabalha atualmente com branches. Ficamos por aqui! :)
