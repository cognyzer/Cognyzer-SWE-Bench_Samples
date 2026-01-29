FROM ubuntu:22.04

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Etc/UTC

RUN apt update && apt install -y \
wget \
git \
build-essential \
&& rm -rf /var/lib/apt/lists/*

# Install go. Based on https://github.com/docker-library/golang/blob/7ba64590f6cd1268b3604329ac28e5fd7400ca79/1.24/bookworm/Dockerfile
RUN set -eux; \
	now="$(date '+%s')"; \
	arch="$(dpkg --print-architecture)"; \
	url=; \
	case "$arch" in \
		'amd64') \
			url='https://dl.google.com/go/go1.23.6.linux-amd64.tar.gz'; \
			;; \
		'armhf') \
			url='https://dl.google.com/go/go1.23.6.linux-armv6l.tar.gz'; \
			;; \
		'arm64') \
			url='https://dl.google.com/go/go1.23.6.linux-arm64.tar.gz'; \
			;; \
		'i386') \
			url='https://dl.google.com/go/go1.23.6.linux-386.tar.gz'; \
			;; \
		'mips64el') \
			url='https://dl.google.com/go/go1.23.6.linux-mips64le.tar.gz'; \
			;; \
		'ppc64el') \
			url='https://dl.google.com/go/go1.23.6.linux-ppc64le.tar.gz'; \
			;; \
		'riscv64') \
			url='https://dl.google.com/go/go1.23.6.linux-riscv64.tar.gz'; \
			;; \
		's390x') \
			url='https://dl.google.com/go/go1.23.6.linux-s390x.tar.gz'; \
			;; \
		*) echo >&2 "error: unsupported architecture '$arch' (likely packaging update needed)"; exit 1 ;; \
	esac; \
	\
	wget -O go.tgz "$url" --progress=dot:giga; \
    tar -C /usr/local -xzf go.tgz; \
    rm go.tgz;
ENV PATH=/usr/local/go/bin:$PATH
RUN go version

RUN adduser --disabled-password --gecos 'dog' nonroot


RUN cat <<'INLINE_SCRIPT' > /root/setup_repo.sh
#!/bin/bash
set -euxo pipefail
git clone -o origin https://github.com/open-policy-agent/opa /testbed
chmod -R 777 /testbed
cd /testbed
git reset --hard 64ebe3588a58f8c39525db2d3fee23ad96ac81c1
git remote remove origin
go mod download
go build ./...
INLINE_SCRIPT
RUN chmod +x /root/setup_repo.sh
RUN /bin/bash /root/setup_repo.sh

WORKDIR /testbed/