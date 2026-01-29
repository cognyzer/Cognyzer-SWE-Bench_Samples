FROM gcc:13

RUN apt-get update && apt-get install -y git cmake ninja-build && rm -rf /var/lib/apt/lists/*

WORKDIR /testbed

RUN git clone --depth 100 https://github.com/nlohmann/json . && \
    git checkout 457bc283ffb3dc4faffab25734af5661506efff0

RUN mkdir -p build && cd build && cmake -G Ninja .. || true

WORKDIR /testbed