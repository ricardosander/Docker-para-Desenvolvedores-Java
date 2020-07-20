FROM tomcat:8.5.47-jdk11-openjdk

EXPOSE 8080

RUN rm -rf ./webapps/*

COPY my-service.war /usr/local/tomcat/webapps/ROOT.war

ENV JAVA_OPTS="-Dspring.profiles.active=docker-demo"

CMD ["catalina.sh", "run"]

LABEL maintainer="Ricardo Sander - ricardo.sander.lopes@gmail.com"