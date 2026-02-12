FROM eclipse-temurin:11-jre


WORKDIR /app

COPY target/paris-music-app-1.0.jar app.jar

EXPOSE 9090

ENTRYPOINT ["java","-jar","app.jar"]

