# Curso de Docker para Desenvolvedores Java
Repositório utilizado para aprendizado do curso Docker - Hands On for Java Developers

[Teste 1.1](teste.md)
[Teste 2.1](teste.html)
[Teste 3.2](teste)

## Definições Iniciais

### Imagem
Uma Imagem é a entidade que construímos quando utilizamos Docker. Ela é a definição de um container.

### Container
Quando rodamos uma imagem Docker, temos um container. Ou seja, um container é a instância de uma imagem. Podemos ter diversas instâncias de uma imagem criadas ou rodando ao mesmo tempo, ou seja, diversos containers.

### Docker
Docker permite algo semelhante a uma virtualização mas de uma forma mais leve. Um container de Docker é um processo rodando sobre o Kernel Linux mas, diferente do que muitos pensam, ele não possui seu próprio Kernel ou Sistema Operacional. 

Quando rodamos um container com Ubuntu ou CentOS, por exemplo, o que estamos fazendo é rodar um processo, sobre o Kernel Linux da máquina hospedeira contendo as ferramentas e programas do sistema operacinal citado. O Kernel Linux é compartilhado entre o host e todos os containers que estão sendo rodados.

Docker é uma solução elegante para gerenciamento de containers baseado em uma conceito de 2008 chamado [LXC](https://en.wikipedia.org/wiki/LXC) (Linux Containers).

## Instalando Docker

### Habilitando Virtualização na BIOS
Caso você esteja usando sistemas operacionais Windows ou Mac e queira utilizar Docker nesses sistemas operacinais, o primeiro passo é verificar e habilitar o suporte a virtualização. Isso é realizado na BIOS do seu sistema e o processo depende de cada computador.

### Instalação
Para instalar Docker basta seguir os passos descritos [aqui](https://www.docker.com/get-started)

## Hello Docker
No Docker também temos nossa versão do "Hello World!", como nas linguagens de programação. Basta rodar o comando 
```
docker run hello-world
```
Caso seja a primeira vez que esteja rodando este comando, ele avisará que não existe uma imagem localmente e fará o download da imagem "hello-world". Após isso, um container será criado - a partir da imagem baixada - e executado, exibindo uma mensagem "Hello Docker!" com diversas informações introdutórias do Docker. Após isso, o container é finalizado.

## Baixando Imagens
Como dito anteriormente, uma imagem é a definição de um container. Para criar um container e pode executá-lo, precisamos de uma imagem para isso.

Para baixar imagens basta rodar o seguinte comando:
```
docker image pull owner-username/image-name
```

Para listar todas as imagens que temos baixadas localmente podemos usar o seguinte comando:
```
docker image ls
```

### Imagens e Versões

Na verdade, a referência completa para uma imagem é feita por três partes, não apenas duas. Uma referência completa seria:

```
owner-name/image-name:version
```

Ou seja, nome-proprietario/nome-imagem:versão. Quando não especificamos a versão, o Docker sempre irá considerar que estamos nos referindo a última versão (latest). Embora tenhamos usado esse recurso nos exemplos desse texto, não recomenda-se que isso seja feito pois pode levar a problemas de compatibilidade e instabilidade sem ao menos estarmos ciente das atualizações.

Por isso, recomendo que em uso real, ou quando encontrarmos problemas em uso de teste, sempre seja especificado a versão da imagem. 

## Rodando Containers

Para rodar containers basta executar o seguinte comando:

```
docker container run image-name
```
Isso fará um container ser criado a partir da imagem informada e o container criado será executado.

Quando um container é criado, ele também é executado. Muitas vezes, o comando que está sendo executado dentro do container fica preso ao nosso terminal. Existe um parâmetro para desanexar (detache) o comando do container do nosso terminal de forma que ele fique rodando "em background":

```
docker container run -d image-name
```

### Expondo portas

Por padrão, as portas de um container não estão acessíveis fora do mesmo. É comum querermos expor portas para acesso externo, como seria o caso de uma aplicação web. Para expor as portas de um container, utilizamos a seguinte opção:

```
-p external-port:container-port
```

Onde "p" significa "publish" (publicar) e está expondo a porta do container e fazendo-a ser acessível por uma porta da máquina local onde o comando foi rodado.

Exemplo:
```
docker container run -p 80:8080 my-image
```

Neste exemplo, estamos criando e rodando um container com base na imagem "my-image" e estamos expondo a porta 8080 do container e deixando acessível pela porta 80 da nossa máquina local.

### Listando e Parando Containers

Para visualizarmos todos os containers que temos rodando, basta rodar o seguinte comando:
```
docker container ls
```
ou
```
docker container ls -a # o parâmetro "a" significa all (todos), ou seja, listar todas os containers, até os que não estão rodando.
```
Ao listar o contrainer você perceberá que ele posui um id (CONTAINER_ID) e um nome (NAMES), que foram gerados quando ele foi criado.

Se quisermos parar um container que está rodando, basta rodar o seguinte comando, referenciando o id ou nome do container:
```
docker container stop id-or-name
```

### Ciclo de Vida de um Container 

Quando rodamos um container, ele executará um comando padrão. Algumas imagens, como a do Ubuntu, podem apenas rodar um comando e encerrar o container. No caso da imagem oficial do Ubuntu, teremos um comando /bin/bash. Como esse bash não está conectado a nenhum terminal e não tem nenhum script para rodar, ele acaba encerrando-se, o que finaliza o ciclo de vida do container.

Uma opção que podemos utilizar nesse caso é ativar o modo interativo (-i) e conectar nosso terminal ao bash do container (-t):
```
docker container run -i -t ubuntu
```
ou 
```
docker container run -it ubuntu
```

Dessa forma, ao rodar o container, seremos conectados ao bash do container e o container ficará rodando até sairmos do bash com o comando ```exit```.

### Iniciando e Reiniciando Containers

Já vimos como rodar um container e como pará-lo. Porém, ao parar um container ele não é destruído, ele apenas fica em um estado de desligado. Podemos rodar novamente o container com o seguinte comando:

```
docker container start id-or-name
```

Em alguns casos podemos querer reiniciar um container que está rodando, ou seja, pará-lo e iniciá-lo novamente. Para isso, usamos o seguinte comando:

```
docker container restart id-or-name
```

### Executando comandos e Logs

Quando queremos executar comandos em um container que estamos rodando, podemos executar o seguintes comando:

```
docker container exec id-or-name command
```

Um uso bem interessante desse recurso é rodar o comando bash dentro do container, assim poderemos rodar diversos outros comandos Unix lá dentro. Mas para tanto, não podemos esquecer de ativar o modo interativo e conectar o container ao nosso terminal com os parâmetros ```-it```:

```
docker container exec -it id-or-name bash
```

Outro recurso muito útil e importante é ver os logs do container, Os logs do container na verdade é apenas o output que está sendo gerado pelo "sistema" como um todo e pode ser visualizado da seguinte forma:

```
docker container logs ir-or-name # pode-se adicionar o parâmetro -f (follow) para seguirmos o log continuamente enquanto ele é gerado.
```

### Removendo containers

Um container que não está rodando ainda existe, no estado parado. Nesse estado ele ainda ocupa recursos, como memória e deveria ser removido caso não seja mais necessário. Uma forma de remover um container é com o seguinte comando:


```
docker container rm ir-or-name
```

Há também uma forma de remover todos os containers que estão parados, com o seguinte comando:

```
docker container prune
```

Esse comando exige uma confirmação e irá remover TODOS os containers parados do seu sistema.


## Comandos Docker : Clássico vs Modernos

Durante a evolução do Docker, decidiu-se migrar dos chamados "comandos clássicos" para os "comandos modernos". Isso foi uma reistruturação pois o Docker estava crescendo tanto que a quantidade de comandos estava começando a gerar confusão e desorganização.

O grande diferencial é que os comandos modernos seguem o padrão "docker recurso comando -parametros", ou seja, o comando será sempre precedido do recurso para o qual estamos rodando o comando. Exemplos de recursos são container e imagem.

Embora os comandos clássicos ainda funcionem normalmente, pode ser que eles venham a ser removidos com o tempo então recomenda-se a adotação dos comandos modernos.

Abaixo, seguem alguns exemplos de comandos clássicos (esquerda) versus comandos modernos (direita):

```
docker pull owner-name/imagem-name | docker image pull owner-name/image-name - baixando imagem
docker ps | docker container ls - listando containers 
docker images | docker image ls - listando imagens baixadas
```

## Repositório Docker e Docker Hub

O [Docker Hub](https://hub.docker.com/) é um respositório público de imagens para Docker. Ele é o repositório padrão mas pode-se criar repositórios privados e usá-los também.

Quando baixamos uma imagem ou rodamos um container (o que irá baixar a imagem caso ela não exista localmente ainda) é do repositório Docker que o Docker irá buscar a imagem.

Uma imagem Docker pode ser usada diretamente ou pode ser extendida e utilizada para criar outras imagens por terceiros. Uma imagem não pode ser alterada por terceiros.

## Criando Imagens

### Conceitos Básicos

Utilizando o Docker Hub, podemos não somente baixar e usar as imagens para rodar containers mas podemos usar as imagens existentes para criar nossas próprias imagens, utilizando o já existente nelas e adicionando o que precisamos.

Por exemplo, digamos que queiramos criar uma imagem para rodar uma aplicação Java em um sistema Ubuntu. Podemos começar utilizando a [imagem oficial do Ubuntu](https://hub.docker.com/_/ubuntu). Conseguimos extender essa imagem, adicionando um "layer" com o ambiente Java 11 - por exemplo, criando uma nova imagem. Essa nova imagem Ubuntu-Java11 que criaríamos poderia ser extendida, adicionando um novo layer, com a aplicação em si.

Desse modo teríamos uma imagem Ubuntu-Java11 que poderia ser utilizada na criação de diversas outras imagens de aplicações Java.

O Docker Hub possui diversas imagens públicas para uso mas é importante termos cuidado para utilizar imagens confiáveis que não contenham algum tipo de (Malware)[https://pt.wikipedia.org/wiki/Malware]. Uma imagem Docker do Docker Hub é composta de owner-name e image-name no seguinte formato: owner-name/image-name de forma que é simples identificar quem é o dono da imagem para identificar repositórios confiáveis. 

Existem também o conceito de "imagens oficiais" as quais são facilmente identificadas por terem uma tag "official" e não terem owner-name, como o exemplo da [imagem oficial do Ubuntu](https://hub.docker.com/_/ubuntu). Essas imagens oficias são criadas e mantidas por profissionais da equipe do Docker para uso da comunidade e são confiáveis.

### Criando Imagens a partir de um Container

Um dos métodos de criar uma nova imagem, é utilizar um Container. Esse não é a melhor forma e nem a mais usada, mas é uma forma importante de conhecer, principalmente pela simplicidade e pelos conceitos.

Digamos que queiramos criar uma imagem com Ubuntu e o ambiente de desenvolvimento Java e que essa imagem ainda não exista. Podemos baixar uma imagem do Ubuntu, rodar um container dessa imagem, fazer a instalação Java no container e criar uma nova imagem contendo a imagem do Ubuntu + as alterações que realizamos no container. Algo bem interessante é que criaremos um tipo de layer somente com as alterações e essa imagem usará a imagem original do Ubuntu, o que economizará espaço e banda, já que essa imagem do Ubuntu é partilhada com outras imagens.

Após realizar as alterações em um container, basta rodar o seguinte comando para criar uma imagem a partir dele:

```
docker container commit -a "Nome Completo do Autor email@do.autor" container-id-ou-nome nome-da-nova-imagem
```

Após rodar esse comando, uma imagem será criada a poderemos criar e rodar novos containers com base nessa imagem.

### Criando Imagem a partir de um Dockerfile

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
