FROM eclipse-temurin:21-jdk AS base

# Set environment variables
ENV GRADLE_VERSION=8.6 \
    MAVEN_VERSION=3.9.6

# Install required packages
RUN apt-get update && apt-get install -y \
    curl \
    unzip \
    git \
    && rm -rf /var/lib/apt/lists/*

# Install Gradle
RUN curl -sL https://services.gradle.org/distributions/gradle-${GRADLE_VERSION}-bin.zip -o gradle.zip \
    && unzip gradle.zip -d /opt/ \
    && rm gradle.zip \
    && ln -s /opt/gradle-${GRADLE_VERSION}/bin/gradle /usr/local/bin/gradle

# Install Maven
RUN curl -fsSL https://downloads.apache.org/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz | tar -xz -C /opt/ \
    && ln -s /opt/apache-maven-${MAVEN_VERSION}/bin/mvn /usr/local/bin/mvn

# Verify installations
RUN java -version && gradle -v && mvn -v

WORKDIR /app
CMD ["/bin/bash"]
