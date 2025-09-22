# ----- STAGE 1: Build Jar File ------
FROM maven:3.9.11-eclipse-temurin-8-noble AS builder

WORKDIR /app

COPY mvnw mvnw.cmd ./
COPY pom.xml .
COPY src ./src

RUN mvn install

# # ----- STAGE 2: Run jar file ------
FROM openjdk:8-jdk-alpine 
WORKDIR /app
# Environment Variable that defines the endpoint of sentiment-analysis python api.
ENV SA_LOGIC_API_URL http://localhost:5000
COPY --from=builder /app/target/sentiment-analysis-web-0.0.1-SNAPSHOT.jar .
EXPOSE 8080
CMD ["java", "-jar", "sentiment-analysis-web-0.0.1-SNAPSHOT.jar", "--sa.logic.api.url=${SA_LOGIC_API_URL}"]
