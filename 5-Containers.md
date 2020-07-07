[< Imagens](4-Imagens.md) | [Início](README.md) | [Comandos Docker >](6-ComandosDocker.md)

# Containers

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

[< Imagens](4-Imagens.md) | [Início](README.md) | [Comandos Docker >](6-ComandosDocker.md)
