FROM python:3.11-slim

WORKDIR /workspace

# Clone repository
RUN apt-get update && apt-get install -y git && \
    git clone https://github.com/pallets/click.git . && \
    git checkout a1235aacb1be55dc66ddcfefbf64dec44b6ab54d

# Install dependencies
RUN pip install --no-cache-dir -e .

# Install test dependencies
RUN pip install --no-cache-dir pytest pytest-cov

CMD ["pytest", "-xvs"]
