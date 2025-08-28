# ================================
# Stage 1: Build the JAR with Maven
# ================================
FROM maven:3.9.6-eclipse-temurin-17 AS builder

# Set working directory inside the container
WORKDIR /app

# Copy Maven config first (for dependency caching)
COPY pom.xml .

# Download dependencies (offline mode)
RUN mvn dependency:go-offline -B

# Copy the rest of the backend source code
COPY src ./src

# Build the Spring Boot application (skip tests for speed)
RUN mvn clean package -DskipTests

# ================================
# Stage 2: Run the Application
# ================================
FROM eclipse-temurin:17-jdk

# Set working directory
WORKDIR /app

# Copy the JAR from the builder stage
COPY --from=builder /app/target/*.jar app.jar

# Expose backend port
EXPOSE 8080

# Run the Spring Boot application
ENTRYPOINT ["java", "-jar", "app.jar"]
