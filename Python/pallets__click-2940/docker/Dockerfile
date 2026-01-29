FROM python:3.11-slim

WORKDIR /workspace

# Clone repository
RUN apt-get update && apt-get install -y git && \
    git clone https://github.com/pallets/click.git . && \
    git checkout 36deba8a95a2585de1a2aa4475b7f054f52830ac

# Install dependencies
RUN pip install --no-cache-dir -e .

# Install test dependencies
RUN pip install --no-cache-dir pytest pytest-cov

CMD ["pytest", "-xvs"]
