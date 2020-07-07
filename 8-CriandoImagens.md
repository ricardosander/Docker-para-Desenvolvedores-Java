[< Repositório Docker e Docker Hub](7-DockerHub.md) | [Início](README.md)

# Criando Imagens

## Conceitos Básicos

Utilizando o Docker Hub, podemos não somente baixar e usar as imagens para rodar containers mas podemos usar as imagens existentes para criar nossas próprias imagens, utilizando o já existente nelas e adicionando o que precisamos.

Por exemplo, digamos que queiramos criar uma imagem para rodar uma aplicação Java em um sistema Ubuntu. Podemos começar utilizando a [imagem oficial do Ubuntu](https://hub.docker.com/_/ubuntu). Conseguimos extender essa imagem, adicionando um "layer" com o ambiente Java 11 - por exemplo, criando uma nova imagem. Essa nova imagem Ubuntu-Java11 que criaríamos poderia ser extendida, adicionando um novo layer, com a aplicação em si.

Desse modo teríamos uma imagem Ubuntu-Java11 que poderia ser utilizada na criação de diversas outras imagens de aplicações Java.

O Docker Hub possui diversas imagens públicas para uso mas é importante termos cuidado para utilizar imagens confiáveis que não contenham algum tipo de (Malware)[https://pt.wikipedia.org/wiki/Malware]. Uma imagem Docker do Docker Hub é composta de owner-name e image-name no seguinte formato: owner-name/image-name de forma que é simples identificar quem é o dono da imagem para identificar repositórios confiáveis. 

Existem também o conceito de "imagens oficiais" as quais são facilmente identificadas por terem uma tag "official" e não terem owner-name, como o exemplo da [imagem oficial do Ubuntu](https://hub.docker.com/_/ubuntu). Essas imagens oficias são criadas e mantidas por profissionais da equipe do Docker para uso da comunidade e são confiáveis.

## Criando Imagens a partir de um Container

Um dos métodos de criar uma nova imagem, é utilizar um Container. Esse não é a melhor forma e nem a mais usada, mas é uma forma importante de conhecer, principalmente pela simplicidade e pelos conceitos.

Digamos que queiramos criar uma imagem com Ubuntu e o ambiente de desenvolvimento Java e que essa imagem ainda não exista. Podemos baixar uma imagem do Ubuntu, rodar um container dessa imagem, fazer a instalação Java no container e criar uma nova imagem contendo a imagem do Ubuntu + as alterações que realizamos no container. Algo bem interessante é que criaremos um tipo de layer somente com as alterações e essa imagem usará a imagem original do Ubuntu, o que economizará espaço e banda, já que essa imagem do Ubuntu é partilhada com outras imagens.

Após realizar as alterações em um container, basta rodar o seguinte comando para criar uma imagem a partir dele:

```
docker container commit -a "Nome Completo do Autor email@do.autor" container-id-ou-nome nome-da-nova-imagem
```

Após rodar esse comando, uma imagem será criada a poderemos criar e rodar novos containers com base nessa imagem.

## Criando Imagem a partir de um Dockerfile

A criação de Imagem a partir de um Dockerfile é vantajosa pois ela é repetível. Ou seja, a partir de um mesmo Dockerfile eu posso regerar a mesma imagem diversas vezes em diversos hosts. 

Um Dockerfile é um arquivo de texto de mesmo nome que deve ser colocado em um diretório próprio. Um exemplo de Dockerfile é o seguinte:

```
FROM ubuntu:latest

MAINTAINER Ricardo Sander "ricardo.sander.lopes@gmail.com"

RUN apt-get update && apt-get install -y openjdk-11-jdk

CMD ["/bin/bash"]
```

A primeira linha indica qual a Imagem base que usamos, que no caso seria a do Ubuntu, última versão. A segunda linha é apenas uma informação para identificarmos quem é o criador e mantenador da imagem. A terceira linha apresenta quais os comandos são executados durante a criação da Imagem. A quarta e última linha define o comando que será executado ao rodarmos essa Imagem.

Após criarmos o Dockerfile, rodamos o seguinte comando para construirmos a Imagem:

```
docker imagem build -t nome-que-queremos-dar-a-imagem .
```

Onde o argumento ```-t``` indica o nome que queremos dar a Imagem e o argumento ```.```é o diretório onde está nosso Dockerfile. No caso o uso de ```.``` indica o diretório atual.

Analisando os logs da criação da Imagem, podemos ver que cada linha se torna uma passo da criação, e que diversos layers vão sendo criados durante a construção da Imagem. Esses layers são camadas da Imagem, assim como a Imagem Ubuntu é uma das camadas. Ao modificamos o Dockerfile e rodar novamente o comando de construção podemos ver que os passos que não houveram alteração não são executados pois a camada criada anterioemente pode ser reutilizada.

[< Repositório Docker e Docker Hub](7-DockerHub.md) | [Início](README.md)
