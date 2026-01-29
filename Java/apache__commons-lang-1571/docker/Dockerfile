FROM maven:3.9-eclipse-temurin-17

RUN apt-get update && apt-get install -y git && rm -rf /var/lib/apt/lists/*

WORKDIR /testbed

RUN git clone --depth 100 https://github.com/apache/commons-lang . && \
    git checkout 67f50236bf337e20c6daa987d4d42c4fedb482ce

RUN mvn dependency:resolve -B || true

WORKDIR /testbed