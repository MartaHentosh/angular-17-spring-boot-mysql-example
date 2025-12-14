# Stage 1: Build
FROM maven:3.9-eclipse-temurin-17 AS build
WORKDIR /workspace

COPY pom.xml .
RUN mvn -B -DskipTests dependency:go-offline

COPY src ./src
RUN mvn -B -DskipTests package

# Stage 2: Run
FROM eclipse-temurin:17-jre-alpine
WORKDIR /app

# copy the repackaged spring boot jar (exclude .jar.original)
COPY --from=build /workspace/target/*.jar /tmp/

RUN set -eux; \
    ls -la /tmp; \
    JAR="$(ls /tmp/*.jar | grep -v '\.jar\.original$' | head -n 1)"; \
    test -n "$JAR"; \
    cp "$JAR" /app/app.jar; \
    rm -rf /tmp/*.jar

EXPOSE 8080
ENTRYPOINT ["java","-jar","/app/app.jar"]
