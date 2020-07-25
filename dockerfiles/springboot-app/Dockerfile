FROM openjdk:11-jre-slim

EXPOSE 8080

WORKDIR /usr/local/bin/

COPY my-service.jar webapp.jar

CMD ["java", "-Dspring.profiles.active=docker-demo", "-jar", "webapp.jar"]

LABEL maintainer="Ricardo Sander - ricardo.sander.lopes@gmail.com"