[< Imagens](4-Imagens.md) | [Início](README.md) | [Comandos Docker >](6-ComandosDocker.md)

# Containers

## Rodando Containers

Para rodar Containers basta executar o seguinte comando:

```bash
docker container run image-name
```
Isso fará um Container ser criado a partir da Imagem informada e o Container criado será executado.

Quando um Container é criado, ele também é executado. Muitas vezes, o comando que está sendo executado dentro do container fica preso ao nosso terminal. Existe um parâmetro para desanexar (```detache```) o comando do Container do nosso terminal de forma que ele fique rodando "em background":

```bash
docker container run -d image-name
```

### Nomeando Containers

Quando criamos um Container, um id e um nome são automaticamente gerados para ele. Os comandos referentes aos Container se baseiam em sei nome OU id. Podemos usar o parâmetro ```--name``` para dar um nome ao Container e facilitar sua identificalçao:

```bash
docker container run -d --name nome-que-quero-para-o-container image-name
```

### Expondo portas

Por padrão, as portas de um Container não estão acessíveis fora do mesmo. É comum querermos expor portas para acesso externo, como seria o caso de uma aplicação web. Para expor as portas de um Container, utilizamos a seguinte opção:

```
-p external-port:container-port
```

Onde "p" significa "publish" (publicar) e está expondo a porta do Container e fazendo-a ser acessível por uma porta da máquina local onde o comando foi rodado.

Exemplo:
```bash
docker container run -p 80:8080 my-image
```

Neste exemplo, estamos criando e rodando um Container com base na imagem ```my-image``` e estamos expondo a porta 8080 do Container e deixando acessível pela porta 80 da nossa máquina local.

### Listando e Parando Containers

Para visualizarmos todos os Containers que temos rodando, basta rodar o seguinte comando:
```bash
docker container ls
```
ou
```bash
docker container ls -a # o parâmetro "a" significa all (todos), ou seja, listar todas os containers, até os que não estão rodando.
```
Ao listar o Contrainer você perceberá que ele posui um id (CONTAINER_ID) e um nome (NAMES), que foram gerados quando ele foi criado.

Se quisermos parar um Container que está rodando, basta rodar o seguinte comando, referenciando o id ou nome do container:
```bash
docker container stop id-or-name
```

Dica: não precisamos referenciar o id completo do Container em nenhum dos comados onde estes são usados. Basta informar os primeiros dígitos do id, suficientes para distinguir de outros Container com ids que comecem com mesmo dígitos. Geramente, 2 ou 3 dígitos são o suficiente.

### Ciclo de Vida de um Container 

Quando rodamos um Container, ele executará um comando padrão. Algumas Imagens, como a do Ubuntu, podem apenas rodar um comando e encerrar o Container. No caso da Imagem oficial do Ubuntu, teremos um comando /bin/bash. Como esse bash não está conectado a nenhum terminal e não tem nenhum script para rodar, ele acaba encerrando-se, o que finaliza o ciclo de vida do Container.

Uma opção que podemos utilizar nesse caso é ativar o modo interativo (-i) e conectar nosso terminal ao bash do container (-t):
```bash
docker container run -i -t ubuntu
```
ou 
```bash
docker container run -it ubuntu
```

Dessa forma, ao rodar o Container, seremos conectados ao bash do Container e o Container ficará rodando até sairmos do bash com o comando ```exit```.

### Iniciando e Reiniciando Containers

Já vimos como rodar um Container e como pará-lo. Porém, ao parar um Container ele não é destruído, ele apenas fica em um estado de desligado. Podemos rodar novamente o Container com o seguinte comando:

```bash
docker container start id-or-name
```

Em alguns casos podemos querer reiniciar um Container que está rodando, ou seja, pará-lo e iniciá-lo novamente. Para isso, usamos o seguinte comando:

```bash
docker container restart id-or-name
```

### Executando comandos e Logs

Quando queremos executar comandos em um Container que estamos rodando, podemos executar o seguintes comando:

```bash
docker container exec id-or-name command
```

Um uso bem interessante desse recurso é rodar o comando bash dentro do Container, assim poderemos rodar diversos outros comandos Unix lá dentro. Mas para tanto, não podemos esquecer de ativar o modo interativo e conectar o Container ao nosso terminal com os parâmetros ```-it```:

```bash
docker container exec -it id-or-name bash
```

Outro recurso muito útil e importante é ver os logs do Container, Os logs do Container na verdade é apenas o output que está sendo gerado pelo "sistema" como um todo e pode ser visualizado da seguinte forma:

```bash
docker container logs ir-or-name # pode-se adicionar o parâmetro -f (follow) para seguirmos o log continuamente enquanto ele é gerado.
```

### Removendo Containers

Um Container que não está rodando ainda existe, no estado parado. Nesse estado ele ainda ocupa recursos, como memória e deveria ser removido caso não seja mais necessário. Uma forma de remover um Container é com o seguinte comando:


```bash
docker container rm ir-or-name
```

Há também uma forma de remover todos os Containers que estão parados, com o seguinte comando:

```bash
docker container prune
```

Esse comando exige uma confirmação e irá remover TODOS os containers parados do seu sistema.

[< Imagens](4-Imagens.md) | [Início](README.md) | [Comandos Docker >](6-ComandosDocker.md)
