# Curso de Docker para Desenvolvedores Java
Repositório utilizado para aprendizado do curso Docker - Hands On for Java Developers

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

Para listar todas as imagens que temos baixadas locamente podemos usar o seguinte comando:
```
docker image ls
```


## Rodando Containers

Para rodar containers basta executar o seguinte comando:

```
docker container run image-name
```
Isso fará um container ser criado a partir da imagem informada e o container criado será executado.

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
docker container ls -a # o parâmetro "a"significa all (todos), ou seja, listar todas os containers, até os que não estão rodando.
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
docker run -i -t ubuntu
```
ou 
```
docker run -it ubuntu
```

Dessa forma, ao rodar o container, seremos conectados ao bash do container e o container ficará rodando até sairmos do bash com o comando ``exit```.

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

## Docker Hub

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
