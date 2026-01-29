FROM ubuntu:22.04

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Etc/UTC

RUN apt update && apt install -y \
wget \
git \
build-essential \
libffi-dev \
libtiff-dev \
python3 \
python3-pip \
python-is-python3 \
jq \
curl \
locales \
locales-all \
tzdata \
&& rm -rf /var/lib/apt/lists/*

# Download and install conda
RUN set -eux; \
  arch="$(uname -m)"; \
  case "$arch" in \
    x86_64) conda_arch="x86_64" ;; \
    aarch64) conda_arch="aarch64" ;; \
    *) echo "Unsupported architecture: $arch" && exit 1 ;; \
  esac; \
  wget "https://repo.anaconda.com/miniconda/Miniconda3-py311_23.11.0-2-Linux-${conda_arch}.sh" -O miniconda.sh; \
  bash miniconda.sh -b -p /opt/miniconda3; \
  rm miniconda.sh
# Add conda to PATH
ENV PATH=/opt/miniconda3/bin:$PATH
# Add conda to shell startup scripts like .bashrc (DO NOT REMOVE THIS)
RUN conda init --all
RUN conda config --append channels conda-forge

RUN adduser --disabled-password --gecos 'dog' nonroot


RUN cat <<'INLINE_SCRIPT' > /root/setup_env.sh
#!/bin/bash
set -euxo pipefail
source /opt/miniconda3/bin/activate
conda create -n testbed python=3.11  -y
conda activate testbed
INLINE_SCRIPT
RUN chmod +x /root/setup_env.sh
RUN sed -i -e 's/\r$//' /root/setup_env.sh
RUN chmod +x /root/setup_env.sh
RUN /bin/bash -c "source ~/.bashrc && /root/setup_env.sh"

WORKDIR /testbed/

# Automatically activate the testbed environment
RUN echo "source /opt/miniconda3/etc/profile.d/conda.sh && conda activate testbed" > /root/.bashrc

RUN cat <<'INLINE_SCRIPT' > /root/setup_repo.sh
#!/bin/bash
set -euxo pipefail
git clone -o origin https://github.com/deepset-ai/haystack /testbed
chmod -R 777 /testbed
cd /testbed
git reset --hard 71416c81bcda4bbe68ea6f2147d49888e37c7316
git remote remove origin
source /opt/miniconda3/bin/activate
conda activate testbed
echo "Current environment: $CONDA_DEFAULT_ENV"
apt-get update
apt-get install -y --no-install-recommends build-essential git curl openjdk-17-jre-headless jq ffmpeg gcc g++ libffi-dev libxml2-dev libxslt1-dev libjpeg-dev zlib1g-dev libmagic1 python3-dev libgomp1
rm -rf /var/lib/apt/lists/*
pip install --upgrade pip setuptools wheel
pip install uv
if [ -f pyproject.toml ]; then uv pip install -e .; fi
uv pip install tqdm 'tenacity!=8.4.0' lazy-imports 'openai>=1.56.1' pydantic Jinja2 'posthog!=3.12.0' pyyaml more-itertools networkx 'typing_extensions>=4.7' requests numpy python-dateutil jsonschema haystack-experimental
uv pip install pytest pytest-bdd pytest-cov pytest-asyncio pytest-rerunfailures coverage mypy pip pylint ipython
uv pip install pre-commit ruff trafilatura azure-ai-formrecognizer 'sentence-transformers>=3.0.0' transformers torch torchvision sentence-transformers langdetect 'nltk>=3.9.1' pandas tabulate structlog
uv pip install opentelemetry-sdk ddtrace docstring-parser 'httpx[http2]' azure-identity
uv pip install arrow openapi3 python-oxmsg pypdf python-docx python-pptx 'pdfminer.six' tika jsonref openpyxl h2
uv pip install 'openai-whisper>=20231106' openapi-llm markdown-it-py mdit_plain jq tiktoken huggingface_hub 'pydantic>=2.0'
uv pip install flaky
python -m spacy download en_core_web_sm || echo 'Spacy model download failed, but continuing...'
python -m nltk.downloader punkt wordnet stopwords averaged_perceptron_tagger punkt_tab || echo 'NLTK downloads completed'
printf "[pytest]\nminversion = 6.0\naddopts = --strict-markers -v\nmarkers =\n    unit: unit tests\n    integration: integration tests\n    flaky: mark tests as flaky (pytest-rerunfailures)\n    slow: slow tests\n" > pytest.ini
git config --global user.email setup@swebench.config
git config --global user.name SWE-bench
git commit --allow-empty -am SWE-bench
INLINE_SCRIPT
RUN chmod +x /root/setup_repo.sh
RUN sed -i -e 's/\r$//' /root/setup_repo.sh
RUN /bin/bash /root/setup_repo.sh

WORKDIR /testbed/