FROM ubuntu:latest

RUN apt-get update && apt-get install -y openjdk-11-jdk

WORKDIR /usr/local/bin

COPY my-service.jar .

CMD ["java", "-jar", "my-service.jar"]

LABEL maintainer="Ricardo Sander - ricardo.sander.lopes@gmail.com"