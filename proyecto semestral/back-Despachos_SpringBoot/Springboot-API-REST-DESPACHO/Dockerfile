# =========================
# ETAPA 1: Build con Maven (Corregida sin Wrapper)
# =========================
FROM maven:3.9.9-eclipse-temurin-17 AS build

WORKDIR /app

# Copiamos el pom.xml para descargar las dependencias usando el maven global
COPY pom.xml .

# Descarga las dependencias en caché (reemplaza a go-offline del wrapper)
RUN mvn dependency:resolve-plugins dependency:go-offline -B

# Copiamos el código fuente
COPY src ./src

# Compilamos usando el comando 'mvn' global en lugar de './mvnw'
RUN mvn clean package -DskipTests

# =========================
# ETAPA 2: Runtime Java (Se queda igual, solo corregimos el 'AS')
# =========================
FROM alpine:3.19 AS runtime

WORKDIR /app

RUN apk add --no-cache openjdk17-jre-headless

RUN addgroup -S appgroup && adduser -S appuser -G appgroup

COPY --from=build /app/target/*.jar app.jar

RUN chown -R appuser:appgroup /app

USER appuser

EXPOSE 8081

ENTRYPOINT ["java", "-jar", "app.jar"]