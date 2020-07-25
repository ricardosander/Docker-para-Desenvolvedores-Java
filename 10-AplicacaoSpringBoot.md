[< Criando uma Aplicação Tomcat](9-AplicacaoTomcat.md) | [Início](README.md)

# Criando uma Aplicação Spring Boot

Na seção anterior, vimos os passos para criarmos uma Imagem Docker com uma aplicação web Java utilizando o servidor Tomcat. Atualmente, porém, é muito mais comum adotarmos uma abordagem mais moderna de desenvolvimento e desenvolver aplicações Java utilizando SpringBoot, a qual já roda o servidor embutido na aplicação, como um servidor Tomcat, por exemplo. Além disso, aplicações assim podem ser compiladas para um arquivo jar, o qual pode ser rodado diretamente na linha de comando, sem necessidade de outra aplicação ou comando para executá-la.

A criação dessa Imagem será bem mais simples. Começamos procurando a Imagem base certa para ela, pois não necessitamos mais de uma imagem contendo o Tomcat. Se buscarmos por Imagens "Java" ou "OpenJDK" no Docker Hub, podemos notar que seremos redirecionados para a mesma Imagem, a OpenJDK. Essa imagem oficinal possui diversas versões do Java, versões slim (reduzidas, com menos ferramentas), versões com o JDK e versões contendo apenas o JRE. Vou escolher usar a versão slim, para ser a mais leve, e a versão contendo apenas o JRE, pois a aplicação será apenas executada dentro da imagem, não compilada. Além disso, vou escolher a versão 11 do Java.

Começamos nosso Dockerfile assim então:

```Dockerfile
FROM openjdk:11-jre-slim
```

Vamos precisa publicar a porta 8080. Além disso, como não será mais uma aplicação rodando no Tomcat, vamos escolher um diretório diferente. O diretório será o ```/usr/local/bin/```, onde normalmente ficam os executárveis do sistema. Vamos definir esse diretório como o diretório de trabalho dentro da Imagem, para simplicar os demais commandos.

```Dockerfile
EXPOSE 8080

WORKDIR /usr/local/bin/
```

Após isso, vamos copiar nosso arquivo jar para dentro da Imagem. Como não precisamos mais seguir o padrão do Tomcat para o nome do arquivo, podemos colocar um nome mais simples e amigável.

```Dockerfile
COPY my-service.jar webapp.jar
```

Para executarmos uma aplicação Spring Boot, o comando seria o seguinte, já adicionando a variável de ambiente necessária para o meu exemplo:

```bash
java -Dspring.profiles.active=docker-demo -jar webapp.jar
```

Traduzimos esse comando para o Dockerfile da seguinte forma, conforme seções anteriores:

```Dockerfile
CMD ["java", "-Dspring.profiles.active=docker-demo", "-jar", "webapp.jar"]
```

Ao juntarmos todas estas etapas, teremos o seguinte Dockerfile:

```Dockerfile
FROM openjdk:11-jre-slim

EXPOSE 8080

WORKDIR /usr/local/bin/

COPY my-service.jar webapp.jar

CMD ["java", "-Dspring.profiles.active=docker-demo", "-jar", "webapp.jar"]
```

Agora basta criar a Imagem:

```bash
docker image build -t my-web-service .
```

E, por último, criar o Container

```bash
docker run -p 80:8080 -d my-web-service
```

Pronto, criamos um Container rondado um serviço Java (Spring Boot), com Tomcat embutido na aplicação. Basta acessar, por exemplo, http://localhost e veremos nossa aplicação rodado.


[< Criando uma Aplicação Tomcat](9-AplicacaoTomcat.md) | [Início](README.md)