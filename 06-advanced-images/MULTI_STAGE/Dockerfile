FROM maven:3.6-alpine as build

COPY app /app

WORKDIR /app

RUN mvn package

FROM openjdk:8-alpine

COPY --from=build /app/target/my-app-1.0-SNAPSHOT.jar /app/app.jar

CMD ["java", "-jar", "/app/app.jar"]
