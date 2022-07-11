# springboot-docker-gradle
The video is located here:

https://youtu.be/x7nrGLJLN5U


# ajuste para funcionar
build.gradle

plugins {
    id 'org.springframework.boot' version '2.5.3'
    id 'io.spring.dependency-management' version '1.0.11.RELEASE'
    id 'java'
    id 'com.palantir.docker' version '0.22.1'
}

docker {
    name "${rootProject.name}"
    dockerfile file('Dockerfile')
    copySpec.from(tasks.bootJar.outputs.files.singleFile).rename(".*","app.jar")
    buildArgs(['JAR_FILE': "app.jar"])
}


dockerfile

FROM adoptopenjdk/openjdk11:alpine
VOLUME /tmp
ARG JAR_FILE
COPY ${JAR_FILE} app.jar
ENTRYPOINT ["java", "-Dspring.profiles.active=local", "-jar", "app.jar"]
EXPOSE 8082
