# =========================
# ETAPA 1: Build con Maven
# =========================
FROM maven:3.9.9-eclipse-temurin-17 AS build

WORKDIR /app

COPY pom.xml .
COPY .mvn .mvn
COPY mvnw .
COPY mvnw.cmd .

RUN chmod +x mvnw

RUN ./mvnw dependency:go-offline -B

COPY src ./src

RUN ./mvnw clean package -DskipTests


# =========================
# ETAPA 2: Runtime Java
# =========================
FROM alpine:3.19 as runtime

WORKDIR /app

RUN apk add --no-cache openjdk17-jre-headless


RUN addgroup -S appgroup && adduser -S appuser -G appgroup

COPY --from=build /app/target/*.jar app.jar

RUN chown -R appuser:appgroup /app

USER appuser

EXPOSE 8081

ENTRYPOINT ["java", "-jar", "app.jar"]