FROM amd64/openjdk:8
EXPOSE 8080
ADD target/my-app-1.0-SNAPSHOT.jar docker-javamavenapp.jar
ENTRYPOINT ["java","-ja","/docker-javamavenapp.jar"]