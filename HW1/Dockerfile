FROM openjdk:11-jre-slim as builder
WORKDIR application

ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} application.jar

RUN java $JAVA_OPTS -Djarmode=layertools -jar application.jar extract

FROM openjdk:11-jre-slim

WORKDIR application

COPY --from=builder application/dependencies/ ./
COPY --from=builder application/spring-boot-loader/ ./
COPY --from=builder application/snapshot-dependencies/ .
COPY --from=builder application/application/ .

RUN addgroup --system clinicgroup && adduser --system --group petclinic

USER petclinic:clinicgroup

EXPOSE 8080
ENTRYPOINT ["java", "org.springframework.boot.loader.JarLauncher"]
