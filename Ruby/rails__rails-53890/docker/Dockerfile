FROM ruby:3.2.4

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Etc/UTC

RUN apt update && apt install -y \
wget \
git \
build-essential \
jq \
&& rm -rf /var/lib/apt/lists/*

RUN adduser --disabled-password --gecos 'dog' nonroot


RUN cat <<'INLINE_SCRIPT' > /root/setup_repo.sh
#!/bin/bash
set -euxo pipefail
git clone -o origin https://github.com/rails/rails /testbed
chmod -R 777 /testbed
cd /testbed
git reset --hard ab3b87e536fc27004629aa5f32bdbe5353626111
git remote remove origin
apt-get update -qq && apt-get install -y git build-essential libsqlite3-dev libpq-dev default-libmysqlclient-dev curl && rm -rf /var/lib/apt/lists/*
gem install bundler -v 2.5.17
bundle config set --local path 'vendor/bundle'
bundle install --jobs 4
INLINE_SCRIPT
RUN chmod +x /root/setup_repo.sh
RUN /bin/bash /root/setup_repo.sh

WORKDIR /testbed/