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
git clone -o origin https://github.com/rust-lang/cargo /testbed
chmod -R 777 /testbed
cd /testbed
git reset --hard 2560340ed1cf838394e766e3652e15001990a73e
git remote remove origin
env RUSTC_BOOTSTRAP=1 cargo build --workspace --locked
INLINE_SCRIPT
RUN chmod +x /root/setup_repo.sh
RUN /bin/bash /root/setup_repo.sh

WORKDIR /testbed/