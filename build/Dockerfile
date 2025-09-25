# Jupyter Notebook (latest) + latest OpenAI SDK, for llama.cpp server
FROM python:3.11-slim

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PIP_NO_CACHE_DIR=1

WORKDIR /workspace

# (Optional) curl to poke the llama.cpp server
RUN apt-get update && apt-get install -y --no-install-recommends curl \
  && rm -rf /var/lib/apt/lists/*

# Always install latest releases
RUN pip install --no-cache-dir --upgrade pip \
 && pip install --no-cache-dir notebook openai requests autogen-agentchat autogen-ext[openai]

# Safer defaults: non-root
RUN useradd -ms /bin/bash appuser
USER appuser

# Defaults (override at runtime)
ENV OPENAI_API_KEY=sk-noauth \
    OPENAI_BASE_URL=http://host.docker.internal:8000/v1 \
    OPENAI_MODEL=llama

EXPOSE 8888

# No auth for local/dev; set a token/password in prod
CMD ["bash", "-lc", "jupyter notebook --ip=0.0.0.0 --port=8888 --no-browser --NotebookApp.token='' --NotebookApp.password=''"]