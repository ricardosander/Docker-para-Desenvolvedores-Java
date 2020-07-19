[< Repositório Docker e Docker Hub](7-DockerHub.md) | [Início](README.md)

# Criando Imagens

## Conceitos Básicos

Utilizando o Docker Hub, podemos não somente baixar e usar as Imagens para rodar Containers mas podemos usar as Imagens existentes para criar nossas próprias Imagens, utilizando o já existente nelas e adicionando o que precisamos.

Por exemplo, digamos que queiramos criar uma Imagem para rodar uma aplicação Java em um sistema Ubuntu. Podemos começar utilizando a [Imagem Oficial do Ubuntu](https://hub.docker.com/_/ubuntu). Conseguimos extender essa Imagem, adicionando um "layer" com o ambiente Java 11 - por exemplo, criando uma nova Imagem. Essa nova imagem Ubuntu-Java11 que criaríamos poderia ser extendida, adicionando um novo layer, com a aplicação em si.

Desse modo teríamos uma Imagem Ubuntu-Java11 que poderia ser utilizada na criação de diversas outras Imagens de aplicações Java.

O Docker Hub possui diversas Imagens públicas para uso mas é importante termos cuidado para utilizar Imagens confiáveis que não contenham algum tipo de (Malware)[https://pt.wikipedia.org/wiki/Malware]. Uma Imagem Docker do Docker Hub é composta de owner-name e image-name no seguinte formato: owner-name/image-name de forma que é simples identificar quem é o dono da Imagem para identificar repositórios confiáveis. 

Existem também o conceito de "Imagens Oficiais", as quais são facilmente identificadas por terem uma tag "official" e não terem owner-name, como o exemplo da [Imagem Oficial do Ubuntu](https://hub.docker.com/_/ubuntu). Essas Imagens oficias são criadas e mantidas por profissionais da equipe do Docker para uso da comunidade e são confiáveis.

## Criando Imagens a partir de um Container

Um dos métodos de criar uma nova Imagem, é utilizar um Container. Esse não é a melhor forma e nem a mais usada, mas é uma forma importante de conhecer, principalmente pela simplicidade e pelos conceitos.

Digamos que queiramos criar uma Imagem com Ubuntu e o ambiente de desenvolvimento Java e que essa Imagem ainda não exista. Podemos baixar uma Imagem do Ubuntu, rodar um Container dessa Imagem, fazer a instalação Java no Container e criar uma nova Imagem contendo a imagem do Ubuntu + as alterações que realizamos no Container. Algo bem interessante é que criaremos um tipo de layer somente com as alterações e essa Imagem usará a Imagem original do Ubuntu, o que economizará espaço e banda, já que essa Imagem do Ubuntu é partilhada com outras Imagens.

Após realizar as alterações em um Container, basta rodar o seguinte comando para criar uma Imagem a partir dele:

```bash
docker container commit -a "Nome Completo do Autor email@do.autor" container-id-ou-nome nome-da-nova-imagem
```

Após rodar esse comando, uma Imagem será criada a poderemos criar e rodar novos Containers com base nessa Imagem.

## Criando Imagem a partir de um Dockerfile

A criação de Imagem a partir de um Dockerfile é vantajosa pois ela é repetível. Ou seja, a partir de um mesmo Dockerfile eu posso regerar a mesma Imagem diversas vezes em diversos hosts. 

Um Dockerfile é um arquivo de texto, de mesmo nome, que deve ser colocado em um diretório próprio. Um exemplo de Dockerfile é o seguinte:

```Dockerfile
FROM ubuntu:latest

MAINTAINER Ricardo Sander "ricardo.sander.lopes@gmail.com"

RUN apt-get update && apt-get install -y openjdk-11-jdk

CMD ["/bin/bash"]
```

A primeira linha indica qual a Imagem base que usamos, que no caso seria a do Ubuntu, última versão. A segunda linha é apenas uma informação para identificarmos quem é o criador e mantenador da Imagem. A terceira linha apresenta quais os comandos são executados durante a criação da Imagem. A quarta e última linha define o comando que será executado ao rodarmos essa Imagem.

Após criarmos o Dockerfile, rodamos o seguinte comando para construirmos a Imagem:

```bash
docker image build -t nome-que-queremos-dar-a-imagem .
```

Onde o argumento ```-t``` indica o nome que queremos dar a Imagem e o argumento ```.``` é o diretório onde está nosso Dockerfile. No caso o uso de ```.``` indica o diretório atual.

Analisando os logs da criação da Imagem, podemos ver que cada linha se torna uma passo da criação, e que diversos layers vão sendo criados durante a construção da Imagem. Esses layers são camadas da Imagem, assim como a Imagem Ubuntu é uma das camadas. Ao modificamos o Dockerfile e rodar novamente o comando de construção podemos ver que os passos que não houveram alteração não são executados pois a camada criada anterioemente pode ser reutilizada.

### Comando MAINTAINER Descontinuiado

Atualmente o comando ```MAINTAINER``` consta na documentação do Docker como descontinuado. Isso significa que o comando pode deixar de existir a qualquer momento e que não é recomendado a criação de novas Imagens com ele.

Mesmo assim, esse comando é amplamente utilizado em Imagens que já existem e é importante ao menos conhecê-lo para saber do que se trata.

Existe uma alternativa para esse comando que é o uso de "labels" que será mencionado posteriomente.

## Copiando arquivos para a Imagem

Já aprendemos como criar uma Imagem básica, rodando comandos para alterá-la, e definindo o comando executado ao rodar essa Imagem, ao criar o Container. Porém, para podermos criar Imagens com nossas próprias aplicações, precisaremos conseguir transferir nossa aplicação para dentro da Imagem.

Podemos fazer isso utilizando o comando ```COPY```, conforme a 3ª linha do exemplo a seguir:

```Dockerfile
FROM ubuntu:latest

RUN apt-get update && apt-get install -y openjdk-11-jdk

COPY my-service.jar /usr/local/bin

CMD ["/bin/bash"]
```

Esse comando copia um arquivo local (primeiro argumento) para um diretório dentro da Imagem (segundo argumento). No exemplo a cima, estamos copiando um arquivo jar para o diretório /usr/local/bin/ da Imagem.

É importante salientar que somente arquivos no diretório do Dockerfile e em seus subdiretórios serão visíveis para execução do comando ```COPY```. Lembra quando definimos o diretório (com o argumento ```.```) onde o Dockerfile estava localizado para criar a Imagem? Esse diretório é considerado o escopo da criação da Imagem e nenhum diretório fora dele será visível durante a criação da Imagem.

Inclusive, quando construirmos uma Imagem de um Dockerfile, o primeiro retorno no terminal que normalmente pode ser visto é a seguinte:

```bash
Sending build context to Docker daemon  212.6MB
```

Esse comando indica que um contexto está sendo criado pelo Docker e que os recursos (arquivos e diretórios) estão sendo carregados. Por isso, é uma boa prática, não ter o Dockerfile na raiz de um projeto e sim em um diretório apenas com os recursos necessários para criação da Imagem.

### Diretório de Trabalho da Imagem

Antes de recriarmos nossa Imagem, ainda podemos aprimorar um pouco mais nosso Dockerfile. Podemos definir qual o "diretório de trabalho" da Imagem. Isso define onde será o ponto de entrada da Imagem e facilitará outros comandos, como o ```COPY``` pois não precisaremos definir todo o caminho. O comando para definir esse diretório é o ```WORKDIR``` e, caso não seja definido, será a raiz da Imagem ```/```.

```Dockerfile
FROM ubuntu:latest

RUN apt-get update && apt-get install -y openjdk-11-jdk

WORKDIR /usr/local/bin

COPY my-service.jar .

CMD ["/bin/bash"]
```

Além de simplificarmos o comando de copiar o arquivo, outros comandos serão simplificados graças a esse comando e, quando acessarmos o Container, estaremos no diretório definido.

Agora basta rodarmos o comando para construir nossa Imagem e rodarmos ela em um Container:

```bash
docker image build -t docker-for-java .
docker container run -it docker-for-java
```

Após rodarmos ambos os comandos, teremos uma nova Imagem, com o arquivo java dentro dela e estaremos com nosso terminal conectado ao Container, no diretório de trabalho especificado.

### ADD como alternativa ao COPY

Na documentação do Docker há uma alternativa ao comando ```COPY``` que é o ```ADD```. A documentação dos dois comandos é bem similar e ```ADD``` parece fazer tudo que o ```COPY``` com alguns detalhes extras como possibilidar adicionar uma URL como origem e descompactar arquivos compactados.

A convenção é usarmos o ```COPY``` para o básico e deixarmos o ```ADD``` para os casos específicos e avançados.

## Comando para Execução do Container

Falamos brevemente sobre o que o comando ```CMD``` faz mas, até o momento, ele não nos foi verdadeiramente útil. Esse comando define qual vai ser o comando executado pelo Container na hora que este for executado. Para nosso Container faz todo sentido que o Container execute o arquivo jar que colocamos lá dentro. Dessa forma, nosso Dockerfile fica assim:

```Dockerfile
FROM ubuntu:latest

RUN apt-get update && apt-get install -y openjdk-11-jdk

WORKDIR /usr/local/bin

COPY my-service.jar .

CMD ["java", "-jar", "my-service.jar"]
```

Existem 3 maneiras de escrever esse comando, a forma demonstrada é chamada ```exec form``` e é considerada a preferível na documentação do Docker. A ```exec form``` é um array de strings (com aspas duplas) onde o primeiro elemento é o executável/comando e os demais são os parâmetros.

Após cronstruir a Imagem e criarmos um Container (no modo detach):

```bash
docker image build -t docker-for-java .
docker container run -d docker-for-java
```

Teremos um Container rodando, em "background" e executando a aplicação Java. Podemos usar os comandos aprendidos anteriormente para listar Containers, parar, iniciar e ver os logs.


### ENTRYPOINT como alternativa ao CMD

Existe um comando muito semelhante ao ```CMD``` que é o ```ENTRYPOINT```. A diferença entre eles é que o que definimos no ```CMD``` é o comando inicial padrão para o Container, enquanto o definido no ```ENTRYPOINT``` é o comando obrigatório. Ou seja, ao criarmos um Container de uma Imagem com ```CMD```, podemos adicionar o comando que queremos que seja rodado enquanto ao criarmos um Container de uma Imagem com ```ENTRYPOINT``` qualquer comando adicionado a criação do Container será ignorado e substituído pelo comando definido no ```ENTRYPOINT```.

## Labels e Meta Dados

Conforme mencionado na seção sobre descontinuamento do comando ```MAINTAINER```, a construção de Imagens nos permite criar labels para adicionar meta dados sobre a nossa imagem. O comando recebe uma chave e um valor e pode ser utilizado para adicionar qualquer tipo de informação sobre a Imagem, como mantenedor, versão, data de criação, etc.

Uma dica interessante é que podemos adicionar o comando ao final do Dockerfile e isso nos ajuda a, caso precisemos alterar essas labels, a construção da Imagem reaproveite todas as etapas anteriores da construção e refaça somente a parte referente as labels. Se usarmos esse comando no meio do arquivo e precisarmos mudar, tudo que foi definido posteriormente precisará ser reconstruído.

Exemplo de uso do comando label, definindo mantenedor e versão:

```Dockerfile
FROM ubuntu:latest

RUN apt-get update && apt-get install -y openjdk-11-jdk

WORKDIR /usr/local/bin

COPY my-service.jar .

CMD ["java", "-jar", "my-service.jar"]

LABEL maintainer="Ricardo Sander - ricardo.sander.lopes@gmail.com"
LABEL version=v0
```

## O Comando EXPOSE

Existe um comando muito utilizado em Dockerfiles que é o comando ```EXPOSE```. Embora muitos acreditem que este comando sirva para expor ou publicar as portas do Container criado com a Imagem, na verdade, esse comando serve apenas como uma boa prática de engenharia para documentar para quem venha a utilizar aquela Imagem que a portas mencionadas no ```EXPOSE``` estarão sendo usadas pelo Container. Para que a porta seja acessível, é necessário publicar a porta conforme mostrado na seção ```Expondo Portas no capítulo 5```.

Exemplo de uso do ```EXPOSE```

```Dockerfile
FROM ubuntu:latest

EXPOSE 8080

CMD ["/bin/bash"]
```

## Variáveis de Ambiente com ENV

Variáveis de ambiente são conjuntos chave-valor utilizados para passar valores para o Container usar em tempo de execução. Definimos uma variável de ambiente utilizando o comando ```ENV``` seguido da chave e valor. Tudo que vier após o espaço após a chave é considerado parte do valor mas caracteres especiais, como aspas, precisam ser escapados.

```Dockerfile
ENV minhaVariavel Valor da Minha Variável
```

Outra forma de usar esse comando é o seguinte:

```Dockerfile
ENV minhaVariavel="Valor da Minha Variável"
```

Exemplo de uso em um Dockerfile:

```Dockerfile
FROM ubuntu:latest

ENV JAVA_OPTS="-Dspring.profiles.active=docker-demo"

CMD ["/bin/bash"]
```

No exemplo acima é definido uma variável de ambiente chamada JAVA_OPTS. O valor dessa variável é tudo que está entre as aspas.

Uma variável de ambiente pode ser definida ao criarmos o Container da Imagem, e esse valor será persistido no Container.

```bash
docker container run --env myDatabasePassword=pass123 my-database-image
```

Toda vez que o Container criado no comando de exemplo a cima for iniciado o reiniciado, o valor da variável de ambiente será o mesmo que o definino quando este foi criado. Esse recurso é muito importante para a definição de variáveis customizáveis e sensíveis, como senhas, por exemplo.

[< Repositório Docker e Docker Hub](7-DockerHub.md) | [Início](README.md)
