# Aeromexico MVP Java Pipeline

[![Build Status](https://jenkins-mvp.rax.latamps.tech/buildStatus/icon?job=java-mvn-quarkus-1%2Fdev)](https://jenkins-mvp.rax.latamps.tech/job/Kaniko-test-1/job/dev/)

Basado en OpenJDK - Quarkus, Hibernate y Panache. Prueba de DevOps. 

## RESTEasy JAX-RS

<p>Un endpoint RESTEasy básico para probar flujo de CI/CD</p>

Guide: https://quarkus.io/guides/rest-json

https://www.rackspace.com 

TOKEN 2:  e855f732e84feebee515e78a7511be8a1d5b9e6f

## Packaging and running the application

Se usará UBER-JAR en este caso.

The application can be packaged using:
```shell script
./mvnw package
```
It produces the `aemxpoc-jdk-pipeline-1.0.0-SNAPSHOT-runner.jar` file in the `/target` directory.
Be aware that it’s not an _über-jar_ as the dependencies are copied into the `target/lib` directory.

If you want to build an _über-jar_, execute the following command:
```shell script
./mvnw package -Dquarkus.package.type=uber-jar
```

The application is now runnable using `java -jar target/aemxpoc-jdk-pipeline-1.0.0-SNAPSHOT-runner.jar`.

## Creating a native executable

You can create a native executable using: 
```shell script
./mvnw package -Pnative
```

Or, if you don't have GraalVM installed, you can run the native executable build in a container using: 
```shell script
./mvnw package -Pnative -Dquarkus.native.container-build=true
```

You can then execute your native executable with: `./target/aemxpoc-jdk-pipeline-1.0.0-SNAPSHOT-runner`

If you want to learn more about building native executables, please consult https://quarkus.io/guides/maven-tooling.html.

## Apéndice Técnico

Code scanning:

```

./mvnw sonar:sonar \
               -Dsonar.projectKey=rax.latamps.tech:aemx-poc-2 \
               -Dsonar.host.url=http://aemx-sonarqube.sonarqube8:9000 \
               -Dsonar.verbose=true \
               -Dsonar.login=4fec561eb42f924a0514c525ddf38a4afb35e4e5

```
