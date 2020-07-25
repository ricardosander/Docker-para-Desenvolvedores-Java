[< Criando Imagens](8-CriandoImagens.md) | [Início](README.md) | [Criando uma Aplicação Spring Boot >](10-AplicacaoSpringBoot.md)

# Criando uma Aplicação Tomcat

## Preparação da Aplicação

Essa seção é um demonstrativo prático e rápido dos conceitos aprendidos até o momento. Vamos criar uma Imagem com uma aplicação Java instalada em um servidor Tomcat. Alguns detalhes desse processo envolvem conhecimentos específicos de Tomcat, Java e Maven.

Primeiramente, criamos um arquivo WAR com base em um projeto Java. Se estivermos utilizando Maven, por exemplo, bastaria rodar o seguinte comando:

```bash
mvn clean package
```

Após concluir a compilação, teremos um arquivo ```war``` no diretório ```target```. Vamos copiar esse ```war``` para o diretório onde vamos criar nosso Dockerfile e renomeá-lo para, digamos, ```my-service.war```.

## Imagem Base

Para criamos nossa Imagem, podemos facilitar o processo utilizando uma [Imagem Oficial Tomcat](https://hub.docker.com/_/tomcat). Precisamos selecionar uma imagem compatível com a versão do Tomcat e do JDK que queremos. No meu caso eu selecionei a versão 8.5.47-jdk11-openjdk, ou seja, uma Imagem com o Tomcat 8.5.47, Open JDK 11. Começamos a criação do Dockerfile com a seguinte linha:

```Dockerfile
FROM tomcat:8.5.47-jdk11-openjdk
```

## Portal do Tomcat

Conforme a documentação da Imagem que eu escolhi, a porta 8080 é utilizada pelo servidor Tomcat presente na Imagem. Por fins de documentação, adicionamos a seguinte linha no Dockerfile:

```Dockerfile
EXPOSE 8080
```

## Preparação da Imagem

É comum o servidor Tomcat vir com uma aplicação padrão para validação do seu funcionamento. Os arquivos da aplicação ficam no diretório ```usr/local/tomcat/webapps/```. Precisamos remover todos arquivos e diretórios dentro desse diretório. Porém, o ```WORKDIR``` dessa Imagem já é ```/usr/local/tomcat```. Então conseguimos resolver essa questão com a seguinte linha:

```Dockerfile
RUN rm -rf ./webapps/*
```

## Adicionando Aplicação à Imagem

A parte mais importante desse processo é adicionarmos nossa aplicação ```war``` à Imagem. Como já adicionamos ela no mesmo diretório do nosso Dockerfile, basta adicionar a seguinte linha:

```Dockerfile
COPY my-service.war /usr/local/tomcat/webapps/ROOT.war
```

O que irá copiar nosso arquivo da aplicação para dentro do diretório de aplicações do Tomcat, além de renomeá-lo como ```ROOT```, o que é um padrão para o servidor Tomcat.

## Variáveis de Ambiente

No caso da aplicação que estou usando, preciso adicionar uma variável de ambiente para identificar o ambiente no quala a aplicação irá rodar. Para isso utilizarei a seguinte linha:

```Dockerfile
ENV JAVA_OPTS="-Dspring.profiles.active=docker-demo"
```

## Executando o Tomcat

E por último, adicionamos a linha que irá efetivamente rodar o Tomcat quando o Container for executado:

```Dockerfile
CMD ["catalina.sh", "run"]
```

## Dockerfile Aplicação Tomcat

Nosso Dockerfile para a aplicação Tomcat ficou assim:

```Dockerfile
FROM tomcat:8.5.47-jdk11-openjdk

EXPOSE 8080

RUN rm -rf ./webapps/*

COPY my-service.war /usr/local/tomcat/webapps/ROOT.war

ENV JAVA_OPTS="-Dspring.profiles.active=docker-demo"

CMD ["catalina.sh", "run"]
```

Agora basta criar a Imagem:

```bash
docker image build -t my-tomcat-service .
```

E, por último, criar o Container

```bash
docker run -p 80:8080 -d my-tomcat-service
```

Pronto, criamos um Container com servidor Tomcat e nossa aplicação Java. Basta acessar, por exemplo, http://localhost e veremos nossa aplicação rodado.

[< Criando Imagens](8-CriandoImagens.md) | [Início](README.md) | [Criando uma Aplicação Spring Boot >](10-AplicacaoSpringBoot.md)