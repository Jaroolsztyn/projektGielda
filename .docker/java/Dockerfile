FROM ubuntu:16.04
MAINTAINER Hari Sekhon (https://www.linkedin.com/in/harisekhon)

LABEL Description="Java + Ubuntu (OpenJDK)"

ENV DEBIAN_FRONTEND noninteractive

ARG JAVA_VERSION=17
ARG JAVA_RELEASE=JDK

ENV JAVA_HOME=/usr

RUN bash -c ' \
    set -euxo pipefail && \
    apt-get update --fix-missing && \
    pkg="openjdk-$JAVA_VERSION"; \
    if [ "$JAVA_RELEASE" = "JDK" ]; then \
        pkg="$pkg-jdk-headless"; \
    else \
        pkg="$pkg-jre-headless"; \
    fi; \
    apt-get install -y --no-install-recommends wget unzip "$pkg" && \
    apt-get clean'

CMD /bin/bash

# Install Midnight Commander, Nano, sudo
RUN apt-get install -y nano sudo

#install Gradle
RUN wget -q https://services.gradle.org/distributions/gradle-4.5.1-bin.zip \
    && unzip gradle-4.5.1-bin.zip -d /opt \
    && rm gradle-4.5.1-bin.zip

# Install flyway
WORKDIR /flyway
ENV FLYWAY_VERSION 5.1.4

RUN wget https://repo1.maven.org/maven2/org/flywaydb/flyway-commandline/${FLYWAY_VERSION}/flyway-commandline-${FLYWAY_VERSION}.tar.gz \
  && tar -xzf flyway-commandline-${FLYWAY_VERSION}.tar.gz \
  && mv flyway-${FLYWAY_VERSION}/* . \
  && rm flyway-commandline-${FLYWAY_VERSION}.tar.gz \
  && sed -i 's/bash/sh/' /flyway/flyway \
  && ln -s /flyway/flyway /usr/local/bin/flyway

WORKDIR /home/ec2-user/offer-analyzer

# Set Gradle in the environment variables
ENV GRADLE_HOME /opt/gradle-4.5.1
ENV PATH $PATH:/opt/gradle-4.5.1/bin

#RUN ls /home/ec2-user/offer-analyzer
#
#WORKDIR /home/ec2-user/offer-analyzer

#RUN gradle fatJar
#RUN java -jar OfferCrawler/build/libs/OfferCrawler-all.jar
