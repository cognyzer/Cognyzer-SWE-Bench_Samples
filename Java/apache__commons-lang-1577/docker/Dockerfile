FROM maven:3.9-eclipse-temurin-17

RUN apt-get update && apt-get install -y git && rm -rf /var/lib/apt/lists/*

WORKDIR /testbed

RUN git clone --depth 100 https://github.com/apache/commons-lang . && \
    git checkout ac91ddd6ad1c09e0fadf0c6422bf04ba4aed37a9

RUN mvn dependency:resolve -B || true

WORKDIR /testbed