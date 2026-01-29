FROM gcc:13

RUN apt-get update && apt-get install -y git cmake ninja-build && rm -rf /var/lib/apt/lists/*

WORKDIR /testbed

RUN git clone --depth 100 https://github.com/nlohmann/json . && \
    git checkout 8f7570014107c5040103c5c4f9a55250be9add0c

RUN mkdir -p build && cd build && cmake -G Ninja .. || true

WORKDIR /testbed