# ---------- Build stage ----------
FROM maven:3.8.6-eclipse-temurin-17 as build
WORKDIR /app

# copy only what is needed to build (speeds up docker build)
COPY pom.xml mvnw ./
COPY .mvn .mvn
COPY src ./src

# give executable permission to mvnw if present
RUN ["chmod", "+x", "./mvnw"]

# build the jar (skip tests for faster builds)
RUN ./mvnw -B -DskipTests clean package

# ---------- Runtime stage ----------
FROM eclipse-temurin:17-jre-jammy
WORKDIR /app

# copy jar from build stage
COPY --from=build /app/target /app/target

# expose default Spring Boot port
EXPOSE 8080

# run the jar (use sh -c so wildcard expansion works)
CMD ["sh", "-c", "java -jar target/*.jar"]
