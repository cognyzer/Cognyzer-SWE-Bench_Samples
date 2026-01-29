FROM ubuntu:22.04
RUN apt-get update && apt-get install -y git build-essential && rm -rf /var/lib/apt/lists/*
WORKDIR /testbed
RUN git clone https://github.com/google/gson . && git checkout ae9604c201b3ba44babc48e982e012b1d23b69c8
WORKDIR /testbed