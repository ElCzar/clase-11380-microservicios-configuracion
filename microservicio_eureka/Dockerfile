# Use OpenJDK 21 as the base image
FROM eclipse-temurin:21-jdk-slim

# Set working directory
WORKDIR /app

# Copy Maven wrapper and configuration files
COPY mvnw .
COPY mvnw.cmd .
COPY pom.xml .

# Copy the .mvn directory (contains wrapper properties)
COPY .mvn .mvn

# Make Maven wrapper executable
RUN chmod +x ./mvnw

# Copy source code
COPY src ./src

# Build the application (skip tests for faster build)
RUN ./mvnw clean package -DskipTests

# Create a new stage for runtime
FROM eclipse-temurin:21-jdk-slim

# Set working directory
WORKDIR /app

# Copy the JAR file from the build stage
COPY --from=0 /app/target/microservicio_eureka-*.jar app.jar

# Expose the port that Eureka server runs on
EXPOSE 8761

# Set JVM options for containerized environment
ENV JAVA_OPTS="-Xmx512m -Xms256m"

# Run the application
ENTRYPOINT ["sh", "-c", "java $JAVA_OPTS -jar app.jar"]