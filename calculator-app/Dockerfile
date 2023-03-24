# Use the official Maven image as the base image
FROM maven:3.8.4-openjdk-11 as builder

# Set the working directory
WORKDIR /app

# Copy the pom.xml into the working directory
COPY pom.xml .

# Download dependencies separately to cache them
RUN mvn dependency:go-offline

# Copy the source code into the working directory
COPY src/ ./src/

# Build the application using Maven
RUN mvn clean package

# Use the official OpenJDK image as the base image for the runtime
FROM openjdk:11-jre-slim

# Set the working directory
WORKDIR /app

# Copy the built application from the builder stage
COPY --from=builder /app/target/Calculator-1.0-SNAPSHOT.jar ./Calculator.jar

# Expose a port if your application requires it (optional)
EXPOSE 3000


# Use the official Tomcat image as the base image for the final image
FROM tomcat:9.0.55-jdk8-openjdk-slim-buster

# Copy the WAR file from the build stage into the Tomcat webapps directory
COPY --from=builder /app/target/Calculator-1.0-SNAPSHOT.jar /usr/local/tomcat/webapps/

# Start the application on launch
CMD ["catalina.sh", "run"]

