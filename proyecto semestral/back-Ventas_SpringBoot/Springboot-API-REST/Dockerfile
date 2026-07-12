# =========================
# ETAPA 1: Build con Maven (Soporta Multi-plataforma nativamente)
# =========================
# Cambiamos la imagen pesada de Maven a una basada en Ubuntu/Debian que sí tiene soporte ARM64.
# Nota: Seguimos usando ./mvnw abajo, así que esta imagen solo provee el entorno Java inicial para ejecutarlo.
# =========================
# ETAPA 1: Build con Maven (Soporta Multi-plataforma nativamente)
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
# ETAPA 2: Runtime Java Ligero (Alpine Puro - ¡Soporta Multi-plataforma!)
# =========================
# Usamos Alpine puro como base, el cual tiene soporte nativo perfecto para ARM64 y AMD64
FROM alpine:3.19 AS runtime

WORKDIR /app

# Instalamos el JRE de OpenJDK 17 nativo desde los repositorios de Alpine
RUN apk add --no-cache openjdk17-jre-headless

# Mantenemos intacta tu lógica de seguridad no-root original
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

COPY --from=build /app/target/*.jar app.jar

RUN chown -R appuser:appgroup /app

USER appuser

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]
