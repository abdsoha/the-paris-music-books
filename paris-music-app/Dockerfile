# ---------- Stage 1: Build ----------
FROM maven:3.9.6-eclipse-temurin-11 AS build

WORKDIR /app
COPY . .
RUN mvn clean package -DskipTests

# ---------- Stage 2: Runtime ----------
FROM eclipse-temurin:11-jre

WORKDIR /app
COPY --from=build /app/target/paris-music-app-1.0.jar app.jar

EXPOSE 9090

ENTRYPOINT ["java","-jar","app.jar"]

