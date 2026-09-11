# ==============================================================================
# Multi-Stage Dockerfile para Spring Boot API REST (Java 17)
# Universidad ICESI - Ingeniería de Software V
# ==============================================================================

# Stage 1: Build y empaquetamiento del archivo JAR
FROM maven:3.9.6-eclipse-temurin-17-alpine AS builder
WORKDIR /app

# Descarga de dependencias en capa de caché
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Compilación y empaquetado del código fuente
COPY src ./src
RUN mvn clean package -DskipTests

# Stage 2: Imagen ligera de ejecución (JRE Alpine)
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app

# Seguridad: Ejecución con usuario no privilegiado
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

# Copia del artefacto JAR generado
COPY --from=builder /app/target/*.jar app.jar
RUN chown -R appuser:appgroup /app

USER appuser
EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]
