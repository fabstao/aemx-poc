####
# This Dockerfile is used in order to build a container that runs the Quarkus application in JVM mode
#
# Before building the container image run:
#
# ./mvnw package -Dquarkus.package.type=uber-jar
#
# Then, build the image with:
#
# docker build -f src/main/docker/Dockerfile.uber-jar -t quarkus/aemxpoc-jdk-pipeline-uber-jar .
#
# Then run the container using:
#
# docker run -i --rm -p 8080:8080 quarkus/aemxpoc-jdk-pipeline-uber-jar
#
# If you want to include the debug port into your docker image
# you will have to expose the debug port (default 5005) like this :  EXPOSE 8080 5050
#
# Then run the container using :
#
# docker run -i --rm -p 8080:8080 -p 5005:5005 -e JAVA_ENABLE_DEBUG="true" quarkus/aemxpoc-jdk-pipeline-fast-jar
#
###
FROM registry.access.redhat.com/ubi8/ubi-minimal:latest 

ARG JAVA_PACKAGE=java-11-openjdk-headless
ARG RUN_JAVA_VERSION=1.3.8
ENV LANG='en_US.UTF-8' LANGUAGE='en_US:en'
# Install java and the run-java script
# Also set up permissions for user `1001`
RUN microdnf install curl ca-certificates ${JAVA_PACKAGE} \
    && microdnf update \
    && microdnf clean all \
    && mkdir /deployments \
    && chown 1000 /deployments \
    && chmod "g+rwX" /deployments \
    && chown 1000:root /deployments \
    && echo "securerandom.source=file:/dev/urandom" >> /etc/alternatives/jre/lib/security/java.security

# Configure the JAVA_OPTIONS, you can add -XshowSettings:vm to also display the heap size.
ENV JAVA_OPTIONS="-Dquarkus.http.host=0.0.0.0 -Djava.util.logging.manager=org.jboss.logmanager.LogManager"
# We make four distinct layers so if there are application changes the library layers can be re-used
#COPY --chown=1001 target/quarkus-app/lib/ /deployments/lib/
COPY target/aemx-poc-2-1.0.0-SNAPSHOT-runner.jar /deployments/
RUN chown -R 1000 /deployments

EXPOSE 8788
USER 1000

ENTRYPOINT [ "java", "-jar", "/deployments/aemx-poc-2-1.0.0-SNAPSHOT-runner.jar" ]
