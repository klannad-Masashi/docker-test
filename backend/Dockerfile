# Build stage
FROM gradle:8.5-jdk21 AS build

WORKDIR /app

# Copy gradle files
COPY gradle gradle
COPY gradlew .
COPY build.gradle .
COPY settings.gradle .

# Copy source code
COPY src src

# Build application
RUN gradle build -x test

# Runtime stage
FROM openjdk:21-jdk-slim

WORKDIR /app

# Install curl for health check
RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*

# Copy built jar from build stage
COPY --from=build /app/build/libs/webapp-0.0.1-SNAPSHOT.jar app.jar

# Expose port
EXPOSE 8080

# Run application
ENTRYPOINT ["java", "-jar", "app.jar"]