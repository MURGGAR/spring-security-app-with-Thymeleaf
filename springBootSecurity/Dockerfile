# Start with a base image containing Java runtime (AdoptOpenJDK)
FROM adoptopenjdk:17-jre-hotspot AS build

# Set the working directory in the image to "/app"
WORKDIR /app

# Copy the Maven wrapper files
COPY mvnw ./
COPY .mvn ./.mvn

# Copy the project Object Model (POM) and install the dependencies
COPY pom.xml .
RUN ./mvnw dependency:go-offline

# Copy the application source code
COPY src ./src

# Build the application with Maven
RUN ./mvnw package

# Set up a second stage for the final image
FROM adoptopenjdk:17-jre-hotspot

# Set the working directory to '/app'
WORKDIR /app

# Copy the compiled JAR file from the build stage
COPY --from=build /app/target/*.jar app.jar

# Set the startup command to execute the JAR
CMD ["java", "-jar", "app.jar"]
