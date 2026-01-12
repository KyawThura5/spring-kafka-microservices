# Stage 1: Build everything
FROM maven:3.9.6-eclipse-temurin-21 AS build
WORKDIR /build

# Copy the shared library and INSTALL it to the local container repo
COPY base-domain /build/base-domain
RUN mvn -f /build/base-domain/pom.xml clean install -DskipTests

# Copy and build the email-service
COPY email-service /build/email-service
WORKDIR /build/email-service
RUN mvn clean package -DskipTests

# Stage 2: Run
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app
COPY --from=build /build/email-service/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]