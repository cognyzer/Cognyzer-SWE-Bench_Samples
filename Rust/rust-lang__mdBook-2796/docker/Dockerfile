FROM rust:1.85.0

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Etc/UTC

RUN apt update && apt install -y \
wget \
git \
build-essential \
&& rm -rf /var/lib/apt/lists/*

RUN adduser --disabled-password --gecos 'dog' nonroot


RUN cat <<'INLINE_SCRIPT' > /root/setup_repo.sh
#!/bin/bash
set -euxo pipefail
git clone -o origin https://github.com/rust-lang/mdBook /testbed
chmod -R 777 /testbed
cd /testbed
git reset --hard 4bac54883b63f98b61db0826b7bf25d67adbf295
git remote remove origin
cargo build --workspace --all-features --release
INLINE_SCRIPT
RUN chmod +x /root/setup_repo.sh
RUN /bin/bash /root/setup_repo.sh

WORKDIR /testbed/