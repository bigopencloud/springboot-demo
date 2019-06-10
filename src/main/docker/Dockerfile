FROM frolvlad/alpine-java:jdk8-slim AS builder
VOLUME /tmp
ADD demo-0.0.1-SNAPSHOT.jar demo.jar
RUN sh -c 'touch /demo.jar'
ENTRYPOINT ["java","-jar","/demo.jar"]
